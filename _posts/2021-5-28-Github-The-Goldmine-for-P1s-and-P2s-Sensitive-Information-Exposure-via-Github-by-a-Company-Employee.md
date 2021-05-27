---
layout: post
title: Github, The Goldmine for P1s and P2s - Sensitive Information Exposure via Github by a Company Employee
---

Github Leaks - We have seen a lot of people in the bug bounty community tweeting about their Github Findings and getting paid out four figs for their reports. That's awesome and Kudos to them for that but, if we take a step back and take a look, there aren't many blogs on Github Findings. Don't get me wrong, there are a few blogs and they are great too, but unfortunately not enough. We need more. In this write-up, I will tell you **EVERYTHING** that led me to finding a Github Leak that was triaged and paid out as a High/P2 Severity bug. Keep reading. 

To find leaks on Github like Credentials, Keys, Secrets, you have to put in the time. It can take hours to find valid credentials. This means that you will need patience more than anything else to find those high-paying juicy creds.


** How I started my Github Recon**

Like any other day, wanting to work on my target, grinding myself to find a bug, I opened my PC and checked my Google Keep. It said "Github Day", well this meant I had to start with the Github Recon on my target. I thought this was gonna be a very boring day but little did I know that I would end up with a really awesome bug. Anyways, I went over to www.github.com and started searching with basic Github Dorks like:


- `"target.com" subdomains`
- `"Target" language:python`
- `"target.com" -help.target.com`
- `"target.com" language:xml`
- `"target.com" password`
- `"target.com" secret`
- `"target.com" token`
- `"target.com" api_key`

And a few more, you get the idea. 



![_config.yml]({{ site.baseurl }}/images/leak.PNG)





![_config.yml]({{ site.baseurl }}/images/dab.gif)

**Resources, Credits and Shoutouts**

- [Youtube Video](https://www.youtube.com/watch?v=l0YsEk_59fQ) uploaded by [@BugCrowd](https://twitter.com/Bugcrowd) and made by [Th3G3nt3lman](https://twitter.com/Th3G3nt3lman) is a great resource for beginners to learn Github Recon and find leaks. 
- Join my Discord Server by clicking [here](https://discord.com/invite/VPtSS8gfZ4).
- Not exactly a resource but I wanted to share with the community something else that I have been doing on the side, I am trying to start my own Online Print-On Demand Business using [Teespring](https://teespring.com/). 

[Here](https://savswag.creator-spring.com/) is the link to the store. If you like any of my designs there, buying them would support me alot(Use code `SUMMER21` for a 10% DISCOUNT. Valid till 1st June.) and also, if you have any feedback on the designs, prices, or anything at all, please send me a Direct Message on my [Twitter](https://twitter.com/savxiety). It would mean the world to me :)

Thanks for Reading this, hope you enjoyed my blog :)

Many more to come xD

Until next time!


-savxiety
