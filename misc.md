# Miscellaneous Commands

**Table of Contents**

- [Stream Resource Update](#stream-resource-update)
- [Viewer Count Server](#viewer-count-server)

#### Stream Resource Update

This message is sent whenever the stream title or game is updated.

```json
{
  "name":"message",
  "args":[
    {
      "method":"serverMsg",
      "params":{
        "channel":"CHANNEL",
        "text":{
          "media":{
            "category_id":"32401",
            "category_name":"Mario Kart 8",
            "category_name_short":null,
            "category_seo_key":"mario-kart-8",
            "category_viewers":"16",
            "category_media_count":"1",
            "category_channels":null,
            "category_logo_small":null,
            "category_logo_large":"/static/img/games/2600974-12510938343_01c49da2be_o.jpg",
            "category_updated":"2015-01-12 05:03:38",
            "media_status":"Stream Title",
            "media_category_id":"32401"
          }
        },
        "type":"resourceUpdate",
        "time":1421043231
      }
    }
  ]
}
```

This message is sent whenever a host update has occured.
<a name="hostmodeupdate" class="anchor"><span class="octicon octicon-link"></span>
```json
{
  "name":"message",
  "args":[
    {
      "method":"serverMsg",
      "params":{
        "channel":"hitakashi",
        "text":{
          "media":null
        },
        "type":"hostModeUpdate",
        "mode":"on",
        "time":1422955717
      }
    }
  ]
}
```

#### Viewer Count Server

This is a seperate server from from the chat server which returns infoMsg with viewer counts.

To get started you need to initiate a conection to the [Player Config API](https://github.com/Hitakashi/Hitbox-API/blob/master/media/player_config.md#get-playerconfigmedia_typeuser_id) and grab the values `wsUrl`, `wsChannel`, `wsToken`, `userName` and `uuid` from plugins->pingback object*.

[*] It looks like the only required field in the joinChannel message is the channel. So you can either fill the whole message out or just send one with the channel filled out.

Once you have those values you can initiate a WebSocket connection to `wsUrl` and send the following message:

```json
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

```json
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
