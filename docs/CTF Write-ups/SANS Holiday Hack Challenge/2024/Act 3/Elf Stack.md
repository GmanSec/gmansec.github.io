<div style="text-align: center; font-size: 2em; font-weight: bold; color: black; margin-bottom: 20px;">
    Elf Stack - Fitzy Shortstack
</div>

<style>
div.conversation {
  background-color: #f4f4f4;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 15px;
  margin: 20px 0;
  line-height: 1.6;
  white-space: pre-wrap; /* Ensures text wraps properly */
  font-family: Arial, sans-serif;
}
</style>

Our host today is Fitzy Shortstack, and this is what he has to tell us:

![Fitzy Shortstack Conversation](../../../../images/HHC/2024/Act%203/Elf%20Stack/Fitzy%20Shortstack-1.png)

We get the following hints after our chat with Fitzy:

##Hint 1 - Elf Stack Intro

I'm part of the ElfSOC that protects the interests here at the North Pole. We built the Elf Stack SIEM, but not everybody uses it. Some of our senior analysts choose to use their command line skills, while others choose to deploy their own solution. Any way is possible to hunt through our logs!

##Hint 2 - Elf Stack Fields

If you are using your command line skills to solve the challenge, you might need to review the configuration files from the containerized Elf Stack SIEM.

##Hint 3 - Elf Stack WinEvent

One of our seasoned ElfSOC analysts told me about a great resource to have handy when hunting through event log data. I have it around here somewhere, or maybe it was online. Hmm.

##Hint 4 - Elf Stack PowerShell

Our Elf Stack SIEM has some minor issues when parsing log data that we still need to figure out. Our ElfSOC SIEM engineers drank many cups of hot chocolate figuring out the right parsing logic. The engineers wanted to ensure that our junior analysts had a solid platform to hunt through log data.

##Hint 5 - Elf Stack Hard - Email1
I was on my way to grab a cup of hot chocolate the other day when I overheard the reindeer talking about playing games. The reindeer mentioned trying to invite Wombley and Alabaster to their games. This may or may not be great news. All I know is, the reindeer better create formal invitations to send to both Wombley and Alabaster.

##Hint 6 - Elf Stack Hard - Email2

Some elves have tried to make tweaks to the Elf Stack log parsing logic, but only a seasoned SIEM engineer or analyst may find that task useful.

##Access to the Challenge - Environment set up

Upon accessing the challenge, we get the following instructions:

![Instructions](../../../../images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-1.png)

If we close the initial window and click “Help”, the following text is displayed:

<div style="font-family: Arial, sans-serif; line-height: 1.6; padding: 20px; border: 1px solid #ccc; border-radius: 10px; background-color: #f9f9f9;">
  <h2 style="color: #2c3e50;">Elf Stack Help</h2>
  <h3 style="color: #34495e;">Description</h3>
  <p>
    Help the ElfSOC analysts and test your technical and security skills while you investigate a malicious attack. You will parse a set of log files to identify the malicious attack vector and various events within an attack chain.
  </p>
  <p>
    The log parsing skills emphasized by this challenge can be done with the provided containerized Elf Stack SIEM, through traditional Linux CLI tools, or however you want. There are two challenge modes (<strong>EASY</strong> and <strong>HARD</strong>) which determine the difficulty of questions presented to you.
  </p>
  <p>
    <strong>Note:</strong> You do not have to use the containerized SIEM to solve any of the questions, but it might make things a bit easier! You can download the containerized Elf Stack SIEM configuration and log files by selecting the "Download" button.
  </p>

  <h3 style="color: #34495e;">Challenge Modes</h3>
  <h4 style="color: #2c3e50;">Easy Mode</h4>
  <p>
    This mode teaches basic log parsing with a common SIEM utility like the ELK stack. You are provided a Docker Compose configuration that allows you to fully setup an ELK stack within a containerized environment.
  </p>
  <p>
    Select this mode if you are new to security, or you want to continue on with the story. This mode is meant to help you learn while doing.
  </p>

  <h4 style="color: #2c3e50;">Hard Mode</h4>
  <p>
    This mode expects you have some knowledge on parsing log files. The attack path is more complex and you will need to research how to identify the steps within the attack chain.
  </p>
  <p>
    Select this mode if you want to challenge your security skills.
  </p>

  <h3 style="color: #34495e;">Optional: Elf Stack SIEM</h3>
  <h4 style="color: #2c3e50;">Containerized ELK Stack</h4>
  <p>
    The containerized SIEM is provided to assist in solving the challenge. The SIEM is fully functional and configured to ingest the provided log files.
  </p>
  <p>
    <strong>Note:</strong> This was built and tested on an Ubuntu 22.04 Linux virtual machine.
  </p>

  <h4 style="color: #2c3e50;">Containerized ELK Stack: Prerequisites</h4>
  <ul>
    <li>RAM: Minimum 16GB</li>
    <li>CPU Cores: Minimum 4</li>
    <li>Hard drive space: Minimum 30GB / Recommended 40GB</li>
    <li>A network interface with internet connectivity</li>
  </ul>
  <p>
    Follow the instructions on <a href="https://www.docker.com/">Docker Installation Guide</a> to install Docker on your respective platform.
  </p>
  <p>
    After installation of Docker, download the Elf Stack files from the download page. Unzip/Extract the containerized files into a single directory.
  </p>

  <h4 style="color: #2c3e50;">Containerized ELK Stack: Setup</h4>
  <p><strong>Note:</strong> Setup time depends on internet connectivity and system resources.</p>
  <ol>
    <li>
      Browse to the directory containing the container SIEM files and setup the environment. Time to complete ~2-10 minutes:<br>
      <code>docker compose up setup</code>
    </li>
    <li>
      Run the ELF Stack SIEM to automatically ingest the logs. Time to complete ~20-30 minutes:<br>
      <code>docker compose up</code>
    </li>
  </ol>
  <p>
    The terminal output will display:
    <ul>
      <li>When logs ingestion is complete.</li>
      <li>The login URL and credentials.</li>
    </ul>
  </p>
  <p>
    <strong>Note:</strong> After your Elf Stack SIEM is setup, ensure you set the timeframe to look at events in 2024.
  </p>

  <h4 style="color: #2c3e50;">Containerized ELK Stack: Teardown</h4>
  <p>
    Use the following command to shut down the Elf Stack SIEM:<br>
    <code>docker compose down --volumes</code>
  </p>
  <p>
    <strong>Note:</strong> The command requires the <code>--volumes</code> syntax to clear the volume data which may skew your results if you set the stack up again later.
  </p>
</div>

If we click download, we get the following files:

https://hhc24-elfstack.holidayhackchallenge.com/download_file/log_chunk_2.log.zip<br>
https://hhc24-elfstack.holidayhackchallenge.com/download_file/elf-stack-siem-with-logs.zip<br>
https://hhc24-elfstack.holidayhackchallenge.com/download_file/log_chunk_1.log.zip

Following the instructions, I downloaded Ubuntu 22.04 and created a VM with 16GB of RAM and 80GB HDD space, with 4 cores and 2 threads per core.

To install docker in Ubuntu, I followed the official guide provided at the Docker site. Below are the commands I executed in the terminal:

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

To test it works as expected, you can execute the following:

```
sudo docker run hello-world
```
If all works as expected, we can run the container provided. Go to the "elf-stack-siem" folder and execute:
```
sudo docker compose up setup
```
Once the set up is completed, execute:
```
sudo docker compose up
```
If all goes well, you will eventually see the below:

