---
layout: post
title: How I was able to Retrieve your Personal Documents using the Wayback Machine!
---


Hi guys, I am back with another blog. It has been a long time since I released anything, so here I am. This one was a really interesting vulnerability and it was pretty fun to exploit it. Without wasting any more of your time, lets get into the bug :)

**Summary**

This was a pretty unique finding and I haven't seen any reports on such a topic other than one disclosed by [@nagli](https://twitter.com/naglinagli). 


Lets call the target website as **shortener-example.com** to respect the privacy of the company. Let me brief you about the website's main feature. The Main website of the company was **www.example.com** and the domain I found this vulnerability on(shortener-example.com) was the company's custom URL shortener. The main website, `www.example.com`, was a web application meant for Teachers, Students and Parents to connect with each other.
The `shortener-example.com` domain was responsible for shortening all the links sent by users of the application to each other in private chat on `www.example.com`. Due to a misconfiguration in the way this was implemented, I was able to retrieve the personal documents that a user would send to another user via links in personal chats.


**Description**

This domain, `shortener-example.com` gathered all the links sent by users on `www.example.com` and would shorten those links. They did this to decrease the load or to maximize storage.
When I later digged deep into how this domain worked, I found out that this domain would collect all the links sent by users to eachother in private chats(Google Drive Links, Unlisted Youtube Video Links, etc) and after shortening those links, the shortened link would look somewhat like `http://shortened-example.com/<SOME-RANDOM-TOKEN>`.
I then found out that this domain would **Index those shortened URLS**. This means that these shortened links would get crawled by search engines. Example of these "search engines" - Google, Wayback Machine, etc.
When I noticed this, I quickly thought of using a tool called [waybackurls](https://github.com/tomnomnom/waybackurls) by [@TomNomNom](https://twitter.com/TomNomNom) to find these Shortened Indexed URLs.
When I wrote the command `echo "shortener-example.com" | waybackurls"` , I saw thousands of shortened links in my terminal.

As I visited them one by one, they opened into full actual URLs that they originally were. I found a lot of interesting URLs like links to Private documents such as documents hosted on Google Drive, Unlisted Youtube Videos, Pastebin links, Unlisted Github Gists, etc. 
I remember that I found a Google Drive link which was hosting a PDF file that was full of PII information (Phone Numbers, Email Addresses, First and Last names, etc). 

Let me explain a bit more about this bug - So basically, whenever a user sends a link to another user(This link can be any link at all. It can be a link to Documents hosted on the internet and it can also be links to just regular websites.) the domain `shortener-example.com` gets that link and shortens it. After shortening this link, the domain indexes those urls allowing web crawlers to find those links(this is unintentional). This means that an attacker can use a webcrawling tool like `waybackurls` that was mentioned earlier to retrieve those shortened links and basically the contents of the original link that you had sent in private chat. Now take a minute to think about this situation. You send a link to your teacher(It could be a private Google Drive Link to suppose your medical report) through `www.example.com` and your teacher receives it, end of the matter. Well, the only thing it is an end of is your **privacy** because by sharing this private, unlisted link to your medical report with just your teacher, you have made it public on the Internet. What happens behind the scenes in this case is that the domain `shortener-example.com` shortens your sent google drive link and indexes it on the domain. Now, an attacker who is aware of this vulnerability would use a web crawler on `shortener-example.com` to find that link(but shortened). Once found, he could open it in his browser and your medical report that you had hosted on Google Drive and shared with nobody other than your teacher would be shown to the attacker. 


**Impact**

- This bug describes a clear privacy violation to the users because the links they sent to each other in private chats would basically become public.
- No authentication was required to retrieve a user's personal documents. All an attacker needed was a web crawler!(Wayback)
- To retrieve the user's personal documents/unlisted youtube videos, no user interaction of any sorts was required and the user would not even know about this.

Armed with these points above and clearly thinking about the major privacy violation impact, I sent this report through HackerOne to the program as a Critical.

Fast forward one month, the issue was fixed and the team rewarded me with a nice 4 digit bounty!


![_config.yml]({{ site.baseurl }}/images/dab.gif)

**Timeline**

- Reported the bug on *Mar 29th (about 1 month ago)*
- Status was changed to `Needs More Info` because the triager had some doubts. This happened on *Mar 30th (about 1 month ago)*
- I gave a detailed reply on *Mar 30th (about 1 month ago)*
- Report was marked as `Triaged` on *Apr 2nd (29 days ago)*
- Report was paid out with a 4 digit bounty :) on *Apr 29th (21 hrs ago)*
- Report was marked as `Resolved` on *Apr 29th (21 hrs ago)*

**Resources, Credits and Shoutouts**

- Special thanks to [@nagli](https://twitter.com/naglinagli) for giving me the idea about this report by disclosing something similar on Hackerone!
- Read [this](https://hackerone.com/reports/1034257)disclosed report by him on Hackerone.
- Join my Discord Server by clicking [here](https://discord.com/invite/VPtSS8gfZ4)!
- 
Thanks for Reading this, hope you enjoyed my blog :)

Many more to come xD

Until next time!


-savxiety
