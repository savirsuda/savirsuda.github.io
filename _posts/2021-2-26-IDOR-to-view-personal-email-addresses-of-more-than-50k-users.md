---
layout: post
title: IDOR which allowed me to view Personal Email Addresses of More than 50K Users! 
---


This is My First Blog and I am really excited about it at the time of writing this. I have been trying to take out time to write this finding up for a while now but haven't been able to do so because of other important things, anyways finally here we are. Without wasting any more of your time, lets get into the bug :)

**Summary**


Lets call the target website as **example.com** and the domain on which this vulnerability was found as **subdomain.example.com** for the Privacy of the company. The Vulnerability exists in the Forgot-Password Page of **subdomain.example.com**. Basically, while requesting a password change for a user,  a **GET** parameter was sent. I was able to manipulate this parameter and was able to view the Personal Email addresses of probably all of the registered users on the website. This was a pretty simple bug and a typical case of an [IDOR](https://portswigger.net/web-security/access-control/idor) .



![_config.yml]({{ site.baseurl }}/images/idor-img.PNG)


**Description**


One day, I was checking the results of my Recon which was done using my custom bash script and while going through the subdomains it had found, I noticed an interesting subdomain. Lets call the subdomain as `subdomain.example.com`. I started crawling the subdomain to find all of its [URLS](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL) using an awesome tool made by [@TomNomNom](https://twitter.com/tomnomnom) called `waybackurls`. I am not going to explain how this tool works or what it is, you can find the Link to it's public Github Repository at the end of this Blog post. Basically I found an Interesting URL with `waybackurls` which is:
`https://subdomain.example.com/accounts/Directory/XYZ/Form.aspx`. This endpoint was actually a Forgot Password Page and it allowed users to request a password reset. Nothing fancy or alerting right now but lets see what happens later ;) The extension of this page was `.aspx` which made me Directory Brute force for other `.aspx` files. Unfortunately I did not find anything and eventually gave up on Directory brute forcing. Now I thought of using Google Dorks on this subdomain. I used the following Google Dork:


`site:subdomain.example.com inurl:id`


When I did this, I got an endpoint as a result of the search. This endpoint contained `ID` as a `GET` parameter in the URL with some other irrelevant parameters. The final URL looked like this: `https://subdomain.example.com/accounts/Directory/XYZ/Form.aspx?ID=1&ContentTypeId=123`
The interesting part was that in the response, when I scrolled down, I could see a user's personal email address. I got the following response:


`Email Sent successfully to redacted@gmail.com`



The HTTP Response looked like this:

```php
HTTP/1.1 200 OK
Date: <SOME-RANDOM-DATE>
Content-Type: text/html; charset=utf-8
Content-Length: 11163
Connection: close
Cache-Control: private, s-maxage=0
Vary: Accept-Encoding
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Content-Security-Policy: default-src 'self' 'unsafe-inline'
Referrer-Policy: same-origin

Email Sent successfully to redacted@gmail.com
```
I found this very interesting and as a result I got excited. I knew exactly what to do, I changed the value of the `ID` parameter to another random numeric ID and yes, I got another private email address of a user! This time I had changed the ID from `1` to `2`. I noticed that even 4 digit long numeric IDs were tied to a user's account. Now, it was time of a nice Proof Of Concept to increase the impact. I fired up an Intruder Window in BurpSuite, configured it to Brute-Force the value of the `id` parameter that was found earlier in the URL, and started the Attack. I looked at the Intruder again after letting it run for a few minutes and was surprised by being presented to more than 50,000 private email addresses of the users!

![_config.yml]({{ site.baseurl }}/images/what.gif)


This means that with the change of the ID parameter I can leak the private emails of users of this web application. Basically, this means that the ID's value 1 is linked with an email that is `redacted@gmail.com` and the ID's value of 2 is linked with the email `redacted2@gmail.com`.

Basically this Attack worked because the application assigned every user's account with a numeric **ID** from `1` till `50,000`. The `ID` of a user could have been guessed easily as this application kept the IDs sequential meaning that if two users sign up, User A and User B, User A signs up a minute before than User B, then User A will be asigned with an ID of `1` and User B would be assigned with an ID of `2`. Intruder Brute-forced all the IDs till 50,000 and since these IDs were sequential, this meant there were 50K users signed up on the web application but more importantly, an attacker would be able to view the **Private Email Addresses of More than 50K Users!** 


**Impact**


By exploiting this IDOR vulnerability an attacker is able to leak private email addresses of about 50,000 users which is a clear privacy violation for the users. The attacker can store these email addresses in a database and plan future attacks on them.

- To Leak the Private Email Address of a User, no user interaction was required at all.
- The ID parameter was vulnerable to IDOR attacks.
- The ID parameter had a numeric ID with which objects(emails) were referenced.
- The value of the ID parameter was predictable and an object is linked in an increasing value. In this case, these IDs with values from 1,2,3,4,5,6,7 and so on was referred to as an object(user email).

Due to these points, the severity of this vulnerability was **High**.


![_config.yml]({{ site.baseurl }}/images/dab.gif)
