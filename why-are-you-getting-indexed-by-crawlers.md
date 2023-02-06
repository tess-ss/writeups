## Summary

The topic at hand is the indexing of website endpoints in Web Archive/URLscan/Alien Vault. Who is to blame? It could be the customers who share the URLs with their network via social media or other platforms. However, why is a company held responsible, and what steps can be taken to mitigate this risk?

## Explanation
Why this writeup or research on this topic, or why is it Important?

I have been struggling for a while explaining my point in Bug Bounties reports for Impact to both triagers and Program Owners regarding this topic. So I thought why not write a detialed explanation and my understanding on this subject so its easier and a universal solution for all Bug Hunters in the community.

So lets consider the example target here which is https://example.com 

First of all let me be very clear, each and every website on the Intranet is spidered/ crawled by these scanners/ crawlers regardless of the fact that users are sharing the endpoints or they aren't. So lets consider https://subdomain.example.com is storing sensitive content of its users such as Invoices or any file regardless of the file extension.

So lets consider the following as https://subdomain.example.com/1122.pdf/?access_token=jwt which is a Billing Invoice for the user to download the PDF of his/ her billing. What are the chances of this being spidered/ crawled? I would say from my personal opinion 80-90% there is a chance this endpoint getting cached/ spidered by Any Indexing site and also they might save the PDF as well. So let's why is this happening when this was suppose to be a secret between the client and the company.

First of all lots of companies makes a mistake here which is they configure `/robots.txt` file on the main domain which is `https://example.com/robots.txt` and they expect it to honor on the subdomains which is not going to be happen here, sorry :) 

Each and every subdomain serving sensitive content should have a very strict `/robots.txt` configured on each of its subdomain if they want to avoid Indexing of they're users sensitive content.

For example: The main domain here is https://example.com so example.com should have a robots.txt file configured on each subdomains like 

```
https://tess.example.com/robots.txt
https://subdomain.example.com/robots.txt 
```

Please do not rely on the main domain `/robots.txt` file cause scanners/ crawlers DO NOT HONOR main domain file while indexing the endpoints and contents of subdomains.

You must having a question, now like okay tess you are focusing so much on `/robots.txt` file so why you don't tell us what contents to be served in the following file to avoid such indexing? Right?

Okay so here is it is, Please configure it like 

```bash
# https://www.robotstxt.org/robotstxt.html
User-agent: *
Disallow: /
```

Keep updating the file as you get reports from bug hunters that a new crawler is not indexing your site, so please update it accordingly.

Now, I have also noticed even after having a strict `/robots.txt` file configured the Indexing does not stops and it keeps happening and if you are already Indexed and now your sensitive data is leaking by it, what should you do in such scenarios? I know you might get frustrated by this point, so what should you do in such scenarios?

Steps to Delete your Site from the Internet Archive / Wayback Machine / Archive.org / Alein Vault and others as well.

Please read these 5 easy and proven steps to remove your site from the Internet Archive / Wayback Machine / Archive.org.

I have provided extensive detail if you scroll down, but the key 5 steps to deleting your site from Archive.org are below:

* Update your website robots.txt file to block the Internet Archive / Wayback Machine / Archive.org Crawler / Check your Copyright Notice
* Draft a DMCA Takedown Notice with specific links to sites / pages you want to be removed from the Internet Archive / Wayback Machine / Archive.org
* Find an old invoice demonstrating the oldest date of ownership you have for the domain.
* Draft and send a polite email with 2. and 3. attached to the Internet Archive / Wayback Machine / Archive.org Crawler
* Wait 3-5 days

I have provided details below with more information to complete each easy step to remove your website from Archive.org and links if you need help. Honestly, my results have always been mixed and it’s one of my frustrations with the Internet Archive. A site update has sometimes resulted in my robots.txt file getting nuked and I find out I am in Archive.org again. I wish Archive.org would give publishers a way of verifying your domain to do a takedown or a webmaster tool like that found on Google/Bing.

