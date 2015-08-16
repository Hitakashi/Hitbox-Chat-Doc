# General Commands

**Table of Contents** 

- [Login Command](#login-command)
- [Logout Command](#logout-command)
- [Chat Messages](#chat-messages)
- [Direct Messages](#direct-messages)
- [System Messages](#system-messages)
- [User List](#user-list)
- [User Info](#user-info)
- [Media Log](#media-log)


#### Login Command

To login to the hitbox chat system you need to send the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"joinChannel",
            "params":{
                "channel":"CHANNEL",
                "name":"USERNAME or don't send this",
                "token":"Users Auth Token or don't send this"
            }
        }
    ]
}
```

When you don't have a token (You can get a token via the API [here](https://github.com/Hitakashi/Hitbox-API/blob/master/auth/login.md#post-authtoken)) you can login as a guest but cannot send messages or get the user list. 

You should always send a lower case channel name.

The client must wait until it gets the login confirmation which looks like this:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"loginMsg",
            "params":{
                "channel":"CHANNEL",
                "name":"USERNAME or UnknownSoldier",
                "role":"guest, anon, user or admin"
            }
        }
    ]
}
```

The `name` value is the name in the chat. You can customized the casing however you want. When it is "UnknownSoldier" you are logged in as a guest. When you don't get a response in 10 seconds your credentials are either wrong or the chat server is not responding, so disconnect the chat server and try another one or check if your credentials are ok. If you supplied a name and token but your `role` is still "guest", your credentials are incorrect, so you should disconnect the server and check your credentials. You will never get a error message from the chat server.

The roles can be found in the (README)[./README.md]

It is important to know that one Socket.IO/Websocket connection can only connect to one channel. You must open a new Socket.io/Websocket connection for every channel you want to connect to.

After a successful login the chat server sends you a backlog of the latest 6 chat messages in the last X(Confirmation soon) minutes and other info like stick messages, polls or giveaways are running.


#### Logout Command

To logout you can either close the Socket.IO/Websocket connection or send:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"partChannel",
            "params":{
                "name":"USERNAME or UnknownSoldier"
            }
        }
    ]
}
```

There will be no response but the connection will be closed from the server.


#### Chat Messages

To send a message to the chat use this format. `text` is limited to about 255-300 characters:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"chatMsg",
            "params":{
                "channel":"CHANNEL",
                "name":"USERNAME",
                "nameColor":"Hexcode for color, or don't send it.",
                "text":"Text of your message"
            }
        }
    ]
}
```
If your message was successfully sent to the channel the chat server sends it back to you like this (And every other chat message)

If your message was rejected by the server due to slow mode or subscriber only, you will get back an infoMsg.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"chatMsg",
            "params":{
                "channel":"CHANNEL",
                "name":"USERNAME",
                "nameColor":"Hexcode for color, or don't send it.",
                "text":"Text of your message",
                "time":EpochTimestamp,
                "role":"role in chat",
                "isFollower":true/false,
                "isSubscriber":true/false,
                "isOwner":true/false,
                "isStaff":true/false,
                "isCommunity":true/false,
                "media":true/false,
                "image":"Path to channel owner/subscriber image",
                "buffer":true,
                "buffersent":true
            }
        }
    ]
}
```

`buffer` and `buffersent` are only added when it's a backlog message. The owner will not have isSubscriber but still has access to subscriber emotes.


### Direct Messages

To send a direct message to another hitbox user you need to send the following message:

```javascript
5:::{
   "name":"message",
   "args":[
      {
         "method":"directMsg",
         "params":{
            "channel":"CHANNEL",
            "from":"USERNAME",
            "to":"RECIPENT",
            "nameColor":"HEXCOLOR",
            "text":"This is a direct message."
         }
      }
   ]
}
```

You will then get back a confirmation that the message was sent. You can also get a infoMsg back that you are sending too fast.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"infoMsg",
            "params":{
                "text":"Direct message to RECIPENT sent.",
                "channel":"CHANNEL",
                "timestamp":1426785188,
                "action":"directmsg"
            }
        }
    ]
}
```

