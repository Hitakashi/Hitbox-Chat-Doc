# How to connect to hitbox Chat.

**Table of Contents**

- [Pre-Server Connection (Step 1)](#pre-server-connection-step-1)
- [Websocket ID (Step 2)](#websocket-id-step-2)
- [Server Connection (Step 3)](#server-connection-step-3)
- [Limits](#limits)
- [Post-Server Connection](#post-server-connection)

#### Pre-Server Connection (Step 1)

To start connecting to hitbox chat you should first initiate a HTTP GET request to http://api.hitbox.tv/chat/servers?redis=true the result should be the following Json Object 

```javascript
[
  {
    "server_ip":"ec2-54-91-252-98.compute-1.amazonaws.com"
  },
  {
    "server_ip":"ec2-54-87-101-9.compute-1.amazonaws.com"
  },
  {
    "server_ip":"ec2-54-221-0-139.compute-1.amazonaws.com"
  },
  {
    "server_ip":"ec2-54-211-95-56.compute-1.amazonaws.com"
  },
  {
    "server_ip":"ec2-54-87-229-245.compute-1.amazonaws.com"
  }
]
```

You can pick whatever server from the response, but you should never save the address. The servers are ever changing and getting it via the API is the safest method.

At this point you should also get your [auth token](https://github.com/Hitakashi/Hitbox-API/blob/master/auth/login.md#post-authtoken) for the user unless you are planning to login as a guest. 

#### Websocket ID (Step 2)

Once you get the server address from above, you should append `/socket.io/1/` to the address. After that you need to send a HTTP GET request to that address, for example http://ec2-54-87-229-245.compute-1.amazonaws.com/socket.io/1/ which will return the following text.

```text
7_wKSaZndijMFrJYCziZ:60:60:websocket
```

You then need to get everything before the first colon : which will be your connection ID. Ex: `7_wKSaZndijMFrJYCziZ`

#### Server Connection (Step 3)

Now you can get on to actually connecting with all the previous information. You should initiate a WebSocket connection to the finalized WS URL `ws://ec2-54-87-229-245.compute-1.amazonaws.com/socket.io/1/websocket/7_wKSaZndijMFrJYCziZ`

If you cannot connect after 8 seconds you should grab another server from Step 1 and as a precaution get a new Connection ID from Step 2.

#### Limits

There are limits on how many connections can be made from one IP/one user within a time frame. The same applies to chat messages and all other functions. More information will be provided in the loginMsg doc.

#### Post-Server Connection

Once you are connected to the server you should recieve a `1::`, after that you need to send a joinChannel message along with whatever else you want. You will also recieve a `2::` every so often and should respond with a `2::` 
