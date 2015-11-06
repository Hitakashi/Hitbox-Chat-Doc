# Moderation Commands

**Table of Contents** 

- [Timeout a user (User)](#timeout-a-user-user)
- [Ban a user (User)](#ban-a-user-user)
- [IP-Ban a user (Admin)](#ip-ban-a-user-admin)
- [Un-Ban a user (User)](#un-ban-a-user-user)
- [Add a moderator (Broadcaster)](#add-a-moderator-broadcaster)
- [Remove a moderator (Broadcaster)](#remove-a-moderator-broadcaster)
- [Slow Mode/Subscriber Only Mode (User)](#slow-modesubscriber-only-mode-user)

The moderator/admin commands can of course only be used by moderators and/or admins.

#### Timeout a user (User)

To kick a user (ban it temporarily for a specified amount of time) you must be at least a moderator and send the following message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"kickUser",
            "params":{
                "channel":"CHANNEL",
                "name":"Name of user to timeout",
                "token":"Token of moderator",
                "timeout":"10"
            }
        }
    ]
}
```

The timeout is in seconds. If everything is correct the chat server sends a infoMsg with `action` set to `kickUser`

#### Ban a user (User)

To ban a user so that he/she is unable to write in a chat you must send the following message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"banUser",
            "params":{
                "channel":"CHANNEL",
                "name":"Name of user to ban"
            }
        }
    ]
}
```

The ban will be announced in the chat via an infoMsg with `action` set to `ban`, it will also send a [userList](./user.md#user-list) and ban list:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"banList",
            "params":{
                "channel":"CHANNEL",
                "data":[
                    "test-account",
                    "test-account-2"
                ]
            }
        }
    ]
}
```

This is a list with all the banned user in the chat. When there are banned users you will also get this list when you call the user list.

#### IP-Ban a user (Admin)

To IP-Ban a user you must be an admin in the channel. You send the same command for banning but add a flag:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"banUser",
            "params":{
                "channel":"CHANNEL",
                "name":"Name of user to ban",
                "token":"Token of admin",
                "banIP":true
            }
        }
    ]
}
```

Be careful with IP-Bans, You don't know how many users you can block with it. (Universities often use one public IP)

#### Un-Ban a user (User)

To un-ban a user you must be moderator and send the following message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"unbanUser",
            "params":{
                "channel":"CHANNEL",
                "name":"Name of user to un-ban",
                "token":"Token of moderator"
            }
        }
    ]
}
```

#### Add a moderator (Broadcaster)

To make a user a moderator you must be the broadcaster (No, Editors don't count)* and send the following message to the server:

* Currently editors can create temporary mods, but the user loses the mod on a refresh.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"makeMod",
            "params":{
                "channel":"CHANNEL",
                "name":"Name of user to mod",
                "token":"Token of broadcaster"
            }
        }
    ]
}
```

The result will be announced in a infoMsg with `action` set as `isAdmin`.

#### Remove a moderator (Broadcaster)

To remove a moderator you must be the broadcaster (No, Editors don't count) and send the following message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"removeMod",
            "params":{
                "channel":"CHANNEL",
                "name":"Name of user to unmod",
                "token":"Token of broadcaster"
            }
        }
    ]
}
```

The result will be announced in a infoMsg with `action` set as `isAdmin`.

#### Slow Mode/Subscriber Only Mode (User)

To set a slow mode in chat (So that every normal user can only post one message every X seconds or only subscribed users can chat) you must be at least moderator and send the following message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"slowMode",
            "params":{
                "channel":"CHANNEL",
                "time":10,
                OR
                "subscriber":true,
                "rate":0
            }
        }
    ]
}
```
Only send rate when your setting sub only mode. It should also only be 0 for now.

You can (currently) only have slow mode or subscriber mode active at the same time. You can send the subscriber flag as false, but not true while time is greater than 0.

To disable slow mode send time as 0. To disable subscriber mode toggle the flag.

The result will be announced in a slowMsg (Identical to infoMsg) with `action` set as `isAdmin`. If you're setting slow mode it will add a `slowTime` variable with its value as the slow time.
