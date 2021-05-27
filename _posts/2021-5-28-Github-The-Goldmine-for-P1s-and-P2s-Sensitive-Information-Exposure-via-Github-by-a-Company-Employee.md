---
layout: post
title: Github, The Goldmine for P1s and P2s - Sensitive Information Exposure via Github by a Company Employee
---

Github Leaks - We have seen a lot of people in the bug bounty community tweeting about their Github Findings and getting paid out four figs for their reports. That's awesome and Kudos to them for that but, if we take a step back and take a look, there aren't many blogs on Github Findings. Don't get me wrong, there are a few blogs and they are great too, but unfortunately not enough. We need more. In this write-up, I will tell you **EVERYTHING** that led me to finding a Github Leak that was triaged and paid out as a High/P2 Severity bug. Keep reading. 

To find leaks on Github like Credentials, Keys, Secrets, you have to put in the time. It can take hours to find valid credentials. This means that you will need patience more than anything else to find those high-paying juicy creds.


**How I started my Github Recon**

Like any other day, wanting to work on my target, grinding myself to find a bug, I opened my PC and checked my Google Keep. It said "Github Day", well this meant I had to start with the Github Recon on my target. I thought this was gonna be a very boring day but little did I know that I would end up with a really awesome bug. Anyways, I went over to [www.github.com](https://www.github.com) and started searching for domain specific things with basic Github Dorks like:


- `"target.com" subdomains`
- `"target.com" language:python`
- `"target.com" NOT help.target.com`
- `"target.com" NOT staging language:xml`
- `"target.com" password`
- `"target.com" secret`
- `"target.com" token`
- `"target.com" api_key`

And a few more, you get the idea. I got a few results, one of them caught my eye. I saw a password in a file. The filetype was JSON and the contents looked like this:

```json

{
	"host": "cpanel.subdomain.target.com",
	"port": "2083",
	"cpanel_username": targetcpanel,
	"cpanel_password": <RANDOM-ALPHANUMERIC-PASSWORD>

}

```

I got excited and a little bit out of control because it took me a lot of time to find this. I navigated to [https://cpanel.subdomain.target.com:2083](http://localhost) and to my disappointment, the port was closed :(


I went back to the file and saw that the name of the file contained "test" in it and at the end of the file, the developer had commented out "Test Data". This led to frustration and I ended up closing my PC and watching Netflix. 


** Back on the Grind**

After some time, when I was feeling motivated again, I launched Github once again and this time I did not want to give up. I thought of removing certain keywords from the search results so that I would find only the results which are not False Positives and Test Data.

For this, I used the above Github Dork:


`target.com password NOT test NOT sandbox NOT staging NOT development NOT docker NOT help NOT language:java` 

Now, from the above Dork, as expected, I found all the good results, test data and other data that I didn't want was not filtered out and I could finally focus on the important stuff. I had a good feeling about this. One thing to note here for me was that I found a lot bash results. Files ending with `.sh`. I then  appended this into my dork:

`language:bash`

Now, I was only looking at bash code. I started searching for keywords like `secret`, `key`, `token`, etc, but I couldn't find any good results. I had the program policy open in another tab and I was reading the little information that is sometimes given about the in-scope domains. Next to the domain that I was searching for, I saw the keyword `Postgres` written next to it. I thought it would be a good addition to my search query and searched for it. I got a lot of results, I clicked on the `Sort` button, and then I clicked on `Rcently indexed`. This shows me the latest index results. On the top of the page, I saw a bash file which contained some random bash and I thought it was nothing interesting but when I took a closer look at it, I saw that I had found postgresql credentials for a website! 

Here is the screenshot:

![_config.yml]({{ site.baseurl }}/images/leak.PNG)


The credentials were in the url and they were very easy to spot. The format in which they were preset looked like this:


`postgresql://<USERNAME>:<PASSWORD>@subdomain.target.com:5439`

This time, I tried to stay calm(it's hard!) and find out if these were **actually** valid credentials or not. This is a Github Tip that I would like to share. Whenever you find something that may look like credentials, always check if these were published to Github by someone related to the Target Company or was published by some random person on the Internet. 


**How to check if the Person who published the credentials is related to the Company?**

The way you do this is, by visiting the profile of the person who has leaked the credentials and copy their Name and search for it on Linkedin. I did this and found out that this guy is a former Senior Software Engineer at the Target Company.

![_config.yml]({{ site.baseurl }}/images/linkedin-screenshot.PNG)

I tried to login using the credentials, port and the host, but I couldn't because the port was filtered or closed. Because of this, I tried to send the report in as a Medium severity bug but I guess I misclicked and it got set to `High`. The next week, to my surprise, I woke up and checked my phone, I saw that the report got triaged, It didn't impress me much because I had started getting a lot triages and I knew this bug would get triaged so I went over to my PC and opened the report to acknowledge the triage and found out that I was paid out for the bug too! Yes, as a `High/P2`!

![_config.yml]({{ site.baseurl }}/images/dab.gif)

**Resources, Credits and Shoutouts**

- [Youtube Video](https://www.youtube.com/watch?v=l0YsEk_59fQ) uploaded by [@BugCrowd](https://twitter.com/Bugcrowd) and made by [Th3G3nt3lman](https://twitter.com/Th3G3nt3lman) is a great resource for beginners to learn Github Recon and find leaks. 
- Join my Discord Server by clicking [here](https://discord.com/invite/VPtSS8gfZ4).
- Subscribe to [@securibee's](https://twitter.com/securibee) newsletter, [@Intigriti's](https://twitter.com/intigriti) bug bytes to stay up-to-date with the resources.
- Not exactly a resource but I wanted to share with the community something else that I have been doing on the side, I am trying to start my own Online Print-On Demand Business using [Teespring](https://teespring.com/). 

[Here](https://savswag.creator-spring.com/) is the link to the store. If you like any of my designs there, buying them would support me alot(Use code `SUMMER21` for a 10% DISCOUNT. Valid till 1st June.) and also, if you have any feedback on the designs, prices, or anything at all, please send me a Direct Message on my [Twitter](https://twitter.com/savxiety). It would mean the world to me :)

Thanks for Reading this, hope you enjoyed my blog :)

Many more to come xD

Until next time!


-savxiety
