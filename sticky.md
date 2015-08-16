# Sticky (MOTD) Commands

**Table of Contents** 

- [Create Sticky (Admin)](#create-sticky-admin)

The sticky message is basically a MOTD (Message of the day) which shows up at the top of the Website UI.

#### Create Sticky (Admin)

To set the sticky message you must be an admin and send this message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"motdMsg",
            "params":{
                "channel":"CHANNEL",
                "name":"USERNAME",
                "nameColor":"hexcode for color",
                "text":"Text of sticky message",
                "time":"2015-08-16T08:36:45.529Z"
            }
        }
    ]
}
```

The chat server will send the following message to the channel, this message will be sent to each user on login.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"motdMsg",
            "params":{
                "channel":"CHANNEL",
                "name":"USERNAME",
                "nameColor":"hexcode for color",
                "text":"Text of sticky message",
                "time":"Timestamp in Epoch",
                "image":"path to channel owner image"
            }
        }
    ]
}
```

To remove a sticky message just send the same motdMsg command with an empty text field. Users will also recieve another motdMsg with empty text.
