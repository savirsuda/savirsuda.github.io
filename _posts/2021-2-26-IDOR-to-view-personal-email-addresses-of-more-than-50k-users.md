---
layout: post
title: IDOR which allowed me to view Personal Email Addresses of More than 50K Users! 
---


This is My First Blog and I am really excited about it at the time of writing this. I have been trying to take out time to write this finding up for a while now, anyways finally here we are. Without wasting any more of your time, lets get into the bug :)

Lets call the target website on which this vulnerability was found as **example.com** for the Privacy of the company. The Vulnerability exists in the Forgot-Password Page of **example.com**. Basically, while requesting a password change for a user,  a **GET** parameter was sent. I was able to manipulate this parameter and was able to view the personal Email addresses of probably all of the registered users on the website. 


![_config.yml]({{ site.baseurl }}/images/idor-img.PNG)