The recipient will then receive the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"directMsg",
            "params":{
                "from":"SENDER",
                "nameColor":"HEXCOLOR",
                "text":"This is a direct message.",
                "time":1426785188,
                "isStaff":false,
                "isCommunity":false,
                "channel":"CHANNEL SENT FROM",
                "type":"info"
            }
        }
    ]
}
```

#### System Messages

The chat server sends system messages, for example adding or removing a moderator, that have the following format:

The name field is only sent on ban.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"infoMsg",
            "params":{
                "text":"Text of your message",
                "channel":"CHANNEL",
                "timestamp":EpochTimeStamp,
                "name":"USER ACTED UPON",
                "action":"Optional, Example ban, kickUser, isAdmin"
            }
        }
    ]
}
```

There is also a special infoMsg for new subscribers that pops up in the website UI (like a poll) 

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"infoMsg",
            "params":{
                "text":"Test-Account just subscribed to this channel",
                "channel":"CHANNEL",
                "subscriber":"Test-Account",
                "type":"subChannel",
                "time":EpochTimeStamp
            }
        }
    ]
}
```

There is also a special chatLog for broadcasters and editors that is sent on multiple actions. (Subscriptions, Followers, Bans, etc) Buffer is only sent on history chatLogs.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"chatLog",
            "params":{
                "text":"<user>Hitakashi</user> banned <user>Claptrap</user>",
                "timestamp":EpochTimeStamp,
                "channel":"CHANNEL",
                "buffer":true
            }
        }
    ]
}
```

#### User List

To get the user list of a channel you must be logged in with an authToken and then send this message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"getChannelUserList",
            "params":{
                "channel":"CHANNEL"
            }
        }
    ]
}
```

You will then get the following list from the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"userList",
            "params":{
                "channel":"CHANNEL",
                "data":{
                    "Guests":null,
                    "admin":["Broadcaster","Staff-Member","Community-Member","Editor"],
                    "user":["Moderator"],
                    "anon":["test-account"],
                    "isFollower":["Moderator","test-account","Staff-Member"],
                    "isSubscriber":["Staff-Member","test-account"],
                    "isStaff":["Staff-Member"],
                    "isCommunity":["Community-Member"]
                }
            }
        }
    ]
}
```

The server will automatically send you this on join.

This is the user list for this channel, divided in the different roles and extra flags. Staff members are always listed in `admin`, `isStaff` and `isSubscriber`. Ambassadors are listed in `admin` and `isCommunity`. `Guests` contain an integer of the number of unregistered viewers.

#### User Info

To get info for one user send this command:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"getChannelUser",
            "params":{
                "channel":"CHANNEL",
                "name":"Username to query"
            }
        }
    ]
}
```

As a response you should get something like this:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"userInfo",
            "params":{
                "channel":"CHANNEL",
                "name":"Username",
                "timestamp":EpochTimeStamp,
                "role":"Role of user",
                "isFollower":true/false,
                "isSubscriber":true/false,
                "isOwner":true/false,
                "isStaff":true/false,
                "isCommunity":true/false
            }
        }
    ]
}
```

If you query a banned user,  `role` is set as `banned`, the is flags are removed and there's an added "banned":true value 

#### Media Log

To get list of media (Currently only images) send this command:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "name":"message",
            "args":[
                {
                    "method":"mediaLog",
                    "params":{
                        "channel":"test-account",
                        "type":"image",
                        "name":"test-account",
                        "token":"SuperSecret"
                    }
                }
            ]
        }
    ]
}
```

As a response you should get something like this:

```javascript
{
    "name":"message",
    "args":[
        {
            "method":"mediaLog",
            "params":{
                "channel":"CHANNEL",
                "type":"image",
                "data":[
                    {
                        "channel":"CHANNEL",
                        "name":"USERNAME",
                        "nameColor":"HEXCOLOR",
                        "time":EPOCHTIMESTAMP,
                        "url":"https://www.google.com/logos/doodles/2015/special-olympics-world-games-2015-5710263202349056-hp.gif",
                        "size":{
                            "x":600,
                            "y":151
                        },
                        "proxyUrl":"http://edge.sf.ak.hitbox.tv/chatimage?u=https%3A%2F%2Fwww.google.com%2Flogos%2Fdoodles%2F2015%2Fspecial-olympics-world-games-2015-5710263202349056-hp.gif"
                    }
                ]
            }
        }
    ]
}
```