![ELK Booting](../../../../images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-7.png)

Grab a coffee because the log ingestion will take a while... once completed, this is what you will see:

![ELK Ready](../../../../images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-8.png)

Now we can access ELK at `http://localhost:5601/`
 
Username: elastic Password: ELFstackLogin!

Once you log in, to do a search, on the left, click "Discover". Then make sure you set the timeframe. I used from 1 January 2024 till now:

![Timeframe](../../../../images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-10.png)

#**Solution - Silver (Easy Mode)**

**Note**: I used ES/QL languate in Elastic for the queries.

###Question 1

**How many unique values are there for the event_source field in all logs?**

While waiting for the set up to get ready, I started solving questions with the command line. Below is a truncated example of a log line:

```
log_chunk_1.log:<134>1 2024-09-15T01:35:01-04:00 SleighRider.northpole.local WindowsEvent - - - {"EventTime": "2024-09-15T01:35:01.000000Z", "Hostname": "SleighRider.northpole.local", "Keywords": -9223372036854775808, "EventType": "AUDIT_SUCCESS", ..."}
```

In this example, the `event_source` is `WindowsEvent`. We can use awk to extract that specific column and count the unique values:

```
cat log_chunk_1.log log_chunk_2.log | awk -F' ' '{print $4}' | sort -u | wc -l
```
**Using ES/QL:**

We can do the following query:

```
from logs-generic-default | stats count () by event_source
```
Or refine even further and get only the final number as result:
```
from logs-generic-default | stats count_distinct(event_source)
```

Answer: **5**

###Question 2
**Which event_source has the fewest number of events related to it?**

```
cat log_chunk_1.log log_chunk_2.log | awk -F' ' '{print $4}' | sort | uniq -c | sort -n | head -n 1
```

The output of that command is: `269 AuthLog`

Answer: **AuthLog**

###Question 3
**Using the event_source from the previous question as a filter, what is the field name that contains the name of the system the log event originated from?**

For this one, I simply checked one log as example:

```
cat log_chunk_1.log log_chunk_2.log | grep "AuthLog" | less

<134>1 2024-09-15T03:25:01-04:00 kringleSSleigH AuthLog - - - {"timestamp": "2024-09-15T06:25:01.801358-04:00", "hostname": "kringleSSleigH", "service": "CRON[5032]:", "message": "pam_unix(cron:session): session closed for user root"}
```

Answer: **hostname**

###Question 4
**Which event_source has the second highest number of events related to it?**
```
cat log_chunk_1.log log_chunk_2.log | awk -F' ' '{print $4}' | sort | uniq -c | sort -n              
    269 AuthLog
   1398 SnowGlowMailPxy
   7476 GreenCoat
  34679 NetflowPmacct
2299324 WindowsEvent
```

Answer: **NetflowPmacct**

###Question 5
**Using the event_source from the previous question as a filter, what is the name of the field that defines the destination port of the Netflow logs?**

```
cat log_chunk_1.log log_chunk_2.log | grep "NetflowPmacct" | less


<134>1 2024-09-15T10:37:43-04:00 kringleconnect NetflowPmacct - - - {"event_type": "purge", "ip_src": "172.24.25.25", "ip_dst": "142.250.191.166", "port_src": 51858, "port_dst": 443, "ip_proto": "tcp", "timestamp_start": "2024-09-15T10:37:43-04:00", "timestamp_end": "0000-00-00T00:00:00-00:00", "packets": 1, "bytes": 79, "src_host": "", "dst_host": ""}
```

Answer: **port_dst**

###Question 6
**Which event_source is related to email traffic?**

From the names of the event_source fields, likely to be SnowGlowMailPxy, but let's check some examples to confirm:

```
cat log_chunk_1.log log_chunk_2.log | grep "SnowGlowMailPxy" | less


<134>1 2024-09-15T08:37:41-04:00 SecureElfGwy SnowGlowMailPxy - - - {"From": "elf_user02@northpole.local", "To": "wcub101@northpole.local", "Subject": "Meeting Schedules Update", "Date": null, "Message-ID": "<96D21546-EC3F-4BFE-9358-C7CE74705F83@SecureElfGwy.northpole.local>", "Return-Path": "elf_user02@northpole.local", "Body": "Dear wcub101,\n\nI hope this email finds you in high spirits. I am reaching out to provide you with an update regarding the upcoming meeting schedules within our organization.\n\nFirstly, I want to highlight an adjustment in the date and time of the weekly team meeting. Based on the feedback received and to accommodate everyone's availability, we have decided to shift the meeting from its current slot on Mondays at 9 AM to Tuesdays at 10 AM. This change will be implemented starting next week, and I kindly request that you adjust your calendars accordingly.\n\nAdditionally, I would like to remind you about the monthly departmental meeting scheduled for this Thursday at 3 PM. The purpose of this meeting is to discuss the progress on ongoing projects, address any concerns, and share updates on overall company performance. We highly encourage your active participation to ensure effective communication across all teams.\n\nFurthermore, the bi-weekly cross-departmental meeting, where representatives from different teams come together to collaborate and share insights, will take place on Friday at 2 PM. This meeting aims to foster a sense of cohesion and synergy among departments, enabling us to work seamlessly towards our common goals.\n\nLastly, for those involved in the client acquisition project, there will be a special client meeting scheduled for next Monday at 11 AM. This meeting will serve to review the progress made, discuss potential strategies, and address any client-related queries or issues. Kindly come prepared with the necessary documentation and key insights, as it is crucial that we maintain a high level of professionalism during these interactions.\n\nIf you have any conflicts with the schedules mentioned above or require any adjustments, please let me know at your earliest convenience. We understand the importance of ensuring the availability and participation of all team members and will strive to accommodate any legitimate conflicts whenever possible.\n\nThank you for your continued dedication and contribution to the North Pole. Should you have any questions or concerns regarding the meetings or any other matter, please do not hesitate to reach out to me.\n\nLooking forward to fruitful discussions and collaborations in the meetings ahead.\n\nBest regards,\n\nelf_user02\n", "Received_Time": "2024-09-15T08:37:41-04:00", "ReceivedIP1": "172.24.25.25", "ReceivedIP2": "172.24.25.20"}
```

Answer: **SnowGlowMailPxy**

###Question 7
**Looking at the event source from the last question, what is the name of the field that contains the actual email text?**

Using the same example as for question 6, we can see that the answer is "Body".

Answer: **Body**

###Question 8
**Using the 'GreenCoat' event_source, what is the only value in the hostname field?**

Example log:

![GreenCoat Example](../../../../images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-9.png)

```
grep "GreenCoat" log_chunk_* | awk -F' ' '{print $3}' | sort -u
```
Answer: **SecureElfGwy**

### Question 9
**Using the 'GreenCoat' event_source, what is the name of the field that contains the site visited by a client in the network?**

Logs from GreenCoat look like this:

```
log_chunk_1.log:<134>1 2024-09-15T06:00:51-04:00 SecureElfGwy GreenCoat - - - {"ip": "172.24.25.93", "user_identifier": "elf_user03", "timestamp": "2024-09-15T06:00:51-04:00", "method": "CONNECT", "url": "disc601.prod.do.dsp.mp.microsoft.com:443", "http_protocol": "HTTP/1.1", "status_code": 200, "response_size": 0, "protocol": "HTTPS", "additional_info": "outgoing via 172.24.25.25", "host": "SnowSentry"}
```

We can also use the following ELK query to get example logs:

```
from logs-generic-default
| where event_source == "GreenCoat"
```