**Step 1: Robots.txt to Block a site from the Internet Archive / Wayback Machine / Archive.org / Check Copyright Notice**

If you’re super interested, you can learn more about robots.txt here.

Archive.org has a mixed attitude to robots.txt but they do honor them.

Make sure you add this to the end of your existing robots.txt file, don’t delete any existing entries.

```
User-agent: ia_archiver
Disallow: /
```

If you’re not sure how to edit your robots.txt then talk to your hosting provider or website developer.

If you use WordPress, this free Archive.org Blocker [WordPress](https://wordpress.org/plugins/block-archive-org-robots-txt/) plugin does everything you need to block Archive.org from WordPress.  Install, activate, and you’re done. If you already use a robots.txt plugin, you can add the above code to the end of your existing robots.txt file.

While you’re making these changes, it’s also a good time to check that there is a current Copyright Notice on your website. Most content management systems put this on your site automatically.


**Step 2: DMCA Takedown Notice for the Internet Archive / Wayback Machine / Archive.org**

The DMCA is short for the Digital Millennium Copyright Act. It’s a piece of US legislation to help copyright holders protect their intellectual property. Even if you don’t live in the US you can use a DMCA notice to have content remove from the Internet Archive / Wayback Machine / Archive.org.

I am #NotALawyer so if you’re dealing with a serious issue with archived content, get your own legal counsel. This is also not legal advice, so if this step makes you nervous best to ask an expert. I have been told by others who have read these instructions that you can skip this DMCA step and still have success. Your mileage may vary.

To generate a DMCA takedown notice, I used this free [DMCA Generator tool](https://www.whoishostingthis.com/resources/dmca/#what-is-the-DMCA) from Who Is Hosting This. Otherwise, use this [DMCA Takedown Notice generator from the Intellectual Property HQ.](https://iphqs.com/dmca-takedown-notice-generator/)

I am going to stress again, DMCA notices are legal documents so make sure you’re fully aware of what you’re doing.

The DMCA form is straightforward, but make sure you paste in as many website addresses from Archive.org that match the dates you owned the domain and the content you want to be removed.

**Step 3: Demonstrating a History of Domain Ownership to the Internet Archive / Wayback Machine / Archive.org** 

If you’re requesting the removal of a whole domain or website from Archive.org may ask for proof of domain ownership. Archive.org provides no automated verification of ownership such as a DNS record change, website code, or uploading of a file. You will need to find an old invoice / receipt from your domain host proving ownership.

Most hosting providers do provide access to a history of invoices, so you will need to log in to your account to get these. Worst case, it might require an email to the accounts department of your hosting company.

If you’re in a hurry, you can try and skip this step and see how Archive.org responds but be prepared for them to ask for this information. One way to try and avoid the issue is to send the request from an email address associated with the domain.

That said, I strongly encourage you to send proof of ownership as part of the request. Archive.org can be frustrating if your domain has switched hosts, registrars, etc. during the request period which they verify against public [domain records](https://whoisrequest.com/history/). If you forget your original register or host, I’d do a [free domain history](https://whoisrequest.com/history/) check to jog your memory.

If you do not own the domain you will not be able to get the site deleted from the Internet Archive.

**Step 4: Email Requesting the Internet Archive / Wayback Machine / Archive.org remove your website** 

The email address for Archive.org takedown requests is info@archive.org but do not email them unless you have done Steps 1-3.

It is better if your email comes from the domain you’re emailing about. For example, if you want Google.com removed, you should have an @google.com email. In my experience, Archive.org will respond to a request from an email address other than the domain you are requesting but they may require additional verification steps.

Sending a request from a free email service like Gmail, Outlook.com, etc. is almost guaranteed to slow things down. This is one of the reasons I recommend Step 3, because it provides additional information when you make the request.

Here’s some suggested wording for an Archive.org Takedown Request / Removal of Domain where:

* [Your_Name] should be replaced with your name and
* [Your_Domain] with your relevant domain name.
* [Start_Date] that has the date from which you want the domain removed and can prove ownership of the domain.

I recommend sending a separate notice for each domain, don’t try and do it all at once.

Subject

```
Formal Request To Remove [Your_Domain] From Internet Archive Wayback MachineCopy
```

Body

```
Hello

I am [Your_Name] owner of [Your_Domain]. 

I’m officially requesting the immediate removal of [Your_Domain] site/domain from web.archive.org and the Internet Archive Wayback Machine.
The User-agent: ia_archiver Disallow: / code in our robots.txt file is not being followed. The Copyright Notice on this site can be found here [Your_Domain]

I am requesting removal of [Your_Domain] from [Start_Date] up to and including today and all days going forward.

Attached is formal DMCA notice as well as evidence that I am the owner of [Your_Domain].

Thank you for your prompt attention.

[Your_Name]
```

**Don’t Forget!** to attach the DMCA notice you generated in Step 2 and proof of ownership in Step 3.


**Step 5: Wait and Track Archive.org**

Once you send your email you will need to wait. I’ve had response times as fast as 24 hours and some that take a few days. Archive.org will reply, just remember that they are US-based (California) so make sure you allow for US Pacific Time, weekends, and major US holidays. Be patient, polite, but firm. If you don’t hear after 3 days, a polite follow-up email may be warranted.

In my experience, if you do everything above, you will get a response within 5 days. It takes about a week after they respond for content to be purged from Archive.org

**Other Tips**

* The Internet Archive / Wayback Machine / Archive.org will only delete pages and sites from when you took ownership, not just because you now have ownership. This is really important. So if you have bought an old domain, you’re out of luck for anything older than the day you commenced ownership.
* I have found the people from Internet Archive / Wayback Machine / Archive.org friendly. So please be polite. They do want to help and anything they ask is to clarify an issue. They do only respond during US business hours. So it pays to be patient (3 working days minimum).
* There is no urgent process. There’s not much else I can say other than if you need this done fast, I share your frustration. If there’s a legal reason why you need something removed quickly, you should really get legal counsel involved.
* I have no experience having content removed from Archive.org where you are not the domain owner, for example, if a domain has breached your copyright and now your content is in the archive. I am #NotALawyer and recommend you get legal advice if your issue relates to your content but not your domain.
* This last piece of advice is as much for me as it is for everyone else. If you want to always block the Internet Archive / Wayback Machine / Archive.org make sure you keep your robots.txt up to date. It’s a lot easier to keep robots.txt updated and block Archive.org than get pages removed.
* There is value to the Internet Archive, so don’t remove your site unless you really feel it’s necessary. It may be better to just remove specific pages.
* If you want to remove your personal information and data from data brokers, then you will need to use something like OneRep.
* I do not provide any support or assistance in removing your site or page. I also cannot remove your site.
* I am not affiliated with the Internet Archive. 

The folllowing contents were copied from [joshualowcock](https://www.joshualowcock.com/guide/how-to-delete-your-site-from-the-internet-archive-wayback-machine-archive-org/) Huge thanks for him to creating this. I copied the entire steps from him because I think nobody can better explain this the way he did. 

Once you remove the site or file a email to do so, your website will no longer be Indexed and will be listed as Not allowed to indexed or forbidden. Just like this 

<img width="902" alt="Screen Shot 2023-02-06 at 1 40 13 PM" src="https://user-images.githubusercontent.com/65326024/217057240-a94f0145-3f5f-4c2f-a843-11dd81f91422.png">


I hope it was easy to understand, And I look forward to all companies to Implement this and not blame any users or any facts for Indexing and take it as a serious security issue.

Thanks you for reading and I hope you have blessed rest of your day/ night.
tess

Follow me on [Twitter](https://twitter.com/ArmanSameer95), If you want to reach me out with further questions.
