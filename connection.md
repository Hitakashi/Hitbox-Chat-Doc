# How to connect to hitbox Websocket.

#### Server Connection

To connect to hitbox chat you should first initiate a HTTP connection to http://api.hitbox.tv/chat/servers?redis=true the result should be the following Json Object 

```json
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