Answer: **url**

###Question 10
**Using the 'GreenCoat' event_source, which unique URL and port (URL:port) did clients in the TinselStream network visit most?**

```
from logs-generic-default
| where event_source == "GreenCoat" | stats count() by (event.url) | sort `count()` DESC
```
<div style="text-align: center;">
  <img src="/images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-12.png" alt="GreenCoat Most Visited URL" border-radius: 10px;">
  <p><em>GreenCoat Most Visited URL</em></p>
</div>

Answer: **pagead2.googlesyndication.com:443**

###Question 11
**Using the 'WindowsEvent' event_source, how many unique Channels is the SIEM receiving Windows event logs from?**

```
from logs-generic-default
| where event_source == "WindowsEvent" | STATS COUNT_DISTINCT(event.Channel)
```

Answer: **5**

###Question 12
**What is the name of the event.Channel (or Channel) with the second highest number of events?**

```
from logs-generic-default
| where event_source == "WindowsEvent" | stats count() by (event.Channel) | sort `count()` DESC
```
Answer: **Microsoft-Windows-Sysmon/Operational**

###Question 13
**Our environment is using Sysmon to track many different events on Windows systems. What is the Sysmon Event ID related to loading of a driver?**

According to Google, it is Event 6 (Driver Loaded).

Answer: **6**

###Question 14
**What is the Windows event ID that is recorded when a new service is installed on a system?**

Again Google: Event ID 4697.

Answer: **4697**

###Question 15
**Using the WindowsEvent event_source as your initial filter, how many user accounts were created?**

```
from logs-generic-default
| where event_source == "WindowsEvent" and event.EventID == 4720
```
There's no results.

Answer: **0**

That's all that was needed for Silver.

#**Solution - Gold (Hard Mode)**

###Question 1
**What is the event.EventID number for Sysmon event logs relating to process creation?**

From google: Event ID 1 - Description: "Process creation"

Answer: 1

###Question 2
**How many unique values are there for the 'event_source' field in all of the logs?**

Same as Q1 Silver.

```
from logs-generic-default | stats count_distinct(event_source)
```
Answer: **5**

###Question 3
**What is the event_source name that contains the email logs?**

Same as Q6 Silver.

Answer: **SnowGlowMailPxy**

###Question 4
**The North Pole network was compromised recently through a sophisticated phishing attack sent to one of our elves. The attacker found a way to bypass the middleware that prevented phishing emails from getting to North Pole elves. As a result, one of the Received IPs will likely be different from what most email logs contain. Find the email log in question and submit the value in the event 'From:' field for this email log event.**

The first thing I did is to review the log fields using:

```
from logs-generic-default
| where event_source == "SnowGlowMailPxy"
```
I noticed that there's 2 Received IP fields: `event.ReceivedIP1` and `event.ReceivedIP2`

I did a count of the IPs for each field:
```
from logs-generic-default
| where event_source == "SnowGlowMailPxy" | STATS COUNT() by (event.ReceivedIP1)
```
Only one IP in the result from the above query: 172.24.25.25
```
from logs-generic-default
| where event_source == "SnowGlowMailPxy" | STATS COUNT() by (event.ReceivedIP2)
```
2 results:
172.24.25.20
34.30.110.62

The second IP has only 1 hit, so that's the one we are interested in. Let's search the event where that IP is present:
```
from logs-generic-default
| where event_source == "SnowGlowMailPxy" and event.ReceivedIP2 == "34.30.110.62"
```

The data from the email body is interesting:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
We need to store the updated naughty and nice list somewhere secure. I posted it here http://hollyhaven.snowflake/howtosavexmas.zip. Act quickly so I can remove the link from the internet! I encrypted it with the password: n&nli$t_finAl1\n\nthx!\nkris\n- Sent from the sleigh. Please excuse any Ho Ho Ho's.
</pre>

But the answer to the question is in the 'From' field: kriskring1e@northpole.local 

Answer: **kriskring1e@northpole.local**

###Question 5
**Our ElfSOC analysts need your help identifying the hostname of the domain computer that established a connection to the attacker after receiving the phishing email from the previous question. You can take a look at our GreenCoat proxy logs as an event source. Since it is a domain computer, we only need the hostname, not the fully qualified domain name (FQDN) of the system.**

The body of the email mentions the following URL: `hollyhaven.snowflake`. Let's check the proxy for any connections towards that URL:

```
from logs-generic-default
| where event_source == "GreenCoat" and event.url like "*hollyhaven.snowflake*"
```
There's only one result (log below truncated):
```
{
  "@timestamp": "2024-09-15T14:36:26.000Z",
  "data_stream.type": "logs",
  "event.additional_info": "outgoing via 172.24.25.25",
  "event.host": "SleighRider",
  "event.http_protocol": "HTTP/1.1",
  "event.ip": "172.24.25.12",
  "event.method": "GET",
  "event.status_code": 200,
  "event.url": "http://hollyhaven.snowflake/howtosavexmas.zip",
  "event.user_identifier": "elf_user02",
  "event_source": "GreenCoat",
  "host.ip": "172.18.0.5",
  "hostname": "SecureElfGwy",
}
```

Answer: **SleighRider**

###Question 6
**What was the IP address of the system you found in the previous question?**

Checking the data in the same search result as for Question 5:

Answer: **172.24.25.12**

###Question 7
**A process was launched when the user executed the program AFTER they downloaded it. What was that Process ID number (digits only please)?**

Step 1 is to establish when the file was downloaded. This can be done by using event ID 11 (File Creation):
```
from logs-generic-default
| where event_source == "WindowsEvent" and event.Hostname == "SleighRider.northpole.local" and event.EventID == 11 and event.TargetFilename like "*howtosavexmas.zip*"
```
The following is the truncated result showing interesting information:
```
{
  "@timestamp": "2024-09-15T14:36:55.000Z",
  "event.Category": "File created (rule: FileCreate)",
  "event.Channel": "Microsoft-Windows-Sysmon/Operational",
  "event.CreationUtcTime": "2024-09-15T10:36:55-04:00",
  "event.EventID": 11,
  "event.Hostname": "SleighRider.northpole.local",
  "event.Image": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe",
  "event.TargetFilename": "C:\\Users\\elf_user02\\Downloads\\howtosavexmas.zip:Zone.Identifier",
  "event.User": "NORTHPOLE\\elf_user02",
  "event_source": "WindowsEvent",
  "host.ip": "172.18.0.5",
}
```
We can see that the user downloaded the zip file. The next thing I did is to do this query for file creation around the same time frame:
```
from logs-generic-default
| where event_source == "WindowsEvent" and event.Hostname == "SleighRider.northpole.local" and event.EventID == 1 
```

I also used the help of grep: 
```
grep "SleighRider" log_chunk_* | grep -i '"Hostname": "SleighRider.northpole.local"' | grep howtosavexmas
```

Truncated log:
```
  "@timestamp": "2024-09-15T14:36:36.000Z",
  "event.CommandLine": "\"C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe\" --single-argument http://hollyhaven.snowflake/howtosavexmas.zip",
  "event.EventID": 1,
  "event.Hostname": "SleighRider.northpole.local",
  "event.Image": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe",
  "event.ParentUser": "NORTHPOLE\\elf_user02",
  "event.ProcessGuid": "{face0b26-4224-660c-d80f-000000000700}",
  "event.ProcessID": 10014,
  "event.ProcessId": 5272,
  "event.Product": "Google Chrome",
}
```
Answer: **10014**

