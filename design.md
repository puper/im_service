# 消息存储
所有消息都会保存，每个用户的个人消息（包括他所发送和接受的）都会存储在一台机器上，
这样能够保证能够获取到个人的聊天历史记录，同时也导致一条消息会存储两份，
发送者和接受者各自一份。
服务器接收到消息时，消息被直接放入接受者的消息队列，用户在线时，发送消息给用户，
用户接受到消息后（服务器收到消息的ACK），消息被移出消息队列。
用户上线后，首先会获取发送所有消息队列的消息，用户接收到消息后（服务器收到
消息的ACK），消息被移出消息队列，此后正常接受在线消息。

# 消息队列
每台服务器消息id是递增的，消息发送也是顺序的，所以只需要每个用户记录最后接收到的消息id，
下一次登陆后，就可以继续取余下的离线消息。
在记录最近接收到的消息id时，加入设备id，这样就可以保证每台手机都可以接收到所有的消息。
web端因为无法获取唯一的设备id，所以web端不获取离线消息，只接受在线消息，同时web
端接收到消息也不需要返回ACK。
