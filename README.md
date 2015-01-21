# Hitbox-Chat-Doc

Most headers will contain the lowest role that can access or see the command.

| Role | Description |
| ---- | ---- |
| guest | You are a unregistered user. You cannot write in chat and any messages you send will be dropped. User list is also disallowed. |
| anon | You are a normal viewer. You write and see the user list. **Strict** messages/second limits apply. Duplicate messages are disallowed. |
| user | You are a moderator. You can kick/ban other users, set slow and sub mode but cannot IP ban a user. |
| admin | You have full permission in chat. You can add/remove moderators and IP ban a user. |

| chatMsg Role | Description |
| ---- | ---- |
| isFollower | If this user is following this channel. |
| isSubscriber | If this user is subscribed to this channel. |
| isOwner | If this is the channel owner, If so you can get the image of the owner in the "image" key. This is how you can differentiate between Editors and other admins. |
| isStaff | If this user is a member of the hitbox staff and has admin rights |
| isCommunity | If this user is a hitbox Ambassador and has admin rights |