For this question, I ended up doing a lot of unnecessary queries for the answer, but the results to those queries gave me useful information to better understand the attack. For example:

```
from logs-generic-default
| where event_source == "WindowsEvent" and event.Hostname == "SleighRider.northpole.local" and event.EventID == 11
```
**Note**: for the search time frame I used "2024-09-15T14:36:55.000Z" as start date (the time at which the file was downloaded).

The reason I did that query is that the downloaded file was a .zip, so my thought process was that likely another file was created when it was unzipped. And sure enough, I saw the following interesting log:
```
{
  "@timestamp": "2024-09-15T14:37:09.000Z",
  "event.CreationUtcTime": "2024-09-15T10:37:09-04:00",
  "event.EventID": 11,
  "event.TargetFilename": "C:\\Users\\elf_user02\\Downloads\\howtosavexmas\\howtosavexmas.pdf.exe",
  "event.User": "NORTHPOLE\\elf_user02",
  "host.ip": "172.18.0.5",
  "hostname": "SleighRider.northpole.local",
}
```
So it looks like ' howtosavexmas.pdf.exe' is the malicious file we are interested in. Now we can check when a process was created using this file by searching for event id 4688 (a new process has been created) and after the time stamp in the log above:
```
from logs-generic-default
| where event_source == "WindowsEvent" and event.Hostname == "SleighRider.northpole.local" and event.EventID == 4688 and event.NewProcessName like "*howtosavexmas*"
```
Truncated log:
```
{
  "@timestamp": "2024-09-15T14:37:50.000Z",
  "event.Category": "Process Creation",
  "event.Channel": "Security",
  "event.CreatorSubject_AccountDomain": "NORTHPOLE",
  "event.CreatorSubject_AccountName": "SLEIGHRIDER$",
  "event.EventID": 4688,
  "event.Hostname": "SleighRider.northpole.local",
  "event.NewProcessName": "C:\\Users\\elf_user02\\Downloads\\howtosavexmas\\howtosavexmas.pdf.exe",
  "event.ParentProcessName": "C:\\Windows\\explorer.exe",
  "event.ProcessID": 4,
  "event.ProcessInformation": "Process Command Line:,",
  "event.ProcessInformation_NewProcessName": "C:\\Users\\elf_user02\\Downloads\\howtosavexmas\\howtosavexmas.pdf.exe",
  "event.ProcessInformation_TokenElevationType": "%%1937",
  "event.SourceName": "Microsoft-Windows-Security-Auditing",
  "event.SubjectDomainName": "NORTHPOLE",
  "event.SubjectUserName": "SLEIGHRIDER$",
  "event.TargetDomainName": "NORTHPOLE",
  "event.TargetSubject_AccountName": "elf_user02",
  "event.TargetUserName": "elf_user02",
  "event.TargetUserSid": "S-1-5-21-3699322559-1991583901-1175093138-1110",
  "event.Task": 13312,
  "event.ThreadID": 1460,
  "event_source": "WindowsEvent",
  "host.ip": "172.18.0.5",
  "hostname": "SleighRider.northpole.local",
}
```
Again, not useful for this question, but I found it interesting.

###Question 8
**Did the attacker's payload make an outbound network connection? Our ElfSOC analysts need your help identifying the destination TCP port of this connection.**

From command line it was easier. I followed the trail of the events for the file using the following command until I located the log with the information I needed:
```
grep "SleighRider" log_chunk_* | grep -i '"Hostname": "SleighRider.northpole.local"' | grep howtosavexmas


log_chunk_1.log:<134>1 2024-09-15T10:37:50-04:00 SleighRider.northpole.local WindowsEvent - - - {"EventTime": "2024-09-15 10:37:50", "Hostname": "SleighRider.northpole.local", "Keywords": -9214364837600034816, "EventType": "AUDIT_SUCCESS", "SeverityValue": 2, "Severity": "INFO", "EventID": 5156, "SourceName": "Microsoft-Windows-Security-Auditing", "ProviderGuid": "{54849625-5478-4994-A5BA-3E3B0328C30D}", "Version": 1, "Task": 12810, "OpcodeValue": 0, "RecordNumber": 734596, "ProcessID": 4, "ThreadID": 1460, "Channel": "Security", "Category": "Filtering Platform Connection", "Opcode": "Info", "Application": "\\device\\harddiskvolume3\\users\\elf_user02\\downloads\\howtosavexmas\\howtosavexmas.pdf.exe", "Direction": "%%14593", "SourceAddress": "172.24.25.12", "SourcePort": 64543, "DestAddress": "103.12.187.43", "DestPort": 8080, "Protocol": 6, "FilterRTID": 0, "LayerName": "%%14611", "LayerRTID": 48, "RemoteUserID": "S-1-0-0", "RemoteMachineID": "S-1-0-0", "EventReceivedTime": "2024-09-15T10:37:50-04:00", "SourceModuleName": "inSecurityEvent", "SourceModuleType": "im_msvistalog", "ApplicationInformation_ProcessID": 8096, "ApplicationInformation_ApplicationName": "\\device\\harddiskvolume3\\users\\elf_user02\\downloads\\howtosavexmas\\howtosavexmas.pdf.exe", "NetworkInformation_Direction": "Outbound", "NetworkInformation_SourceAddress": "172.24.25.12", "NetworkInformation_SourcePort": 64543, "NetworkInformation_DestinationAddress": "103.12.187.43", "NetworkInformation_DestinationPort": 8443, "NetworkInformation_Protocol": 6, "FilterInformation_FilterRunTimeID": 0, "FilterInformation_LayerName": "Connect", "FilterInformation_LayerRunTimeID": 48, "MoreDetails": "The Windows Filtering Platform has permitted a connection."}
```
Answer: **8443**

###Question 9
**The attacker escalated their privileges to the SYSTEM account by creating an inter-process communication (IPC) channel. Submit the alpha-numeric name for the IPC channel used by the attacker.**
```
grep "SleighRider" log_chunk_* | grep -i '"Hostname": "SleighRider.northpole.local"' | grep "pipe"
```
This give us a few interesting hits and the answer:
```
log_chunk_1.log:<134>1 2024-09-15T10:38:22-04:00 SleighRider.northpole.local WindowsEvent - - - {"EventTime": "2024-09-15 10:38:22", "Hostname": "SleighRider.northpole.local", "Keywords": -9223372036854775808, "EventType": "INFO", "SeverityValue": 2, "Severity": "INFO", "EventID": 13, "SourceName": "Microsoft-Windows-Sysmon", "ProviderGuid": "{5770385F-C22A-43E0-BF4C-06F5698FFBD9}", "Version": 2, "Task": 13, "OpcodeValue": 0, "RecordNumber": 731, "ProcessID": 10014, "ThreadID": 6340, "Channel": "Microsoft-Windows-Sysmon/Operational", "Domain": "NT AUTHORITY", "AccountName": "SYSTEM", "UserID": "S-1-5-18", "AccountType": "User", "Category": "Registry value set (rule: RegistryEvent)", "Opcode": "Info", "RuleName": "T1031,T1050", "UtcTime": "2024-09-15T10:38:22-04:00", "ProcessGuid": "{face0b26-e125-6606-0b00-000000000700}", "Image": "C:\\Windows\\system32\\services.exe", "TargetObject": "HKLM\\System\\CurrentControlSet\\Services\\ddpvccdbr\\ImagePath", "Details": "cmd.exe /c echo ddpvccdbr &gt; \\\\.\\pipe\\ddpvccdbr", "User": "NT AUTHORITY\\SYSTEM", "EventReceivedTime": "2024-09-15T10:38:22-04:00", "SourceModuleName": "inSysmon", "SourceModuleType": "im_msvistalog", "ProcessId": 628, "MoreDetails": "Registry value set:"}
```
From ELK, we can do the following query to filter by the process ID for the malicious executable (answer to question 7). Checking the logs sequence provides the answer to this question as well as other interesting information about the attacker actions:
```
from logs-generic-default
| where event_source == "WindowsEvent" and (event.EventID == 11 or event.EventID == 1)
| where event.ProcessID == 10014
```

