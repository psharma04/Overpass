# Technical

## Foreword

I am not a Computer Science or Software Engineering graduate. Almost all of this is either my opinion, or based on my experience doing code review and development for self-hosted community projects.

I have intentionally taken a stance known as "zero trust" throughout this page. Zero trust ideology is an increasingly common practice in cybersecurity, where developers design with the assumption that every other part of the codebase is compromised other than the current stage, and users assume that the developer is malicious. For example, a user should assume that a password input box is saving your passwords and sending them to a malicious database, and the web developer should assume that the server has been hacked somehow and is sending all password data to some random.

You may disagree with this model, and that's fine. This is entirely my opinion.

## Introduction
Overpass is my fork of Lars Kaesberg's EmailVerify bot, with minor instance-specific tweaks to be better suited for UNSW usage. All major changes are upstreamed where they align with the original bot's purpose, because contributing to open source projects is good for the world of software development.

I have stripped out unnecessary functionality from the fork, as this represents a potential attack vector which is unnecessary for the use-case. I've also added an S/MIME integration with Actalis certificates to verify that the code is not being MITM'd.

 If you need a general purpose version of the bot, or want to host your own, I'd recommend just using Lars' version.

I am usually running slightly behind the current version of the upstream code, in order to ensure that compatibility with any changes I make. However, I am aiming to be mostly interoperable with Lars' version. You can see the version I'm using on [GitHub](https://github.com/psharma04/Overpass/).

## The issue with Drawbridge

The real problem with Drawbridge is what happens after you click the "Submit" button:

 ![]()

If you're not familiar with HTTP requests, this might not mean much to you. The problem is that my zID and zPass are being transmitted as plaintext from my device to the CSE server, which then (I'm assuming) checks it against UNSW's Active Directory instance (meaning it's likely making a second plaintext request over the internet with my password, since UNSW's Active Directory is hosted on Microsoft's servers, and Drawbridge is hosted on on-campus servers).

Yes, the site is using HTTPS, but that honestly doesn't mean much. Do you remember seeing this prompt the first time you connected to uni wifi?
![image]()

![222]()

The MacOS version explains it a little better, but essentially what that certificate means is that UNSW has the ability to make fake TLS certificates for any website you access while on university-owned networks (this is known as a "Certificate Authority"). There have been several high-profile credential attacks which use this same technique on public networks (think shopping center free wifi) to steal people's login credentials.

Even if you don't care about your UNSW account, are you sure that no other accounts use the same password? Because they're all vulnerable too. Also consider the fact that access to your UNSW account means access to your full name, home address, phone number, and (if you're using HECS/SA-HELP) your tax file number. 2FA somewhat mitigates this risk, but Microsoft's proprietary 2FA has been previously susceptible to this exact same attack vector.

The best way of dealing with passwords is to not deal with them at all. The second best way is to encrypt them on-device before making the HTTP request (usually through hashing), ensuring that your server (and any snoopy middle-men) don't have any chance of seeing your plaintext password. This still has vulnerabilities, such as rainbow table and replay attacks, but it makes it significantly harder to do so.

Here's a scenario:

> Your ex (another UNSW student) is bitter about your breakup and wants to get back at you. They MITM your password from your attempt to link your zID to a new society server using Drawbridge, link their own burner discord account to your student ID, log into campus wifi with your details, and then spams racial slurs in Discord servers until your zID gets reported to Conduct and Integrity.

Far-fetched? Yes, but not impossible, and actually quite feasible (we tested everything except the racial slurs on a consenting student's account successfully).

## What about client-side hashing?

Hashing is the industry standard technique for making the "real" contents of something unreadable but uniquely identifiable. Think of a hash as a serial number for your password. It uniquely identifies that password, but doesn't tell you what it actually is.

The point of client-side hashing is to eliminate the "malicious server" attack vector, where the server is collecting your form submissions and storing the passwords. Hashing means that the server's owner can't see what your actual password is.

While it does protect users from the threat of their passwords being leaked and then used to log into other platforms, client-side hashing doesn't resolve the crazy ex scenario mentioned before.

Additionally, I don't think there's a reliable method of using client-side hashing with an Active Directory backend, and unless the bot devs are able to convince Microsoft to change the way that their platform works, the method that Drawbridge seems to use (LDAP authentication) is incompatible with client-side hashing.

For those who are curious, I believe that the correct way to do password authentication (if you insist on doing it) is double hashing: once on the client side, and then a second time (with a salt) on the server side. Also MFA, please.

## What other authentication methods could they have used?

You've probably already seen the best one: Single Sign-On. 

You know that popup that sends you to Microsoft's login page when you open Moodle/myUNSW? By letting Microsoft handle the entire authentication process (using OIDC), you're not relying on one developer to get password management right.

Microsoft has much more to lose than Arc does if they have a security hole that gives away your password. Additionally, they have legal requirements which force them to disclose a password database leak if you get caught up in one. Arc, UNSW, and CSE have no such requirement.
