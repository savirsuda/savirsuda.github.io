---
layout: post
title: How I was able to Retrieve your Personal Documents using the Wayback Machine!
---


Hi guys, I am back with another blog. It has been a long time since I released anything, so here I am. This one was a really interesting vulnerability and it was pretty fun to exploit it. Without wasting any more of your time, lets get into the bug :)

**Summary**


Lets call the target website as **shortener-example.com** to respect the privacy of the company. Let me brief you about the website's main feature. The Main website of the company was **www.example.com** and the domain I found this vulnerability on(shortener-example.com) was the company's custom URL shortener.
The `shortener-example.com` domain was responsible for shortening all the links sent by users of the application to each other in private chat on `www.example.com`. Due to a misconfiguration in the way this was implemented, I was able to retrieve the personal documents that a user would send to another user in personal chats.


**Description**

This domain, `shortener-example.com` gathered all the links sent by users on `www.example.com` and would shorten those links. They did this to decrease the load or to maximize storage.
When I later digged deep into how this domain worked, I found out that this domain would collect all the links sent by users to eachother in private chats(Google Drive Links, Unlisted Youtube Video Links, etc) and after shortening those links, the shortened link would look somewhat like `http://shortened-example.com/<SOME-RANDOM-TOKEN>`.
I then found out that this domain would **Index those shortened URLS**. This means that these shortened links would get crawled by search engines. Example of these "search engines" - Google, Wayback Machine, etc.
When I noticed this, I quickly thought of using a tool called [waybackurls](https://github.com/tomnomnom/waybackurls) by [@TomNomNom](https://twitter.com/TomNomNom) to find these Shortened Indexed URLs.
When I wrote the command `echo "shortener-example.com" | waybackurls"` , I saw thousands of shortened links in my terminal.

As I visited them one by one, they opened into full actual URLs that they originally were. I found a lot of interesting URLs like links to Private documents such as documents hosted on Google Drive, Unlisted Youtube Videos, Pastebin links, etc. 
I remember that I found a Google Drive link which was hosting a PDF file that was full of PII information (Phone Numbers, Email Addresses, First and Last names, etc). 

Let me explain a bit more about this bug - So basically, whenever a user sends a link to another user(This link can be any link at all. It can be a link to Documents hosted on the internet and it can also just links to regulat websites.) the domain `shortener-example.com` gets that link and shortens it. After shortening this link, the domain indexes those urls allowing web crawlers to find those links. Now take a minute to think about this situation. You send a link to your teacher(It could be a Google Drive Link to suppose your medical report) and your teacher receives it, end of the matter. Well, he only thing it is an end to is your **privacy** because by sharing this link to your medical report with just your teacher, you have made it public on the Internet.

**Impact**




![_config.yml]({{ site.baseurl }}/images/dab.gif)


**Resources, Credits and Shoutouts**

Thanks for Reading this, hope you enjoyed my first blog :)

Many more to come xD

Until next time!


-savxiety