Answer: **ddpvccdbr**

###Question 10
**The attacker's process attempted to access a file. Submit the full and complete file path accessed by the attacker's process.**

With a little googling I found that the interesting event IDs to search for would be:

* 4663: An attempt was made to access an object
* 4656: A handle to an object was requested.

So let's search in ELK using as additional filter the process ID we know from previous questions:

```
from logs-generic-default
| where event_source == "WindowsEvent" and (event.EventID == 4663 or event.EventID == 4656)
| where event.ProcessID == 10014
```
There's only one result containing the following:
```
 "event.ObjectName": "C:\\Users\\elf_user02\\Desktop\\kkringl315@10.12.25.24.pem",
```
Answer: **C:\Users\elf_user02\Desktop\kkringl315@10.12.25.24.pem**

###Question 11
**The attacker attempted to use a secure protocol to connect to a remote system. What is the hostname of the target server?**

From the previous question, we know the attacker got hold of what it is most likely an SSH Key. I initially tried searching for network events in the Windows Events:
```
from logs-generic-default
| where event_source == "WindowsEvent" and event.EventID == 3
| where event.ProcessID == 10014
```
The result shows 3 IPs, the below are the IPs and the corresponding hostnames
```
172.24.25.153 - DC01.northpole.local
103.12.187.43 - 19.148.239.35.bc.googleusercontent.com
172.24.25.25 - SECUREELFGWY.northpole.local
```
However, none of those are the answer.

I also checked GreenCoat logs, but could not find any interesting log. I decided to go back to check what are all the sources we have available:
```
    269 AuthLog
   1398 SnowGlowMailPxy
   7476 GreenCoat
  34679 NetflowPmacct
2299324 WindowsEvent
```
From the available sources, `AuthLog` is the most promising as it contains Linux authentication logs, which a login using SSH would have triggered. Nevertheless, I first grepped for the string `kkringl315` (the username as per the answer to the previous question) as follow:

```
grep 'kkringl315' log_chunk_*
```
The results confirmed that the answer is likely hidden in the `AuthLog`, so I did the below query:
```
from logs-generic-default | where event_source == "AuthLog"
| where event.service like "*ssh*"
```
And from there, I further refined the query:
```
from logs-generic-default | where event_source == "AuthLog"
| where event.service like "*ssh*"
| stats count() by event.hostname
```
Only one option now.

Answer: **kringleSSleigH**

###Question 12
**The attacker created an account to establish their persistence on the Linux host. What is the name of the new account created by the attacker?**

It is easy to find the answer with grep:
```
grep 'AuthLog' log_chunk_* | grep 'useradd'

log_chunk_2.log:<134>1 2024-09-16T10:59:46-04:00 kringleSSleigH AuthLog - - - {"timestamp": "2024-09-16T13:59:46.121497-04:00", "hostname": "kringleSSleigH", "service": "useradd[6207]:", "message": "new user: name=ssdh, UID=1002, GID=1002, home=/home/ssdh, shell=/bin/bash, from=/dev/pts/6"}
```
Answer: **ssdh**

###Question 13
**The attacker wanted to maintain persistence on the Linux host they gained access to and executed multiple binaries to achieve their goal. What was the full CLI syntax of the binary the attacker executed after they created the new user account?**

Similar to above, it is easy to find the answer with a grep command:
```
grep 'AuthLog' log_chunk_* | grep 'useradd' -A 10
```
with that we can see what the attacker did after adding the user. To answer the question, this is the relevant log entry:
```
log_chunk_2.log:<134>1 2024-09-16T11:00:14-04:00 kringleSSleigH AuthLog - - - {"timestamp": "2024-09-16T14:00:14.317262-04:00", "hostname": "kringleSSleigH", "service": "sudo:", "message": " kkringl315 : TTY=pts/5 ; PWD=/opt ; USER=root ; COMMAND=/usr/sbin/usermod -a -G sudo ssdh"}
```
Answer: **/usr/sbin/usermod -a -G sudo ssdh**

###Question 14
**The attacker enumerated Active Directory using a well known tool to map our Active Directory domain over LDAP. Submit the full ISO8601 compliant timestamp when the first request of the data collection attack sequence was initially recorded against the domain controller.**

The dc is `dc01.northpole.local`

Grepping for windows events against the DC with Event ID 2889 (LDAP bind) will likely highlight the attack:
```
grep "WindowsEvent" log_chunk_* | grep 'dc01.northpole.local' | grep '"EventID": 2889'
```
This is the first hit:
```
log_chunk_2.log:<134>1 2024-09-16T11:10:12-04:00 dc01.northpole.local WindowsEvent - - - {"LogName": "Directory Service", "Source": "Microsoft-Windows-ActiveDirectory_DomainService", "EventID": 2889, "Category": "LDAP Interface", "Level": "Information", "Keywords": "Classic", "Description": "The following client performed a SASL (Negotiate/Kerberos/NTLM/Digest) LDAP bind without requesting signing (integrity verification), or performed a simple bind over a clear text (non-SSL/TLS-encrypted) LDAP connection.", "Computer": "dc01.northpole.local", "ClientIPaddress": "172.24.25.22:18598", "ServicePort": 389, "ServiceName": "dc01.northpole.local", "ServiceIpAddress": "172.24.25.153", "UserID": "elf_user@northpole.local", "BindType": "0 - Simple Bind that does not support signing", "Date": "2024-09-16T11:10:12-04:00"}
```
Answer: **2024-09-16T11:10:12-04:00**

###Question 15
**The attacker attempted to perform an ADCS ESC1 attack, but certificate services denied their certificate request. Submit the name of the software responsible for preventing this initial attack.**

Event ID 4886 is for certificate requests, so I started grepping for it:
```
grep "WindowsEvent" log_chunk_* | grep '"EventID": 4886'
```
There's only one hit, and it looks suspicious:
```
log_chunk_2.log:<134>1 2024-09-16T11:15:12-04:00 dc01.northpole.local WindowsEvent - - - {"LogName": "Security", "Source": "Microsoft-Windows-Security-Auditing", "Date": "2024-09-16T11:15:12-04:00", "EventID": 4886, "Category": "Certification Services - Certificate Issuance", "Level": "Information", "Keywords": "Audit Success", "User": "N/A", "Computer": "dc01.northpole.local", "Description": "A certificate was issued to a user.", "UserInformation_UserName": "elf_user@northpole.local", "UserInformation_UPN": "nutcrakr@northpole.local", "CertificateInformation_CertificateAuthority": "elf-dc01-SeaA", "CertificateInformation_CertificateTemplate": "ElfUsers", "AdditionalInformation_RequesterComputer": "10.12.25.24", "AdditionalInformation_CallerComputer": "172.24.25.153"}
```

