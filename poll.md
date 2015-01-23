# Poll Commands

**Table of Contents** 

- [Start Poll (Admin)](#start-poll-admin)
- [Poll Info (Guest)](#poll-info-guest)
- [Poll Vote (Anon)](#poll-vote-anon)
- [Pause/Restart Poll (Admin)](#pauserestart-poll-admin)
- [End Poll (Admin)](#end-poll-admin)

A poll can only be created by a chat admin. It's possible to create a poll and giveaway but the website UI will only show the latest.

#### Start Poll (Admin)

To start a poll you must be an admin and send the following command.

```json
{
  "method":"createPoll",
  "params":{
    "channel":"CHANNEL",
    "question":"Question to ask",
    "choices":[
      {
        "text":"Answer 1"
      },
      {
        "text":"Answer 2"
      },
      ... Up to ten choices
    ],
    "subscriberOnly":true/false,
    "followerOnly":true/false,
    "start_time":"2014­12­10T14:53:33.142Z",
    "nameColor":"000000"
  }
}
```
You can decide if it's open to only followers or subscribers by toggling the flags.

This will start the poll, add it to the website UI and you will recieve the following message in the channel:

#### Poll Info (Guest)

```json
{
  "method":"pollMsg",
  "params":{
    "channel":"CHANNEL",
    "question":"Question",
    "choices":[
      {
        "text":"Answer 1",
        "votes":12
      },
      {
        "text":"Answer 2",
        "votes":40
      }
    ],
    "start_time":"2014­12­10T14:53:33.142Z",
    "status":"started",
     "voters":[
       "List of",
       "voters"
     ],
    "nameColor":"000000",
    "subscriberOnly":false,
    "followerOnly":false,
    "votes":52
  }
}
```

#### Poll Vote (Anon)

```json
{
  "method":"voteMsg",
  "params":{
    "name":"USERNAME",
    "channel":"CHANNEL",
    "choice":"Vote Choice Integer. Starts at 0 for choice 1",
    "token":"USERS_AUTH_TOKEN"
  }
}
```

After you send a voteMsg, the server will broadcast a [pollMsg](#Start-Poll-Admin) to all anon and above users.

#### Pause/Restart Poll (Admin)

To pause a poll you must be an admin and send the following message.

```json
{
  "method":"pausePoll",
  "params":{
    "channel":"CHANNEL",
    "token":"USERS_AUTH_TOKEN"
  }
}
```

The server will then broadcast a [pollMsg](#start-poll-admin) with the status set to `paused`. This is the last time to get poll results unless the poll is restarted. To restart the poll send the following message.

```json
{
  "method":"startPoll",
  "params":{
    "channel":"CHANNEL",
    "token":"USERS_AUTH_TOKEN"
  }
}
```

#### End Poll (Admin)

To end a poll you need to be an admin and send the following message.

```json
{
  "method":"endPoll",
  "params":{
    "channel":"CHANNEL",
    "token":"USERS_AUTH_TOKEN"
  }
}
```

The server will then send the following message:

```json
{
  "method":"pollMsg",
  "params":{
    "channel":"CHANNEL",
    "status":"ended"
  }
}
```
