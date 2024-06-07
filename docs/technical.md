# Technical

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