So my next step was to check what logs were before or after that one:
```
grep "WindowsEvent" log_chunk_* | grep '"EventID": 4886' -A 10 -B 10
```
And sure enough, one of the entries contains the answer:
```
log_chunk_2.log:<134>1 2024-09-16T11:14:12-04:00 dc01.northpole.local WindowsEvent - - - {"LogName": "Security", "Source": "Microsoft-Windows-Security-Auditing", "Date": "2024-09-16T11:14:12-04:00", "EventID": 4888, "Category": "Certification Services - Certificate Request Denied", "Level": "Information", "Keywords": "Audit Failure", "User": "N/A", "Computer": "dc01.northpole.local", "Description": "A certificate request was made for a certificate template, but the request was denied because it did not meet the criteria.", "UserInformation_UserName": "elf_user@northpole.local", "CertificateInformation_CertificateAuthority": "elf-dc01-SeaA", "CertificateInformation_RequestedTemplate": "Administrator", "ReasonForRejection": "KringleGuard EDR flagged the certificate request.", "AdditionalInformation_RequesterComputer": "10.12.25.24", "AdditionalInformation_RequestedUPN": "administrator@northpole.local"}
```
Answer: **KringleGuard**

###Question 16
**We think the attacker successfully performed an ADCS ESC1 attack. Can you find the name of the user they successfully requested a certificate on behalf of?**

The same search I did for the previous question has the answer, as the first log in the output contains the successful attack:
```
grep "WindowsEvent" log_chunk_* | grep '"EventID": 4886'

log_chunk_2.log:<134>1 2024-09-16T11:15:12-04:00 dc01.northpole.local WindowsEvent - - - {"LogName": "Security", "Source": "Microsoft-Windows-Security-Auditing", "Date": "2024-09-16T11:15:12-04:00", "EventID": 4886, "Category": "Certification Services - Certificate Issuance", "Level": "Information", "Keywords": "Audit Success", "User": "N/A", "Computer": "dc01.northpole.local", "Description": "A certificate was issued to a user.", "UserInformation_UserName": "elf_user@northpole.local", "UserInformation_UPN": "nutcrakr@northpole.local", "CertificateInformation_CertificateAuthority": "elf-dc01-SeaA", "CertificateInformation_CertificateTemplate": "ElfUsers", "AdditionalInformation_RequesterComputer": "10.12.25.24", "AdditionalInformation_CallerComputer": "172.24.25.153"}
```
Answer: **nutcrakr**

###Question 17
**One of our file shares was accessed by the attacker using the elevated user account (from the ADCS attack). Submit the folder name of the share they accessed.**

Event ID 5140 identifies network share access. So I searched as follow:
```
from logs-generic-default
| where event_source == "WindowsEvent"
| where event.User like "*nutcrakr*"
| where event.EventID == 5140
```
There was no hits, so I tried with grep checking the word share, and strangely I got hits that had event ID 5140:
```
grep "WindowsEvent" log_chunk_* | grep "nutcrakr" | grep -i share

log_chunk_2.log:<134>1 2024-09-16T11:18:43-04:00 dc01.northpole.local WindowsEvent - - - {"EventTime": "2024-09-16 11:18:43", "Hostname": "dc01.northpole.local", "Keywords": -9214364837600034816, "EventType": "AUDIT_SUCCESS", "SeverityValue": 2, "Severity": "INFO", "EventID": 5140, "SourceName": "Microsoft-Windows-Security-Auditing", "ProviderGuid": "{54849625-5478-4994-A5BA-3E3B0328C30D}", "Version": 1, "Task": 12808, "OpcodeValue": 0, "RecordNumber": 498420, "ProcessID": 4, "ThreadID": 5692, "Channel": "Security", "Category": "File Share", "Opcode": "Info", "SubjectUserSid": "S-1-5-21-3699322559-1991583901-1175093138-1112", "SubjectUserName": "nutcrakr", "SubjectDomainName": "NORTHPOLE", "SubjectLogonId": "0xd8fdb3", "ObjectType": "File", "IpAddress": "34.30.110.62", "IpPort": 53378, "ShareName": "\\\\*\\WishLists", "ShareLocalPath": "\\??\\C:\\WishLists", "AccessMask": "0x1", "AccessList": "%%4416\r\n\t\t\t\t", "EventReceivedTime": "2024-09-16T11:18:43-04:00", "SourceModuleName": "inSecurityEvent", "SourceModuleType": "im_msvistalog", "Subject_SecurityID": "S-1-5-21-3699322559-1991583901-1175093138-1112", "Subject_AccountName": "nutcrakr", "Subject_AccountDomain": "NORTHPOLE", "Subject_LogonID": "0xD8FDB3", "NetworkInformation_ObjectType": "File", "NetworkInformation_SourceAddress": "34.30.110.62", "NetworkInformation_SourcePort": 53378, "NetworkInformation": ",", "ShareInformation_ShareName": "\\\\*\\WishLists", "ShareInformation_SharePath": "\\??\\C:\\WishLists", "AccessRequestInformation_AccessMask": "0x1", "AccessRequestInformation_Accesses": "ReadData (or ListDirectory)", "AccessRequestInformation": ",", "MoreDetails": "A network share object was accessed."}
```
Looks like not all logs were ingested by ELK, and actually, this is mentioned in the hints from the Elf. The above log has the answer:

Answer: **WishLists**


###Question 18
**The naughty attacker continued to use their privileged account to execute a PowerShell script to gain domain administrative privileges. What is the password for the account the attacker used in their attack payload?**

I used grep to locate even ID 4104 Script block logging (shows the content of executed scripts).
```
grep "WindowsEvent" log_chunk_* | grep "nutcrakr" | grep -i 4104


log_chunk_2.log:<134>1 2024-09-16T11:33:12-04:00 SleighRider.northpole.local WindowsEvent - - - {"MessageNumber": 1, "MessageTotal": 1, "ScriptBlockText": "Add-Type -AssemblyName System.DirectoryServices\n$ldapConnString = \"LDAP://CN=Domain Admins,CN=Users,DC=northpole,DC=local\"\n$username = \"nutcrakr\"\n$pswd = 'fR0s3nF1@k3_s'\n$nullGUID = [guid]'00000000-0000-0000-0000-000000000000'\n$propGUID = [guid]'00000000-0000-0000-0000-000000000000'\n$IdentityReference = (New-Object System.Security.Principal.NTAccount(\"northpole.local\\$username\")).Translate([System.Security.Principal.SecurityIdentifier])\n$inheritanceType = [System.DirectoryServices.ActiveDirectorySecurityInheritance]::None\n$ACE = New-Object System.DirectoryServices.ActiveDirectoryAccessRule $IdentityReference, ([System.DirectoryServices.ActiveDirectoryRights] \"GenericAll\"), ([System.Security.AccessControl.AccessControlType] \"Allow\"), $propGUID, $inheritanceType, $nullGUID\n$domainDirEntry = New-Object System.DirectoryServices.DirectoryEntry $ldapConnString, $username, $pswd\n$secOptions = $domainDirEntry.get_Options()\n$secOptions.SecurityMasks = [System.DirectoryServices.SecurityMasks]::Dacl\n$domainDirEntry.RefreshCache()\n$domainDirEntry.get_ObjectSecurity().AddAccessRule($ACE)\n$domainDirEntry.CommitChanges()\n$domainDirEntry.dispose()\n$ldapConnString = \"LDAP://CN=Domain Admins,CN=Users,DC=northpole,DC=local\"\n$domainDirEntry = New-Object System.DirectoryServices.DirectoryEntry $ldapConnString, $username, $pswd\n$user = New-Object System.Security.Principal.NTAccount(\"northpole.local\\$username\")\n$sid=$user.Translate([System.Security.Principal.SecurityIdentifier])\n$b=New-Object byte[] $sid.BinaryLength\n$sid.GetBinaryForm($b,0)\n$hexSID=[BitConverter]::ToString($b).Replace('-','')\n$domainDirEntry.Add(\"LDAP://<SID=$hexSID>\")\n$domainDirEntry.CommitChanges()\n$domainDirEntry.dispose()", "ScriptBlockId": "{01bbe2da-58c3-4490-aa52-682dbae233a3}", "Path": "", "Provider_Name": "Microsoft-Windows-PowerShell", "Provider_Guid": "{a0c1853b-5c40-4b15-8766-3cf1c58f985a}", "EventID": 4104, "Version": 1, "Level": 5, "Task": 2, "Opcode": 15, "Keywords": "0x0", "TimeCreated_SystemTime": "2024-09-16T11:33:12-04:00", "EventRecordID": 54059, "Correlation_ActivityID": "{17aa0df9-5d3d-46e9-bce0-55b7a5be4b43}", "ParentProcessID": 928, "ThreadID": 4896, "Channel": "Microsoft-Windows-PowerShell/Operational", "Computer": "SleighRider.northpole.local", "Security_UserID": "S-1-5-21-3699322559-1991583901-1175093138-1110"}
```

