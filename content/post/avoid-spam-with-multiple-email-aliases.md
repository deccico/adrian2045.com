+++
title = "Avoid Spam With Multiple Email Aliases"
date = "2025-01-22"
tags = [
    "productivity",
    "privacy",
    "email",
    "code",
    "python",
]
thumbnail = "/images/posts/avoid-spam.webp"
+++
Here I introduce my solution to deal with multiple newsletters and services requesting your email address while keeping 
your email private and safe. 
<!--more-->


![](/images/posts/avoid-spam.webp)[^1]
[^1]: Image design: Dall-E

# Introduction and Motivation

More information means more power and money and big entities are always hungry for it. This includes advertisement 
companies such as Google, marketing and sales organisations, spammers, governments, etc. From a personal perspective 
avoiding to be profiled and your data sold to the next bidder is not great. But how to avoid this while also living a 
normal life? How can we take advantage of the multiple services the modern life offers us? With easy to create Infinite 
Email Aliases, of course! While not a perfect solution, it at least can help us keep our email addresses private. 
Receiving less spam and being able to focus more. 

Computer privacy and security are very hard. Borderline impossible. But despair not! Here I introduce an easy solution 
for keeping your personal email address safe while being able to subscribe to newsletters and services to your hearts 
content. While computer security is never perfect, adding multiple security layers can make all the difference to keep 
bad actors in check or at least makes it more expensive to bypass our controls. Onion layers FTW!

# Requirements

What do we need? We will use a Namecheap account with a special domain we just use for forwarding emails. If privacy is 
your intention, it's better to have a separate, special domain for this purpose. Alternatively you could use another
email service provider that has an API and allows to setup redirection.

# How does it work?

![](/images/posts/email-fwd.webp)


We will make use of Namecheap API to setup email redirections. All emails aliases will go to one, single email address.
Of course it's possible to setup different ones if your use case is different.

My recommendation is to add multiple numbers in order to have a "premade" list of email aliases that we can just give 
on the fly, say from 1 to 100 and also setup particular ones to match the service we are utilising. For example:

Let's say our private domain for email aliases is myfwd.com Then we will have:

1@myfwd.com -> adrian@gmail.com<br>
2@myfwd.com -> adrian@gmail.com<br>
3@myfwd.com -> adrian@gmail.com<br>
..<br>
99@myfwd.com -> adrian@gmail.com<br>

Also:

council@fwd.com -> adrian@gmail.com<br>
bank@fwd.com -> adrian@gmail.com<br>
newsletter@fwd.com -> adrian@gmail.com<br>


So the first set would be "premade" while the special, name aliases would need to be added to be created on demand.


# Show me the code

In the code below I'm setting up 50 email aliases. You would need to setup Namecheap api, whitelist your IP and obtain
the api keys. All that information in <b>credentials.py</b> needs to be completed.

You can also access the code here: https://github.com/deccico/aliar/tree/main

## credentials.py 

<b>credentials.py</b> contains the main variables and secret keys.

```python
api_user = ''
api_key = ''
username = ''
client_ip = ''
domain = 'your-email-alias-domain.com'
alias_email = 'yourname@gmail.com'
```


## aliar.py

This is the main code that you run and where you define the email aliases. 1..50 aliases are created below:

