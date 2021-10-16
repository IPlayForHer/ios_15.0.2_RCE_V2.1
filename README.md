
# iOS 15.0.2 RCE v2.1 Airdrop Delivered Data Wipe
### Author: Jonathan Scott  @jonathandata1
### Date: October 16th, 2021
![iOS 15.0.2 RCE V2](https://i.postimg.cc/7YdWJrQB/Untitled-design-Max-Quality-2021-10-15-T195750-817.jpg)

## Description
### This exploit is based off of RCE V1 that I posted, but works in a bit of a different way...We were focused on being local before, but now we are moving in a bit of a different direction. More of a MISDIRECTION 

## Tools Needed

 - Apple Configurator 2
 - Install automation tools, when you have Apple Configurator 2 open, at the top left click on the "Apple Configurator 2" menu item, then click on "install automation tools" This is going to install binaries you can run in your terminal.
 - `brew install websocketd`
 - `brew install ngrok`
 - iPhone 13 Pro - iOS 15.0.2 (target phone)
 - iPhone 11 Pro - iOS 15.0.2 (evil phone)
 **- You can use a mix of different phones this exploit will work on iPhone 5s - iPhone 13 Pro Max iOS 9.0-15.0.2**
 
 ## Setup & Discussion
 
 Just like in RCE V1, you will need to make sure that the target phone is plugged into the target MacOS computer, and make sure the device is trusted or "paired" with the computer. 

You also need to make sure that automation tools are installed on the target computer. This PoC is still a little on the "light" side of things, but we will get more invasive in our next PoC, we won't need any cables at all in our next one.

Similarly, I am not going to tell how how to scan your network and find the hosts on our network so that you you can do a do a websocket redirect from your computer to the target computer because we're still not ready for that yet...one step at a time so that you can master how this websocket exploit works. 

I am performing what I am calling a "True Trust" Exploit, these are the most dangerous exploits in the world as I have mentioned in my Twitter feed. You have full trust, and full ability to perform whatever actions you want on the target device because you are abusing the trust relationship the device and the connected host have. You are forcing the host to perform actions on the target device without your knowledge or consent. 

**Ok so lets establish the official setup for this PoC**
1. Make sure both the target phone and the evil phone are connected to the same Wi-Fi network
2. You do not have to be signed in to icloud at all, icloud account are NOT needed on any of the phone or the host machine, this will work with all iclouds OFF, iCloud does not have anything to do with this exploit
3. You are running this exploit for your own testing purposes so that you can master how websockets work, and build onto this exploit
4. You are going to be running the websocket on your own machine which will also be your target machine
5. Automation tools have already been install on your machine/target machine because this PoC , again is preparing you for the next one that will not involve the target machine being used. 
6. You will need to setup ngrok, follow the instructions after setup and make sure you are configured properly go to [ngrok.com](https://www.ngrok.com) for setup and instructions
7. In this PoC I am using an upgraded package for ngrok that allows me to have my own custom subdomain. 
![iOS 15.0.2 RCE V2](https://i.postimg.cc/15Fj65V8/Screen-Shot-2021-10-16-at-3-54-44-AM.png)

Ngrok is going to forward your localhost websocket to a URL that can be pulled up anywhere in the world, so for example in RCE V1 we were using 0x.local:8081 to connect, and send the exploit through the localhost. 

We are still sending the exploit through the localhost, but it will look like we are sending it from a secured website. https://jonathanscott.ngrok.io

## Running the exploit

1. Plugin the target phone (iPhone 13 Pro iOS 15.0.2) into your Macbook Pro via usb
2. If a trust prompt pops up, make sure to trust it
3. If you have a passcode, you will need to enter your passcode and trust with the Macbook Pro or Mac mini, etc..
4. Open a terminal and cd into this repo
5. `sh index.sh`
6. Split your terminal or open up another one and run ngrok
7. `ngrok http http://0x.local:8081`
8. Change 0x.local to your localhost name, or you an just leave it as localhost:8081
9. If you are using ngrok premium the code execution will look more like this
10. `ngrok http --region=us --hostname=jonathanscott.ngrok.io 8081`
11. Make sure that your websocket and ngrok are both running on port 8081 for this to work

***Your terminal split could look something like this....***
![iOS 15.0.2 RCE V2](https://i.postimg.cc/VvXs3PvD/Screen-Shot-2021-10-16-at-4-05-44-AM.png)

12. Now that you have ngrok running on a public domain on the evil phone you can go to the forwarding URL, so for example on the evil phone you would go to https://jonathanscott.ngrok.io, you need to go to this site so that you can send it to your target phone.
13. In safari share this website via Air Drop with the target device...please be mindful that for this PoC, the target device must be plugged into the host computer the entire time so that the exploit can execute over usbmuxd
14. The target phone will have to accept the airdrop message from you, it will not automatically open, but once the target sees the cute puppy! Yes of course I want to open it!
15. This part involves a bit of social engineering...Hey friend, click that puppy video i just sent you...its hilarious!!!
16. Once the target taps on the puppy, the data wipe starts, it does not matter if the device has a passcode on it, it does not matter if the device has an icloud on it, this data erasure will overwrite those controls.
17. The public domain calls back to the localhost, and the command
18. `cfgutil erase` **is executed on the localhost, once it is executed, if you have no knowledge of the attack stopping it is not possible.** 

**Note: Normally when performing a factory reset on an iPhone, you will be asked to remove your iCloud account, enter your passcode...etc...this is for security purposes so that not just anyone can data wipe your phone...in this scenario, all guards are DOWN!!!**

**NOTE: This is still a local attack, the next PoC will be a lot more invasive and not local.**

## Attack Scenario

 - Home attacks 
 - Work Attacks 
 - School Attacks 
 - Anywhere that you can access
   a host, setup the host, and make sure the target device is trusted

## Diagram for RCE V2.1 PoC True-Trust Vulnerability
![iOS 15.0.2 RCE V2](https://i.postimg.cc/Hk7PwWJJ/RCE-v2-1.jpg)