Answer: **fR0s3nF1@k3_s**

###Question 19
**The attacker then used remote desktop to remotely access one of our domain computers. What is the full ISO8601 compliant UTC EventTime when they established this connection?**

I checked Event ID 4624 (successful logon) with logon type 10 (which is specific for RDP), and there was only one hit:

with grep:
```
grep "WindowsEvent" log_chunk_* | grep '"EventID": 4624' | grep '"LogonType": 10'
```
with ELK:
```
from logs-generic-default
| where event_source == "WindowsEvent"
| where event.EventID == 4624
| where event.LogonType == 10
```
```
log_chunk_2.log:<134>1 2024-09-16T11:35:57-04:00 dc01.northpole.local WindowsEvent - - - {"EventTime": "2024-09-16 11:35:57", "Hostname": "dc01.northpole.local", "Keywords": -9214364837600034816, "EventType": "AUDIT_SUCCESS", "SeverityValue": 2, "Severity": "INFO", "EventID": 4624, "SourceName": "Microsoft-Windows-Security-Auditing", "ProviderGuid": "{54849625-5478-4994-A5BA-3E3B0328C30D}", "Version": 2, "Task": 12544, "OpcodeValue": 0, "RecordNumber": 530313, "ActivityID": "{D72392BD-843F-0000-1F93-23D73F84DA01}", "ProcessID": 704, "ThreadID": 1124, "Channel": "Security", "Category": "Logon", "Opcode": "Info", "SubjectUserSid": "S-1-5-18", "SubjectUserName": "DC01$", "SubjectDomainName": "NORTHPOLE", "SubjectLogonId": "0x3e7", "TargetUserSid": "S-1-5-21-3699322559-1991583901-1175093138-1112", "TargetUserName": "nutcrakr", "TargetDomainName": "NORTHPOLE", "TargetLogonId": "0xdd425e", "LogonType": 10, "LogonProcessName": "User32 ", "AuthenticationPackageName": "Negotiate", "WorkstationName": "DC01", "LogonGuid": "{00000000-0000-0000-0000-000000000000}", "TransmittedServices": "-", "LmPackageName": "-", "KeyLength": 0, "ProcessName": "C:\\Windows\\System32\\svchost.exe", "IpAddress": "10.12.25.24", "IpPort": 0, "ImpersonationLevel": "%%1833", "RestrictedAdminMode": "%%1843", "TargetOutboundUserName": "-", "TargetOutboundDomainName": "-", "VirtualAccount": "%%1843", "TargetLinkedLogonId": "0xdd41af", "ElevatedToken": "%%1843", "EventReceivedTime": "2024-09-16T11:35:57-04:00", "SourceModuleName": "inSecurityEvent", "SourceModuleType": "im_msvistalog", "Subject_SecurityID": "S-1-5-18", "Subject_AccountName": "DC01$", "Subject_AccountDomain": "NORTHPOLE", "Subject_LogonID": "0x3E7", "LogonInformation_LogonType": 10, "LogonInformation_RestrictedAdminMode": "No", "LogonInformation_VirtualAccount": "No", "LogonInformation_ElevatedToken": "No", "NewLogon_SecurityID": "S-1-5-21-3699322559-1991583901-1175093138-1112", "NewLogon_AccountName": "nutcrakr", "NewLogon_AccountDomain": "NORTHPOLE", "NewLogon_LogonID": "0xDD425E", "NewLogon_LinkedLogonID": "0xDD41AF", "NewLogon_NetworkAccountName": "-", "NewLogon_NetworkAccountDomain": "-", "NewLogon_LogonGUID": "{00000000-0000-0000-0000-000000000000}", "ProcessInformation_ProcessID": "0x994", "ProcessInformation_ProcessName": "C:\\Windows\\System32\\svchost.exe", "NetworkInformation_WorkstationName": "DC01", "NetworkInformation_SourceNetworkAddress": "10.12.25.24", "NetworkInformation_SourcePort": 0, "DetailedAuthenticationInformation_LogonProcess": "User32", "DetailedAuthenticationInformation_AuthenticationPackage": "Negotiate", "DetailedAuthenticationInformation_TransitedServices": "-", "DetailedAuthenticationInformation_PackageNameNTLMonly": "-", "DetailedAuthenticationInformation_KeyLength": 0, "MoreDetails": "An account was successfully logged on.\nThis event is generated when a logon session is created. It is generated on the computer that was accessed.\nThe subject fields indicate the account on the local system which requested the logon. This is most commonly a service such as the Server service, or a local process such as Winlogon.exe or Services.exe.\nThe logon type field indicates the kind of logon that occurred. The most common types are 2 (interactive) and 3 (network).\nThe New Logon fields indicate the account for whom the new logon was created, i.e. the account that was logged on.\nThe network fields indicate where a remote logon request originated. Workstation name is not always available and may be left blank in some cases.\nThe impersonation level field indicates the extent to which a process in the logon session can impersonate.\nThe authentication information fields provide detailed information about this specific logon request.\n- Logon GUID is a unique identifier that can be used to correlate this event with a KDC event.\n- Transited services indicate which intermediate services have participated in this logon request.\n- Package name indicates which sub-protocol was used among the NTLM protocols.\n- Key length indicates the length of the generated session key. This will be 0 if no session key was requested."}
```

Answer: **2024-09-16T15:35:57.000Z**

###Question 20
**The attacker is trying to create their own naughty and nice list! What is the full file path they created using their remote desktop connection?**