```python
import requests

# Replace these variables with your actual values
from credentials import *

# Namecheap API endpoint for adding an email alias
url = 'https://api.namecheap.com/xml.response'

# Parameters for the API call
params = {
    'ApiUser': api_user,
    'ApiKey': api_key,
    'UserName': username,
    'ClientIp': client_ip,
    'Command': 'namecheap.domains.dns.setEmailForwarding',
    'mailbox1': '1', 'ForwardTo1': alias_email,
    'mailbox2': '2', 'ForwardTo2': alias_email,
    'mailbox3': '3', 'ForwardTo3': alias_email,
    'mailbox4': '4', 'ForwardTo4': alias_email,
    'mailbox5': '5', 'ForwardTo5': alias_email,
    'mailbox6': '6', 'ForwardTo6': alias_email,
    'mailbox7': '7', 'ForwardTo7': alias_email,
    'mailbox8': '8', 'ForwardTo8': alias_email,
    'mailbox9': '9', 'ForwardTo9': alias_email,
    'mailbox10': '10', 'ForwardTo10': alias_email,
    'mailbox11': '11', 'ForwardTo11': alias_email,
    'mailbox12': '12', 'ForwardTo12': alias_email,
    'mailbox13': '13', 'ForwardTo13': alias_email,
    'mailbox14': '14', 'ForwardTo14': alias_email,
    'mailbox15': '15', 'ForwardTo15': alias_email,
    'mailbox16': '16', 'ForwardTo16': alias_email,
    'mailbox17': '17', 'ForwardTo17': alias_email,
    'mailbox18': '18', 'ForwardTo18': alias_email,
    'mailbox19': '19', 'ForwardTo19': alias_email,
    'mailbox20': '20', 'ForwardTo20': alias_email,
    'mailbox21': '21', 'ForwardTo21': alias_email,
    'mailbox22': '22', 'ForwardTo22': alias_email,
    'mailbox23': '23', 'ForwardTo23': alias_email,
    'mailbox24': '24', 'ForwardTo24': alias_email,
    'mailbox25': '25', 'ForwardTo25': alias_email,
    'mailbox26': '26', 'ForwardTo26': alias_email,
    'mailbox27': '27', 'ForwardTo27': alias_email,
    'mailbox28': '28', 'ForwardTo28': alias_email,
    'mailbox29': '29', 'ForwardTo29': alias_email,
    'mailbox30': '30', 'ForwardTo30': alias_email,
    'mailbox31': '31', 'ForwardTo31': alias_email,
    'mailbox32': '32', 'ForwardTo32': alias_email,
    'mailbox33': '33', 'ForwardTo33': alias_email,
    'mailbox34': '34', 'ForwardTo34': alias_email,
    'mailbox35': '35', 'ForwardTo35': alias_email,
    'mailbox36': '36', 'ForwardTo36': alias_email,
    'mailbox37': '37', 'ForwardTo37': alias_email,
    'mailbox38': '38', 'ForwardTo38': alias_email,
    'mailbox39': '39', 'ForwardTo39': alias_email,
    'mailbox40': '40', 'ForwardTo40': alias_email,
    'mailbox41': '41', 'ForwardTo41': alias_email,
    'mailbox42': '42', 'ForwardTo42': alias_email,
    'mailbox43': '43', 'ForwardTo43': alias_email,
    'mailbox44': '44', 'ForwardTo44': alias_email,
    'mailbox45': '45', 'ForwardTo45': alias_email,
    'mailbox46': '46', 'ForwardTo46': alias_email,
    'mailbox47': '47', 'ForwardTo47': alias_email,
    'mailbox48': '48', 'ForwardTo48': alias_email,
    'mailbox49': '49', 'ForwardTo49': alias_email,
    'mailbox50': '50', 'ForwardTo50': alias_email,

    'mailbox1000': 'bank', 'ForwardTo1000': alias_email,
    'mailbox1001': 'council', 'ForwardTo1001': alias_email,
    'mailbox1002': 'elctricity', 'ForwardTo1002': alias_email,

    'DomainName': domain
}

# Make the API call
response = requests.post(url, params=params)

# Check the response
if response.status_code == 200:
    print("Success: ", response.text)
else:
    print("Error: ", response.status_code, response.text)

```

# Limitations

While I have run successfully this setup for many months I noticed some limitations:

## Answering to emails

In case you need to answer to an email sent to any of your aliases, you would be responding from your main, private 
email. While this might be OK, you can also setup an email service for that particular address in Namecheap. While 
aliases are free, hosting email start from around $1 per month depending on the amount of inboxes you wish to maintain.

## Time to receive emails

A forwarding might introduce an additional delay, although in the practice I could hardly notice any. I found only
one service incapable of sending emails to my alias email. This should become very obvious and easy to detect. 
Unfortunately in those cases we might need to give up on the service or use another address.

## IP Addition

Namecheap does require to whitelist IPs. This means that every now and then we might need to whitelist a new IP
in case we reset our router or our ISP decides to rotate it.

# Benefits

Infinite, free and private email addresses! I am no longer hesitant to give my email thinking in all the spam I might 
receive. The code is easy to run for new addresses and the API is very safe. There are many other solutions but this is
the only one where I have full control and is also free to implement. 

Additionally, if you receive spam from one of your aliases, you will immediately know who sold you. If one email 
becomes very spammy (it happens) you can very easily comment out the code and disable the email. One word of caution:
don't add or remove aliases manually in the Namecheap dashboard. Either use the api or the web UI.

# Summary

In this post I present my solution to increase privacy in a meaningful and easy eay at not cost other than the domain
we are using.
