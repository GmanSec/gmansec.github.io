<div style="text-align: center; font-size: 2em; font-weight: bold; color: black; margin-bottom: 20px;">
    Santa Vision - Ribb Bonbowford
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

When we speak with Ribb this is what he has to tell us regarding the challenge we need to complete:

![Ribb Bonbowford Conversation 1](../../../../images/HHC/2024/Act%203/Santa%20Vision/Ribb%20Bonbowford-1.png)
![Ribb Bonbowford Conversation 2](../../../../images/HHC/2024/Act%203/Santa%20Vision/Ribb%20Bonbowford-2.png)

The initial hints are given by Ribb to help us with the challenge (some hints are triggered after completing some of the sub-objectives):

##Hint 1 - Mosquito Mosquitto

[Mosquitto](https://mosquitto.org/) is a great client for interacting with MQTT, but their spelling may be suspect. Prefer a GUI? Try [MQTTX](https://mqttx.app/)

##Hint 2 - Misplaced Credentials

See if any credentials you find allow you to subscribe to any [MQTT](https://en.wikipedia.org/wiki/MQTT) feeds.

##Hint 3 - Filesystem Analysis

[jefferson](https://github.com/onekey-sec/jefferson/) is great for analyzing JFFS2 file systems.

##Hint 4 - Database Pilfering

Consider checking any database files for credentials...

##Hint 5 - Like a Good Header on Your HTTP?

Be on the lookout for strange HTTP headers... 

##Hint 6 - Looking Deeper

Discovering the credentials will show you the answer, but will you see it?

##Access to the Challenge - Environment set up

When we click the terminal we are redirected to our favorite festive alligator: GateXOR. We start the challenge by clicking “Time Travel” Button to load the environment:

![Santa Vision Challenge Set up](../../../../images/HHC/2024/Act%203/Santa%20Vision/SantaVision-1.png)

As shown in the image above, once the environment loads, an IP address will be allocated to us for the challenge. In the example above, the IP address allocated is: **34.45.163.69**.<br>
**Note**: The IP address may change each time you start the challenge. As a result, the solutions below reference different IP addresses depending on the specific session.

#**Solution - Steps Common for Silver and Gold**

**Note**: The Silver and Gold solutions are entirely independent of each other. It's highly recommended to complete the Silver challenge first before moving on to Gold. Attempting both simultaneously may lead to confusion and make it more difficult to arrive at the correct solutions for each. However, these initial steps are the same for both paths.

As suggested by Ribb, let's begin by scanning the target. I started with a standard TCP scan:

```bash
nmap -sTVC -Pn 34.45.163.69 -nvv -oA santavision
```

The results show that three services are open: 22, 8000, 9001.
While we investigate those services, we should also launch a full TCP scan to ensure we didn't miss anything:

```bash
nmap -sT -p- -Pn 34.45.163.69 -nvv -oA santavision.full
```

<div style="text-align: center;">
  <img src="/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-4.png" alt="Santa Vision A full TCP scan" border-radius: 10px;">
  <p><em>Full TCP scan result</em></p>
</div>


The full TCP scan revealed an additional open port: 1883, which Nmap identified as an MQTT service. Given the hints, it seems likely that we'll need to interact with this service using the tools (Mosquitto or MQTTX).

Port 8000 appears to be running an HTTP service, so let's access it through the browser:

![Santa Vision Web Page](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-3.png)

#**Solution - Silver**

##Santa Vision A - Silver

![Santa Vision A Objective](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-0.png)

The first step appears to be finding credentials to log in to the portal. To start, I explored the website to see what I could find. I began by checking the page’s source code, where I found the following interesting comment:

![Santa Vision Portal Source Comment](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-5.png)

The comment seems to contain credentials for the MQTT service, let's try:

![Santa Vision MQTT](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-8.png)

Works! The same credentials worked to log into the portal:

![Santa Vision portal Log in](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-6.png)

Santa Vision A silver answer: **elfanon**.

##Santa Vision B - Silver

![Santa Vision B Objective](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-7.png)

**Tip**: Make sure to talk to Ribb after completing each section. He provides valuable hints and information that can guide you toward the next objective. This is what he tell us after completing A (he also gives a hint for A Gold but will cover that in the Gold path):

<div class="conversation">
Great work! You've taken the first step—nicely done. You're on the silver path and off to a strong start!

You've gained access, but there’s still much more to uncover. Patience and persistence will guide you—silver or gold, you're making progress!

Now that you're in, it’s time to go deeper. We need access to the northpolefeeds. This won't work if you use Wombley or Alabaster’s credentials—find the right user to log in.
</div>

By experimenting with the portal, it became evident that it serves as a front-end interface for interacting with the MQTT service. There are two buttons called “List Available Clients” and “List Available Roles” in the page. By using them, we can gather some valuable data to assist in solving Santa Vision B:

<div style="text-align: center;">
  <img src="/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-9.png" alt="Availble Clients" border-radius: 10px;">
  <p><em>List Available Clients</em></p>
</div>


<div style="text-align: center;">
  <img src="/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-22.png" alt="Available Roles" border-radius: 10px;">
  <p><em>List Available Roles</em></p>
</div>

Available clients: 'elfmonitor', 'WomblyC', 'AlabasterS'

Available roles: 'SiteDefaultPasswordRole', 'SiteElfMonitorRole', 'SiteAlabsterSAdminRole', 'SiteWomblyCAdminRole'

The data suggests that the username is elfmonitor, especially since Ribb mentioned not to use Alabaster or Wombley's credentials. However, the real challenge is determining the password for elfmonitor.
After some crazy attempts, I noticed Santa's elves were up to some mischief... they were playing with me and hid the password in plain sight! I discovered that the List of Available Roles held the key to unlocking the mystery:

Username: elfmonitor
Password: SiteElfMonitorRole

Adding the additional details activates the monitors. Note: The port required to connect to the monitors is **9001**.<br>
Once the monitors are on, we can connect to the `northpolefeeds` topic and we can observe the images being broadcasted by Alabaster and Wombley, as shown below:

![Santa Vision northpolefeeds images](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-23.png)

When you correctly connect to the monitors, the following message will be displayed in the northpole feed when connected to MQTT:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
./static/images/monitor1.png,./static/images/monitor2.png,./static/images/monitor3.png,./static/images/monitor4.png,./static/images/monitor5.png,./static/images/monitor6.png,./static/images/monitor7.png,./static/images/monitor8.png
</pre>

It is also possible to access the images by directly accessing the URL. For example:

http://34.59.164.209:8000//static/images/monitor1.png

Answer for Santa Vision B Silver: **elfmonitor**.

##Santa Vision C - Silver

![Santa Vision C Objective](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-14.png)

As before, let's talk to Ribb to see what he has to say now that we completed Santa Vision B:

<div class="conversation">
Excellent progress! You've moved us closer to understanding this network—keep it up on the silver path!

You're doing fantastic! The northpolefeeds are now in your sights. Silver or gold, you're pushing forward with great momentum!

We're getting closer. Now, we need to dig into the frostbitfeed. It’s time to figure out if any other feeds are lurking beneath the surface—and uncover the elves' secret operation.
</div>

Based on the solution for **Santa Vision B**, we can connect to the MQTT service using the **elfmonitor** credentials. I downloaded and configured MQTTX to establish the connection.
Once connected, subscribing to the `frostbitfeed` topic allows us to receive all messages broadcast to it. Below is an example of some of the messages retrieved from the feed:

![frostbitfeed topic messages](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-16.png)

The feed reveals two critical pieces of information:

1. To resolve Santa Vision C, we can see there is another feed available: `santafeed`.
2. A message from the feed contains the following payload:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
"payload": "Error msg: Unauthorized access attempt. /api/v1/frostbitadmin/bot/<botuuid>/deactivate, authHeader: X-API-Key, status: Invalid Key, alert: Warning, recipient: Wombley"
</pre>

We can ignore this second piece of information for now, but we will need it to solve the objective “Deactivate Frostbit Naughty-Nice List Publication”.

Coming back to Santa Vision C, the challenge asks if other feeds are available, and it is clear that this refers to the `santafeed` topic. Subscribing to it provides access to its messages. After some time, one of the messages includes the answer:

<div style="text-align: center;">
  <img src="/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-17.png" alt="Available Roles" border-radius: 10px;">
  <p><em>Message from santafeed with the answer</em></p>
</div>

Answer for Santa Vision C Silver: **Idemcerybu**

##Santa Vision D - Silver

![Santa Vision D Objective](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-18.png)

Ribb! Talk to me now that C is completed!

<div class="conversation">
Wonderful job! You've uncovered a critical piece of the puzzle—well on track with the silver approach!

You're almost there! The operation’s code is unlocked, but the final challenge is waiting. Silver or gold, you're close to victory!

It’s time to take back control of the Santa Broadcast Network. There really shouldn't be multiple administrators—send the right message, and Santa’s true spirit will return. What’s Santa test-driving this season?
</div>

While reviewing the `santafeed` messages, one in particular caught my attention: 

```
"singleAdminMode=false\n"
```

After seeing this, I immediately thought that the solution was to set the parameter to 'true'. I began trying to send JSON formatted messages, but they were failing. Then, I realized that in MQTTX, you can select different formats for the messages you receive. When I switched the messages to JSON format, I got an error. This made me realize that the solution was to send the following message as plain text to the 'santafeed' topic:

```
singleAdminMode=true
```

That immediately triggered a new message from northpolefeeds, indicating it was successful:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-1.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-2.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-3.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-4.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-5.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-6.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-7.png,./static/images/hhc2024santatopsecreteasyimages376919542/santa376919542-8.png
</pre>

![Santa Vision singleAdminMode=true](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-19.png)

Looking at the images, plus some Google-Fu to find out how the heck that 'contraption' was called in English, got me the final answer to complete Santa Vision Silver Objective:

Answer for Santa Vision D Silver: **pogo stick**

![Santa in pogo stick](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-20.png)


#**Solution - Gold**

**Note**: The questions are the same as for Silver, so I won't repeat them here.

##Santa Vision A - Gold

It turns out that some of the initial hints were for the Gold path. In particular, [Hint 3 regarding jefferson](#hint-3-filesystem-analysis) and [Hint 4 regarding checking DBs for credentials](#hint-4-database-pilfering)

After completing Santa Vision A Silver, Ribb gives us an interesting hint for the Gold path:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
(Gold hint) Stay curious. Sometimes, the smallest details—often overlooked—hold the keys to the kingdom. Pay close attention to what’s hidden in the source.
</pre>

The clue highlights the importance of disregarding the data known from the Silver path and approaching the challenge as if starting from scratch. It took me a bit of time to realize that I should have been looking at the problem without assuming I already had credentials. 

Once I had this in mind, I revisited the login page and spotted the following message at the bottom of the page. In fact, I initially discovered it by intercepting the server response in Burp, as the hint suggested checking the source code:

![Santa Vision Login page](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-10.png)

The message mentions the topic `status` suggesting we can subscribe to it. I did so and started receiving messages from that feed. Inspecting the messages, we can see the following one:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
File downloaded: /static/sv-application-2024-SuperTopSecret-9265193/applicationDefault.bin
</pre>

![frostbitfeed topic messages - gold](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-11.png)

Let's get the file:

http://34.59.164.209:8000/static/sv-application-2024-SuperTopSecret-9265193/applicationDefault.bin

I started by inspecting the file with the command 'file'. The result indicates that the file is a jffs2 filesystem:

![file](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-12.png)

Given the hint from Ribb, the way forward seems clear. Let's explore the file using 'jefferson':

```
jefferson applicationDefault.bin -d jefferson
```

The output turned out like to be the source code for the Santa Vision portal. I spent some time inspecting the code and there were multiple interesting pieces of information on it. Focusing on the data relevant to solving Santa Vision Gold, I came across the following credentials in multiple places within the `/app/src/core/views.py` file:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
def deleteBrokerClients(name): #Delete Player Broker Clients
    try:
        mqttPublish.single("$CONTROL/dynamic-security/v1","{\"commands\":[{\"command\": \"deleteClient\",\"username\": \""+name+"\"}]}",hostname="localhost",port=1883,auth={'username':"SantaBrokerAdmin", 'password':"8r0k3R4d1mp455wD"})
</pre>

The credentials `("SantaBrokerAdmin" / "8r0k3R4d1mp455wD")` worked to connect to the MQTTX service.

Additionally, I found a reference to a database file in the `/app/src/accounts/views.py` file:

```
/sv2024DB-Santa/SantasTopSecretDB-2024-Z.sqlite
```

I downloaded the file and opened it using DB Browser for SQLite, though other tools like sqlite3 from the command line could also be used. Upon inspecting the database, I found Santa's password stored there:


![Santa Broker pwd](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-13.png)

```
Username: santaSiteAdmin
Password: S4n+4sr3411yC00Lp455wd
```

This user has administrative privileges in the Santa Vision portal and it's what we need to complete Santa Vision Gold A:

Answer for Santa Vision A Gold: **santaSiteAdmin**

##Santa Vision B - Gold

Ribb gave us the following hint for Santa Vision B:

[Hint 5 - Like a Good Header on Your HTTP?](#hint-5-like-a-good-header-on-your-http)

And this is what he tell us during our conversation with him (after Santa Vision B Silver is completed):

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
(Gold hint) Look beyond the surface. Headers and subtle changes might just open new doors. Pay close attention to everything as you log in.
</pre>

Given the hint, I logged in into the Santa Vision portal using the newly discovered credentials (username: `santaSiteAdmin` / password: `S4n+4sr3411yC00Lp455wd`).

Below is the server's response after successfully logging in:

![Headers](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-15.png)

The server's response headers include the following parameters (**note**: the password is regenerated for each session):

```
BrkrUser: santashelper2024
BrkrPswd: playerSantaHelperPass4196705495
```

The credentials work for switching on the monitors and see the images broadcasted by Alabaster and Wonbley, similar to the Silver objective. This time, we use the following information to switch on the monitors:

```
BrkrUser: santashelper2024
BrkrPswd: playerSantaHelperPass4196705495
Camera Feed Server: <IP from GateXOR>
Camera Feed Port: 9001
```

Once the monitors are on, we can connect to the `northpolefeeds` as done previosuly.

Answer for Santa Vision B Gold: **santashelper2024**

##Santa Vision C - Gold

We get the following hint from Ribb after completing the silver path:

[Hint 6 - Looking Deeper](#hint-6-looking-deeper)

And as usual, his dialogues are also helpful:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
(Gold hint) Sometimes the answers are in the quiet moments. Pay attention to every feed and signal—you may find what you're looking for hidden deep in the streams.
</pre>

Solving Santa Vision C Gold is essentially the same as solving Santa Vision C Silver. The process involves: 

* Connecting to the `frostbitfeed` (using santashelper2024 to connect to MQTT).
* Discovering the `santafeed` topic within the messages.
* Subscribing to `santafeed` and retrieving the key message:

```
Sixteen elves launches operation: Idemcerybu
```

The key now is in the hint: *“but will you see it?”*. The answer is easy, but took me some time to realize the obvious. After some time, I was reviewing my notes for other challenges. I randomly came across my notes for the Elf Connect objective. There, I noticed the following list of cipher methods used in the third round of the game:

[Caesar, One-time Pad, Ottendorf, Scytale]

Given that the Ottendorf cipher was used for the Frosty Keypad objective, made me think that maybe the answer was the Caesar cipher. Sure enough, deciphering `Idemcerybu` with a Caesar cipher key of 10 revealed the answer: 'Snowmobile'.

Answer for Santa Vision C Gold: **Snowmobile**

##Santa Vision D - Gold

Last time our friend Ribb gets to give us a hint this year:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
(Gold hint) Think about the kind of ride Santa would take in a world filled with innovation. His vehicle of choice might surprise you—pay attention to the futuristic details.
</pre>

The Santa Vision D Gold objective closely mirrors its Silver equivalent. To solve it, the same steps as for silver are required, but using the `santahelper2024` credentials instead:

* Loggin with the santahelper2024 credentials in the MQTT service.
* Send the same message as before to the santafeed topic using the santahelper2024 account:

```
singleAdminMode=true
```

The monitors will start loading with the following images:

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
./static/images/hhc2024santatopsecretimages835826406/santa835826406-1.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-2.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-3.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-4.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-5.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-6.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-7.png,./static/images/hhc2024santatopsecretimages835826406/santa835826406-8.png
</pre>

![Santa hovercraft](/images/HHC/2024/Act%203/Santa%20Vision/SantaVision-21.png)

Answer for Santa Vision D Gold: **hovercraft**


#Final Message from Ribb Bonbowford

<pre style="white-space: pre-wrap; word-wrap: break-word; background-color: #f5f5f5;">
Excellent! You've successfully removed the propaganda and restored the true spirit of the season. A solid silver finish—well done!

Mission accomplished! The airwaves are restored, and the message is one of unity and teamwork. Whether silver or gold, you've done an incredible job!
</pre>

#Challenge Steps Summary

Silver:

* Santa Vision A: elfanon/elfanon
* Santa Vision B: elfmonitor/SiteElfMonitorRole (connect to northpolefeeds)
* Santa Vision C: connect to frostbitfeed, connect to santafeed Answer: Idemcerybu
* Santa Vision D: send plain text message to santafeed: singleAdminMode=true

Gold:

* Santa Vision A: santaSiteAdmin/S4n+4sr3411yC00Lp455wd (using Jefferson and DB)
* Santa Vision B: When you log in with Santa's creds (santaSiteAdmin/S4n+4sr3411yC00Lp455wd), you get the following headers: santashelper2024/playerSantaHelperPass9058336588 - this password changes every instance
* Santa Vision C: Idemcerybu with Caesar cipher key of 10 = Snowball
* Santa Vision D: same as silver but using santashelper2024 creds.
