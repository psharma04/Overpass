# Overpass for UNSW

> A simple Discord verification bot that doesn't involve trusting strangers with your password.

## What is this?

Overpass is a discord bot designed to verify that a student is who they say they are. It relies on some simple principles:

1. A student's zID is unique,
2. The person who owns a specific zID also has access to the email `zID@ad.unsw.edu.au`
3. No-one else has access to that zID's email inbox
4. You don't (and shouldn't) trust strangers with your password.

## How does it work?

1. A server admin (usually a society exec or course convenor) sets up the bot in their server
2. Someone joins the server
3. The bot assigns them an "Unverified" role, stopping them from accessing anything but the `#verify` channel
4. They click the button in the channel and receive a DM from the bot
5. They give the bot their UNSW email (either zID@ad.unsw.edu.au)
6. The bot sends them an email with a 6 digit code
7. They reply to the bot with the code from their email
8. The bot assigns them a "Verified" role in the discord server, and posts their zID in a channel that's only accessible to the server admins.

## What makes this different from Drawbridge?

[Drawbridge](https://web.cse.unsw.edu.au/~apps/discord/) is the Arc-approved version of this bot, developed by Tom (a former CSESoc exec/CSE Course Convenor) and maintained by Dylan from CSE. However, I consider Drawbridge to have a major design flaw:

> "Your zPass will not be stored. I don't want your zPass, I like my own one."

Unfortunately, Drawbridge is currently closed-source, meaning no-one can actually verify that your password isn't being stored by the operators. Additionally, your password is sent in plaintext to the server. I've written a more technical explanation of Why That's Bad (ironically, based on stuff I learned in a CSE course) further down the page for those who are more technically inclined.

Even if you don't trust me, the worst case is that I know your zID and what UNSW discord servers you've joined. Unlike Drawbridge, Overpass never has a risk of giving me, server admins, the bot or any webpage your password, making it (at least to me) significantly safer to use. However, it still successfully performs the task of logging a student's zID for the admins of a discord server, which is the entire purpose of both bots.

Additionally, a student wanting to remove their data is as simple as asking the server admins to run the `/delete_user_data` command for their Discord username.

## I'm a server admin. How do I set it up?

Unfortunately, the setup is a little bit involved. If you're having trouble, feel free to send me a DM on Discord.

1. Create 2 new channels on your server - I'd recommend calling them `#verify` and `#registration-logs` or similar.
2. Create 2 new roles - I'd call them `Verified` and `Unverified`.
3. Set the permissions for the roles as follows:
   1. `Unverified` should have all permissions disabled for every channel except `#verify`
   2. `Verified` should have all permissions disabled for the channel `#verify`
   3. `#registration-logs` should be set up to only be visible to moderators. You can use the "Private Channels" function for this.
4. [Invite the bot](https://discord.com/oauth2/authorize?client_id=1031151331190263848&permissions=268504128&scope=bot%20applications.commands) to your server.
5. Run all of the following commands:

| Command (I would copy-paste this)                            | Expected response from the bot                        | Notes                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------- | ------------------------------------------------------------ |
| `/unverifiedrole @Unverified`                                | "Unverified role changed to unverified"               | Substitute `@Unverified` for your Unverified user role       |
| `/verifiedrole @Verified`                                    | "Verified role changed to verified"                   | Substitute `@Verified` for your Verified user role           |
| `/add_unverified_on_join True`                               | "Enabled auto add unverified role!"                   |                                                              |
| [For UNSW staff-run servers or any other case where Arc wouldn't need access]`/domains unsw.edu.au,ad.unsw.edu.au` | "Added @unsw.edu.au,@ad.unsw.edu.au"                  | MAKE SURE THIS IS EXACTLY THE SAME AND THAT BOTH DOMAINS ARE INCLUDED IN THE BOT'S REPLY. **DO NOT ADD** `student.unsw.edu.au` |
| [For clubs/servers where Arc staff need access]`/domains unsw.edu.au,ad.unsw.edu.au,arc.unsw.edu.au` | "Added @unsw.edu.au,@arc.unsw.edu.au,@ad.unsw.edu.au" | MAKE SURE THIS IS EXACTLY THE SAME AND THAT ALL 3 DOMAINS ARE INCLUDED. **DO NOT ADD** `student.unsw.edu.au` |
| `/set_log_channel #registration-logs`                        | "Modified log channel"                                | Substitute `#registration-logs` for your zID log channel. MAKE SURE THIS IS ONLY ACCESSIBLE TO MODERATORS. |
| `/verifymessage This Discord server is operated by <SOCIETY>. By registering, you agree to comply with the server's rules, as well as the UNSW Code of Conduct. Please enter your UNSW email address (usually z1234567@ad.unsw.edu.au). You will get a 6 digit code emailed to your UNSW email within the next few minutes, please send that code as a DM reply to this bot.` | "Modified verify message"                             | Add any legal text you need to the message, and replace `<SOCIETY>` with whatever group is responsible for the server. |

6. Copy-paste your server rules into `#verify`.
7. Make sure only moderators have the "Send Messages" Permission for `#verify`.
8. Run the `/button` command with the parameters below:

| Channel   | Button Text          | Message                                                      |
| --------- | -------------------- | ------------------------------------------------------------ |
| `#verify` | Click here to verify | This bot will verify your zID. Go to https://overpass.unsw.bot for more information on how it works. |

​		It should respond "Button created", and you should see the following message in the `#verify` channel:

![CleanShot 2024-05-29 at 22.26.06@2x](https://raw.githubusercontent.com/psharma04/Overpass/main/images/example-button.png)

9. Run `/status` to confirm that you've set the bot up properly. If there are any unexpected differences from the following screenshot, you might have dome something wrong.

![](https://raw.githubusercontent.com/psharma04/Overpass/main/images/expected-config.png)

10. [OPTIONAL] Run `/verify` and verify yourself to test that it works with your account before sharing the server with students/members.
11. Done! You should be good to share the server with new members and have them verify when they join.

## Common tasks

### I need to remove a student's data from my server

All you need to do is find their discord username in `#verification-logs` and delete the corresponding message from the bot!

### I need to ban a zID from my server

Run `/blacklist zID@unsw.edu.au, zID@ad.unsw.edu.au`. 

### I want to add/remove a domain

Simply run `/domains domain.tld` for each new domain, or `/removedomains domain.tld` for each domain you want to remove.

I'd recommend running `/domains` after either of those commands to make sure the new list is correct for your server.

### I want to remove the bot entirely

Run `/delete_server_data`. The bot will nuke itself and disconnect from your server (you'll have to do all the setup again if you want to re-add it).

### I need help with some other task

If `/help` doesn't give you the information you need, DM me on Discord.