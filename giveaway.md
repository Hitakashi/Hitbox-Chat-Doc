# Raffle Commands

**Table of Contents** 

- [Create a giveaway (Admin)](#create-a-giveaway-admin)
- [Giveaway Info (Guest)](#giveaway-info-guest)
- [Pause a giveaway (Admin)](#pause-a-giveaway-admin)
- [End a giveaway (Admin)](#end-a-giveaway-admin)
- [Resume a giveaway (Admin)](#resume-a-giveaway-admin)
- [Vote in a giveaway (Anon)](#vote-in-a-giveaway-anon)
- [Pick a winner (Admin)](#pick-a-winner-admin)
- [Hide a giveaway (Admin)](#hide-a-giveaway-admin)
- [Cleanup a giveaway (Admin)](#cleanup-a-giveaway-admin)

A giveaway is like a [poll](./poll.md) but you can pick a winner and have to announce a prize.

#### Create a giveaway (Admin)

To create a giveaway you must be an admin and send the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"createRaffle",
            "params":{
                "channel":"CHANNEL",
                "question":"QUESTION",
                "prize":"Description of Prize",
                "choices":[
                    {
                        "text":"Answer 1"
                    },
                    {
                        "text":"Answer 2"
                    }
                ],
                "subscriberOnly":true/false,
                "followerOnly":true/false,
                "start_time":"2015-08-16T08:11:34.581Z",
                "nameColor":"Hexcode for color"
            }
        }
    ]
}
```

You can decide if the giveaway will only be accessible by followers, subscribers or all users.

The server will then send a message like this to the channel:

#### Giveaway Info (Guest)

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"raffleMsg",
            "params":{
                "channel":"CHANNEL",
                "question":"QUESTION",
                "prize":"Description of Prize",
                "choices":[
                    {
                        "text":"Answer 1",
                        "count":0
                    },
                    {
                        "text":"Answer 2",
                        "count":0
                    }
                ],
                "start_time":"2015-08-16T08:11:34.581Z",
                "status":"started",
                "forAdmin":true/false,
                "nameColor":"Hexcode for color",
                "subscriberOnly":true/false,
                "followerOnly":true/false
            }
        }
    ]
}
```

Counts will be removed if you aren't an admin.

This message is always sent on connection until the giveaway is [cleaned up](#cleanup-a-giveaway-admin).

#### Pause a giveaway (Admin)

To pause a giveaway send the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"pauseRaffle",
            "params":{
                "channel":"CHANNEL"
            }
        }
    ]
}
```

After you send a pauseRaffle, the server will broadcast a [raffleMsg](#Giveaway-Info-Guest) to all anon and above users with the status value set to `paused`.

### End a giveaway (Admin)

To end a giveaway send the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"endRaffle",
            "params":{
                "channel":"hitakashi"
            }
        }
    ]
}
```

After you send a endRaffle, the server will broadcast a [raffleMsg](#Giveaway-Info-Guest) to all anon and above users with the status value set to `ended`. The giveaway must be in this state to roll a winner.


#### Resume a giveaway (Admin)

You can also resume a giveaway if it's still in a paused state.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"startRaffle",
            "params":{
                "channel":"CHANNEL"
            }
        }
    ]
}
```

After you send a startRaffle, the server will broadcast a [raffleMsg](#Giveaway-Info-Guest) to all anon and above users with the status value set to `started`.

#### Vote in a giveaway (Anon)

To vote in a giveaway a user must send this message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"voteRaffle",
            "params":{
                "name":"USERNAME",
                "channel":"CHANNEL",
                "choice":"Number of answer, starts at 0"
            }
        }
    ]
}
```

After you send a voteRaffle, the server will broadcast a [raffleMsg](#Giveaway-Info-Guest) to all anon and above users.

#### Pick a winner (Admin)

To pick a winner you must end the giveaway first and then send the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"winnerRaffle",
            "params":{
                "channel":"CHANNEL",
                "answer":"Number of answer, starts at 0"
            }
        }
    ]
}
```

The server will respond twice the following message, one for all users without the email (with forAdmin set to false) and one for admins (with forAdmin set to true) with the email:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"winnerRaffle",
            "params":{
                "channel":"CHANNEL",
                "winner_name":"Test-Account",
                "winner_email":"example@example.com",
                "winner_picked":true,
                "forAdmin":true,
                "answer":"Number of answer, starts at 0"
            }
        }
    ]
}
```

### Hide a giveaway (Admin)

To hide a giveaway from the UI send the following message:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"hideRaffle",
            "params":{
                "channel":"hitakashi"
            }
        }
    ]
}
```

After you send a hideRaffle, the server will broadcast a [raffleMsg](#Giveaway-Info-Guest) to all anon and above users with the `status` set to `hidden`.

#### Cleanup a giveaway (Admin)

To end a giveaway send this message to the server:

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"cleanupRaffle",
            "params":{
                "channel":"CHANNEL"
            }
        }
    ]
}
```

After you send a cleanupRaffle, the server will broadcast the following to all users.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"raffleMsg",
            "params":{
                "status":"delete"
            }
        }
    ]
}
```
