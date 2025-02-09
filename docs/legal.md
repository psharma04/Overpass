# Legal

The Terms of Service and Privacy Policy for the Overpass Discord bot.

## Terms of Service

### Usage Agreement

You accept the following Terms of Service and Privacy Policy by inviting the Bot to your server.

The privilege of using and inviting this bot can be revoked for any server or user if the Terms of Service or Privacy
Policy of this Bot or the [Terms of Service](https://discord.com/terms), [Privacy Policy](https://discord.com/privacy)
or [Community Guidlines](https://discord.com/guidelines) of Discord gets violated.

The bot is allowed to collect data as described in [Privacy Policy](#privacy-policy).

### Intended Age

Users under the minimal age of Discord's [Terms of Service](https://discord.com/terms) are not allowed to use this bot.

### Affiliation

This Bot is not affiliated with, supported or made by Discord.

This bot is also not affiliated or maintained by Arc, UNSW, or any societies who may be using it unless specified otherwise.

### Liability

Breaking these Terms of Service won't make the owner of the bot liable.

These terms can be updated at any time by the owner of the bot. Any user can opt out of these new terms by removing the
Bot from its own servers.

### Contact

Email: <a href="mailto:overpass@pks.ai">Send Email</a>

## Privacy Policy

### Data Usage

The bot uses the stored data to verify that no emails get used multiple times. Everything else is settings data for individual servers.

The data is only used by the bot and won't be shared with any 3rd party services.

### Stored Information

Following data gets stored from the bot:

#### Server

- `id` server id
- `domains` verified domains
- `verifiedrole` role for verified users
- `unverifiedrole` role for unverified users
- `channelid` reference to the channel with the verify message
- `messageid` reference to the verify message
- `language` language setting for the server
- `autoVerify` automatically verify when joining a server
- `autoAddUnverified` automatically add the unverified role to every new member of the server
- `verifyMessage` message to show to the user when verifying
- `logChannel` channel to log the emails of verified users
- `blacklist` list of emails that are blocked from being verified

#### User

- `userid` user id
- `email` an MD5 hashed version of the email address
- `guildid` server id

### Removal of Data

#### Server

The admin can remove the server and all the user data by using `/delete_server_data`

When the bot gets removed from the server all the data is removed automatically.

#### User

The user can remove their own data by using `/delete_user_data`

## Warranty

No warranty (whether express or implied) or support is provided by the developers of this code, except on a voluntary basis. No expectation of performance, except of the nature described in the technical documentation, should be expected.