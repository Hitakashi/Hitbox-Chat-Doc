# Miscellaneous Commands

**Table of Contents**

- [Stream Resource Update](#stream-resource-update)
- [Host Mode Update](#host-mode-update)
- [Viewer Count Server](#viewer-count-server)

#### Stream Resource Update

This message is sent whenever the stream title or game is updated.

```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"serverMsg",
            "params":{
                "channel":"CHANNEL",
                "text":{
                    "media":{
                        "category_id":"933",
                        "category_name":"Blacklight Retribution",
                        "category_name_short":null,
                        "category_seo_key":"blacklight-retribution",
                        "category_viewers":"0",
                        "category_media_count":"1",
                        "category_channels":null,
                        "category_logo_small":null,
                        "category_logo_large":"/static/img/games/2578159-box_blr.png",
                        "category_updated":"2015-08-15 02:10:58",
                        "media_status":"This is a stream title!",
                        "media_category_id":"933"
                    }
                },
                "type":"resourceUpdate",
                "time":1439714353
            }
        }
    ]
}
```

#### Host Mode Update

This message is sent whenever a host update has occured.
<a name="hostmodeupdate" class="anchor"/><span class="octicon octicon-link"></span>
```javascript
5:::{
    "name":"message",
    "args":[
        {
            "method":"serverMsg",
            "params":{
                "channel":"hitakashi",
                "text":{
                    "media":{
                        
                    }
                },
                "type":"hostModeUpdate",
                "mode":"on",
                "time":1439714454
            }
        }
    ]
}
```

The media object contains an exact copy of the [Media API](https://github.com/Hitakashi/Hitbox-API/blob/master/media/live.md#get-medialivechannel) for the channel you're hosting.

When you turn host mode off, It sends the same message with the media object as your own channel and `mode` set to `off`

#### Viewer Count Server

This is a seperate server from from the chat server which returns infoMsg with viewer counts.

To get started you need to get one server from the [Player Server API](http://www.hitbox.tv/api/player/server). Then get `wsUrl`, `wsChannel`, `wsToken`, `userName` and `uuid` from plugins->pingback object* on the [Live Player API](https://github.com/Hitakashi/Hitbox-API/blob/master/player/player.md#get-playerconfiglivemedia_id).

[*] It looks like the only required field in the joinChannel message is the channel. So you can either fill the whole message out or just send one with the channel filled out.

Once you have those values you can initiate a WebSocket connection to `wsUrl` and send the following message:

You DO NOT add 'name' or 'args' values like all the other messages.

```javascript
{
  "method":"joinChannel",
  "params":{
    "channel":"",
    "name":"",
    "token":"",
    "country":"",
    "uuid":"",
    "isSubscriber":"false",
    "isFollower":"false",
    "embed":"false",
    "referrer":""
  }
}
```

`channel` = `wsChannel` 

`name` = `userName`

`token` = `wsToken`

`uuid` = `uuid`

You will then recieve the following message every time the viewer counts are changed.

```javascript
{
  "method":"infoMsg",
  "params":{
    "channel":"hitakashi",
    "viewers":1,
    "subscribers":0,
    "followers":1,
    "registered":1,
    "embed":0,
    "online":false
  }
}
```