I saw the log as part of researching previous answers and I was pretty sure it was related to the answer for Q17, so I did a simple grep to try to locate the log again using known information like the target host and the username:
```
grep "WindowsEvent" log_chunk_* | grep "nutcrakr" | grep "dc01.northpole.local" | grep -v '"PrivilegeList": ' | grep -i list

log_chunk_2.log:<134>1 2024-09-16T11:36:28-04:00 dc01.northpole.local WindowsEvent - - - {"EventTime": "2024-09-16 11:36:28", "Hostname": "dc01.northpole.local", "Keywords": -9223372036854775808, "EventType": "INFO", "SeverityValue": 2, "Severity": "INFO", "EventID": 1, "SourceName": "Microsoft-Windows-Sysmon", "ProviderGuid": "{5770385F-C22A-43E0-BF4C-06F5698FFBD9}", "Version": 5, "Task": 1, "OpcodeValue": 0, "RecordNumber": 641, "ProcessID": 6468, "ThreadID": 4816, "Channel": "Microsoft-Windows-Sysmon/Operational", "Domain": "NT AUTHORITY", "AccountName": "SYSTEM", "UserID": "S-1-5-18", "AccountType": "User", "Category": "Process Create (rule: ProcessCreate)", "Opcode": "Info", "RuleName": "-", "UtcTime": "2024-09-16T11:36:28-04:00", "ProcessGuid": "{f151dc49-502c-660c-8702-000000000900}", "Image": "C:\\Windows\\System32\\notepad.exe", "FileVersion": "10.0.17763.1697 (WinBuild.160101.0800)", "Description": "Notepad", "Product": "Microsoft\u00ae Windows\u00ae Operating System", "Company": "Microsoft Corporation", "OriginalFileName": "NOTEPAD.EXE", "CommandLine": "\"C:\\Windows\\system32\\NOTEPAD.EXE\" C:\\WishLists\\santadms_only\\its_my_fakelst.txt", "CurrentDirectory": "C:\\WishLists\\santadms_only\\", "User": "NORTHPOLE\\nutcrakr", "LogonGuid": "{f151dc49-500d-660c-5e42-dd0000000000}", "LogonId": "0xdd425e", "TerminalSessionId": 2, "IntegrityLevel": "Medium", "Hashes": "MD5=5394096A1CEBF81AF24E993777CAABF4,SHA256=A28438E1388F272A52559536D99D65BA15B1A8288BE1200E249851FDF7EE6C7E,IMPHASH=C8922BE3DCDFEB5994C9EEE7745DC22E", "ParentProcessGuid": "{f151dc49-500f-660c-5902-000000000900}", "ParentProcessId": 1364, "ParentImage": "C:\\Windows\\explorer.exe", "ParentCommandLine": "C:\\Windows\\Explorer.EXE", "ParentUser": "NORTHPOLE\\nutcrakr", "EventReceivedTime": "2024-09-16T11:36:28-04:00", "SourceModuleName": "inSysmon", "SourceModuleType": "im_msvistalog", "ProcessId": 9152, "MoreDetails": "Process Create:"}
```

Answer: **C:\WishLists\santadms_only\its_my_fakelst.txt**

###Question 21
**The Wombley faction has user accounts in our environment. How many unique Wombley faction users sent an email message within the domain?**

I initially struggled to understand the question, so I tried to work out the answer by dissecting it slowly. The question asks about emails, so I need to check the SnowGlowMailPxy source. I started by using ELK to display all the unique email addresses that were used in the logs:
```
from logs-generic-default
| where event_source == "SnowGlowMailPxy"
| stats count() by event.From
```
There are 571 emails, and that was not the answer. So let's dig in more into the question. It says "within the domain", so I guess we can filter by emails sent from the `northpole.local` domain:
```
from logs-generic-default
| where event_source == "SnowGlowMailPxy"
| where event.From like "*northpole*"
| stats count() by event.From
```
That give us 21 unique addresses. But that's not the answer either. Checking the results I can see the following:

![Elf Stack Q21](/images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-16.png)

Now I understand! Some email addresses look like using the name of Wombley Cube, suggesting these belong to the Wombley faction users. With this in mind, let's build a query that will give us the final answer:

```
from logs-generic-default
| where event_source == "SnowGlowMailPxy"
| where event.From like "*northpole*" and event.From like "*cub*"
| stats COUNT_DISTINCT(event.From)
```

Answer: **4**

###Question 22
**The Alabaster faction also has some user accounts in our environment. How many emails were sent by the Alabaster users to the Wombley faction users?**

I built this query to get the answer:
```
from logs-generic-default
| where event_source == "SnowGlowMailPxy"
| where event.From like "*northpole*" and event.From like "*snow*"
| where event.To like "*cub*"
```
The above initial query returned 23 results, but the answer was incorrect. After some back and forth trying to understand why, I modified the query to display all the unique To and From values instead:

```
from logs-generic-default
| where event_source == "SnowGlowMailPxy"
| where event.From like "*northpole*" and event.From like "*snow*"
| where event.To like "*cub*"
| stats count() by event.From, event.To
```

And there it was, naughty Santa had guessed the query and added a trap:

![Elf Stack Q22](/images/HHC/2024/Act%203/Elf%20Stack/Elf%20Stack-17.png)

So to the previous count of 23 we need to remove that 1 email, giving us the final answer.

Answer: **22**

###Question 23
**Of all the reindeer, there are only nine. What's the full domain for the one whose nose does glow and shine? To help you narrow your search, search the events in the 'SnowGlowMailPxy' event source.**

I grepped for what I thought would make sense: `rudolph`. There was not that many emails, so I just went through the body and selected the ones that did not look like phishing or spam, got it at the second try.
```
grep "SnowGlowMailPxy" log_chunk_* | grep -i rudolph
```
Answer: **rud01ph.glow**


###Question 24
**With a fiery tail seen once in great years, what's the domain for the reindeer who flies without fears? To help you narrow your search, search the events in the 'SnowGlowMailPxy' event source.**

The first challenge was to figure out the name of the Reindeer... Some Googling and some guessing suggested that maybe comet was the name I was looking for.

For me, the best query to find the answer was to display all unique From email addresses as follow:
```
grep "SnowGlowMailPxy" log_chunk_* | grep -oP '"From":\s*"\K[^"]+' | sort | uniq
```

The above command showed all the unique emails ordered alphabetically. This made easier to locate good candidates by glancing over the results.

Answer: **c0m3t.halleys**

#Final Message from Fitzy Shortstack

Once we complete the objective, Fitzy tell us the following:

![Fitzy Shortstack Conversation Completion](../../../../images/HHC/2024/Act%203/Elf%20Stack/Fitzy%20Shortstack-2.png)

#Challenge Answers Summary

Easy Mode - Silver

Q1: 5<br>
Q2: AuthLog<br>
Q3: hostname<br>
Q4: NetflowPmacct<br>
Q5: port_dst<br>
Q6: SnowGlowMailPxy<br>
Q7: Body<br>
Q8: SecureElfGwy<br>
Q9: url<br>
Q10: pagead2.googlesyndication.com:443<br>
Q11: 5<br>
Q12: Microsoft-Windows-Sysmon/Operational<br>
Q13: 6<br>
Q14: 4697<br>
Q15: 0

Hard Mode - Gold

Q1: 1<br>
Q2: 5<br>
Q3: SnowGlowMailPxy<br>
Q4: kriskring1e@northpole.local<br>
Q5: SleighRider<br>
Q6: 172.24.25.12<br>
Q7: 10014<br>
Q8: 8443<br>
Q9: ddpvccdbr<br>
Q10: C:\Users\elf_user02\Desktop\kkringl315@10.12.25.24.pem<br>
Q11: kringleSSleigH<br>
Q12: ssdh<br>
Q13: /usr/sbin/usermod -a -G sudo ssdh<br>
Q14: 2024-09-16T11:10:12-04:00<br>
Q15: KringleGuard<br>
Q16: nutcrakr<br>
Q17: WishLists<br>
Q18: fR0s3nF1@k3_s<br>
Q19: 2024-09-16T15:35:57.000Z<br>
Q20: C:\WishLists\santadms_only\its_my_fakelst.txt<br>
Q21: 4<br>
Q22: 22<br>
Q23: rud01ph.glow<br>
Q24: c0m3t.halleys<br>

