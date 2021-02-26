---
layout: post
title: IDOR which allowed me to view Personal Email Addresses of More than 50K Users! 
---


This is My First Blog and I am really excited about it at the time of writing this. I have been trying to take out time to write this finding up for a while now but haven't been able to do so because of other important things, anyways finally here we are. Without wasting any more of your time, lets get into the bug :)

**Summary**


Lets call the target website as **example.com** and the domain on which this vulnerability was found as **subdomain.example.com** for the Privacy of the company. The Vulnerability exists in the Forgot-Password Page of **subdomain.example.com**. Basically, while requesting a password change for a user,  a **GET** parameter was sent. I was able to manipulate this parameter and was able to view the Personal Email addresses of probably all of the registered users on the website. This was a pretty simple bug and a typical case of an [IDOR](https://portswigger.net/web-security/access-control/idor) .

**Description**


One day, I was checking the results of my Recon which was done using my custom bash script and I while going through the subdomains it had found, I noticed an interesting subdomain. Lets call the subdomain as `subdomain.example.com`. I started crawling the subdomain to find all of its [URLS](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL) using an awesome tool made by [@TomNomNom](https://twitter.com/tomnomnom) called `waybackurls`. I am not going to explain how this tool works or what it is, you can find the Link to it's public Github Repository at the end of this Blog post. Basically I found an Interesting URL with `waybackurls` which is:
`https://subdomain.example.com/accounts/Directory/XYZ/Form.aspx`. The extension of this page was `.aspx` which made me Directory Brute force for other `.aspx` files. Unfortunately I did not find anything and eventually gave up on Directory brute forcing. Now I thought of using Google Dorks on this subdomain. I used the following Google Dork:
`site:subdomain.example.com inurl:id`. W


![_config.yml]({{ site.baseurl }}/images/idor-img.PNG)
