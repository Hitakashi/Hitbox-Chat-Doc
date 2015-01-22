# General Commands


#### Login Command

To login to the hitbox chat system you need to send the following message:

```json
{
  "method":"joinChannel",
  "params":{
    "channel":"CHANNEL",
    "name":"USERNAME or don't send this",
    "token":"Users Auth Token or don't send this"
  }
}
```

When you don't have a token (You can get a token via the API [here](https://github.com/Hitakashi/Hitbox-API/blob/master/auth/login.md#post-authtoken)) you can login as a guest but cannot send messages or get the user list. 

You should always send a lower case channel name.

The client must wait until it gets the login confirmation which looks like this:

```json
{
  "method":"loginMsg",
  "params":{
    "channel":"CHANNEL",
    "name":"USERNAME or UnknownSoldier",
    "role":"guest, anon, user or admin"
  }
}
```

The `name` value is the name in the chat. You can customized the casing however you want. When it is "UnknownSoldier" you are logged in as a guest. When you don't get a response in 10 seconds your credentials are either wrong or the chat server is not responding, so disconnect the chat server and try another one or check if your credientials are ok. You will never get a error message from the chat server.

The roles can be found in the (README)[./README.md]

It is important to know that one Socket.IO/Websocket connection can only connect to one channel. You must open a new Socket.io/Websocket connection for every channel you want to connect to.

After a successful login the chat server sends you a backlog of the latest 6 chat messages in the last X(Confirmation soon) minutes and other info like stick messages, polls or giveaways are running.


#### Logout Command

To logout you can either close the Socket.IO/Websocket connection or send:

```json
{
  "method":"partChannel",
  "params":{
    "name":"USERNAME or UnknownSoldier"
  }
}
```

There will be no response but the connection will be closed from the server.


#### Chat Messages

To send a message to the chat use this format. `text` is limited to about 255-300 characters:

```json
{
  "method":"chatMsg",
  "params":{
    "channel":"CHANNEL",
    "name":"USERNAME",
    "nameColor":"Hexcode for color, or don't send it.",
    "text":"Text of your message"
  }
}
```
If your message was successfully sent to the channel the chat server sends it back to you like this (And every other chat message)

If your message was rejected by the server due to slow mode or subscriber only, you will get back an infoMsg.

```json
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
```

`buffer` and `buffersent` are only added when it's a backlog message.

#### System Messages

The chat server sends sytem messages, for example adding or removing a moderator, that have the following format:

```json
{
  "method":"infoMsg",
  "params":{
    "text":"Text of your message",
    "channel":"CHANNEL",
    "timestamp":EpochTimeStamp,
    "action":"Optional, Example ban, kickUser, isAdmin"
  }
}
```


#### User List 

To get the user list of a channel you must be logged in with an authToken and then send this message:

```json
{
  "method":"getChannelUserList",
  "params":{
    "channel":"CHANNEL"
  }
}
```

You will then get the following list from the server:

```json
{
  "method":"userList",
  "params":{
    "channel":"CHANNEL",
    "data":{
      "admin":["Broadcaster","Staff-Member","Community-Member"],
      "user":["Moderator"],
      "anon":["test-account"],
      "isFollower":["Moderator","test-account","Staff-Member"],
      "isSubscriber":["Staff-Member","test-account"],
      "isStaff":["Staff-Member"],
      "isCommunity":["Community-Member"]
    }
  }
}
```

This is the user list for this channel, divided in the different roles and extra flags. Staff members are always listed in `admin`, `isStaff` and `isSubscriber`. Ambassadors are listed in `admin` and `isCommunity`

#### User Info

To get info for one user send this command:

```json
{
  "method":"getChannelUser",
  "params":{
    "channel":"CHANNEL",
    "name":"Username to query"
  }
}
```

As a response you should get something like this:

```json
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
```
