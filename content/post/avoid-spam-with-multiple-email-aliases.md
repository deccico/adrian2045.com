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

Here I introduce my solution to deal with multiple newsletters and services requesting your email address while keeping your email private and safe.
<!--more-->

![](/images/posts/avoid-spam.webp)[^1]
[^1]: Image design: Dall-E

# Introduction and Motivation

More information means more power and money, and big entities are always hungry for it. This includes advertisement companies such as Google, marketing and sales organisations, spammers, governments, etc. From a personal perspective, being profiled and having your data sold to the highest bidder is far from ideal. But how do we avoid this while living a normal life? How can we take advantage of the many services modern life offers us? The answer: easy-to-create infinite email aliases! While not a perfect solution, this approach can help us keep our email addresses private, receive less spam, and focus more on what matters.

Computer privacy and security are very challenging. Borderline impossible. But despair not! Here, I introduce an easy solution for keeping your personal email address safe while subscribing to newsletters and services to your heart’s content. While computer security is never perfect, adding multiple security layers can make all the difference in keeping bad actors in check or at least making it more expensive for them to bypass our controls. Onion layers FTW!

# Requirements

What do we need? We will use a Namecheap account with a special domain solely for forwarding emails. If privacy is your intention, it's better to have a separate domain for this purpose. Alternatively, you could use another email service provider that has an API and allows for redirection setup.

# How Does It Work?

![](/images/posts/email-fwd.webp)

We will use the Namecheap API to set up email redirections. All email aliases will forward to one main email address. Of course, it’s possible to set up different forwarding addresses if your use case requires it.

I recommend adding multiple numbers to create a "premade" list of email aliases that you can provide on the fly (e.g., 1 to 100), as well as setting up specific ones to match the services you use. For example:

Let’s say our private domain for email aliases is **myfwd.com**. Then we would have:

```
1@myfwd.com → adrian@gmail.com
2@myfwd.com → adrian@gmail.com
3@myfwd.com → adrian@gmail.com
...
99@myfwd.com → adrian@gmail.com
```

Additionally:

```
council@myfwd.com → adrian@gmail.com
bank@myfwd.com → adrian@gmail.com
newsletter@myfwd.com → adrian@gmail.com
```

The first set would be "premade," while the specific aliases could be created on demand.

# Show Me the Code

In the code below, I set up 50 email aliases. You will need to configure the Namecheap API, whitelist your IP, and obtain the API keys. Complete all the information in **credentials.py**.

You can also access the code here: [GitHub Repository](https://github.com/deccico/aliar/tree/main)

## credentials.py

**credentials.py** contains the main variables and secret keys.

```python
api_user = ''
api_key = ''
username = ''
client_ip = ''
domain = 'your-email-alias-domain.com'
alias_email = 'yourname@gmail.com'
```

## aliar.py

This is the main code that you run to define the email aliases. Below, 1 to 50 aliases are created:

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

While I have successfully used this setup for months, I’ve noticed some limitations:

## Replying to Emails

In case you need to answer to an email sent to any of your aliases, you would be responding from your main, private 
email. While this might be OK, you can also setup an email service for that particular address in Namecheap. While 
aliases are free, hosting email start from around $1 per month depending on the amount of inboxes you wish to maintain.

## Time to receive emails

Forwarding may introduce a slight delay. However, in practice, I’ve hardly noticed any delays. Only one service has been unable to send emails to my alias, which became immediately apparent. In such cases, you might need to use a different, non forwarded address or give up on that service.

## IP Whitelisting

Namecheap requires IP whitelisting. This means that occasionally, you may need to whitelist a new IP if you reset your router or your ISP rotates your IP address.

# Benefits

Infinite, free, and private email addresses! I am no longer hesitant to share my email address for fear of spam. The code is easy to run for new addresses, and the API is very secure. While there are other solutions, this one offers full control and is free to implement.

Additionally, if you receive spam from one of your aliases, you will know exactly who sold your information. If an email alias becomes too spammy, you can easily disable it by commenting out the relevant code. One word of caution: avoid manually adding or removing aliases in the Namecheap dashboard. Either use the API or the web UI.

# Summary

In this post, I presented a solution to significantly enhance your privacy in an easy, cost-effective way. The only expense is the domain you use for forwarding emails.

