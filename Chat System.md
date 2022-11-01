## Chat System

### Requirements Clarification
1. one to one chatting
2. user status
3. media resources sharing
4. group chatting
5. push notifications to offline user


### Scale of Data Size
Assuming we have 500 million daily active users and each user sends 40 messages daily on average and each message is 100 bytes on average. To store all the messages for one day, we need:

```20 billion messages * 100bytes = 2TB```

Other than the chat message, we still need to store users' information.


**Bandwidth Estimation**: 25MB/s of incoming data for each second

```2TB / 86400 sec ~= 25MB/s```


**Chat Server Quantity**: 
Assuming a modern server can handle 50k concurrent connections at any time, for 500 million users, we need 10k such servers. (facebook has 30k servers now)


### API Interface Design

```sendMessage(User sender, User receiver, Message message)```

```setStatus(User user, UserStatus prevStatus, UserStatus status)```

### Database Model Design

```
User {
  String userId,
  UserStatus status,
  TimeStamp lastOnlineTime,
}
```
```
Message {
  String messageId,
  String content,
  User sender,
  User receiver,
  MessageStatus status,
}
```

### Diagram

1. One to One Chat:

UserA ——send message——> gateway(a network node connect two networks with different protocols) ————> Session Service(guide you to the server for a specific user) ————> Database ————> gateway —— websocket——> UserB


> router: can communicate between two different networks using the same protocol.

> websocket:
> allowing duplex(bi-directional messages)
> need handshake (based on TCP)
> can build a persistant connection


2. Message Status

A database to maintain messages






