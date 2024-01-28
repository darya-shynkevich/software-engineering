
![](../../../_Attachments/Pasted%20image%2020240128142009.png)

1. WebSocket manager - stores mapping between servers, ports, and users
2. Message service - stores user's messages
3. To minimise the latency and reduce the number of calls to the WebSocket manager, each WebSocket server caches the following information:
	- If both users are connected to the same server, the call to the WebSocket manager is avoided.
	- It caches information of recent conversations about which user is connected to which WebSocket server.
4. Usually, the WebSocket servers are lightweight and don’t support heavy logic such as handling the sending and receiving of media files. We have another service called the **asset service**, which is responsible for sending and receiving media files.
5. The content of the file is loaded onto a **CDN** if the asset service receives a large number of requests for some particular content.

## How it works

Now, let’s assume that user A wants to send a message to user B. As shown in the above figure, both users are connected to different WebSocket servers. The system performs the following steps to send messages from user A to user B:

1. User A communicates with the corresponding WebSocket server to which it is connected.
2. The WebSocket server associated with user A identifies the WebSocket to which user B is connected via the WebSocket manager. If user B is online, the WebSocket manager responds back to user A’s WebSocket server that user B is connected with its WebSocket server.
3. Simultaneously, the WebSocket server sends the message to the message service and is stored in the Mnesia database where it gets processed in the first-in-first-out order. **As soon as these messages are delivered to the receiver, they are deleted from the database.**
4. Now, user A’s WebSocket server has the information that user B is connected with its own WebSocket server. The communication between user A and user B gets started via their WebSocket servers.
5. If user B is offline, messages are kept in the Mnesia database. Whenever they become online, all the messages intended for user B are delivered via push notification. Otherwise, these messages are deleted permanently after 30 days.

## Support for group messages

WebSocket servers don’t keep track of groups because they only track active users. However, some users could be online and others could be offline in a group. For group messages, the following three main components are responsible for delivering messages to each user in a group:
- Group message handler
- Group message service
- Kafka

Let’s assume that user A wants to send a message to a group with some unique ID—for example, `Group/A`. The following steps explain the flow of a message sent to a group:

1. Since user A is connected to a WebSocket server, it sends a message to the message service intended for `Group/A`.
2. The message service sends the message to Kafka with other specific information about the group. The message is saved there for further processing. In Kafka terminology, a group can be a topic, and the senders and receivers can be producers and consumers, respectively.
3. Now, here comes the responsibility of the group service. The **group service** keeps all information about users in each group in the system. It has all the information about each group, including user IDs, group ID, status, group icon, number of users, and so on. This service resides on top of the MySQL database cluster, with multiple secondary replicas distributed geographically. A Redis cache server also exists to cache data from the MySQL servers. Both geographically distributed replicas and Redis cache aid in reducing latency.
4. The group message handler communicates with the group service to retrieve data of `Group/A` users.
5. In the last step, the group message handler follows the same process as a WebSocket server and delivers the message to each user.

# References:

1. [Amazon System Design Interview Question | Design Chat System](https://www.youtube.com/watch?v=FqpEKEfForY&list=PLOAph0xkZvSuqy8yq_0D6NEABhmSTRYrN&index=9) (video)
2. [Whatsapp System design or software architecture](https://www.youtube.com/watch?v=L7LtmfFYjc4&list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV&index=9) (video)
3. [System Design: Chat Application](https://medium.com/@m.romaniiuk/system-design-chat-application-1d6fbf21b372)
4. [WhatsApp System Design](https://jinlow.medium.com/whatsapp-system-design-3750ef4d8b80)
5. [WhatsApp System Design](https://medium.com/@karan99/system-design-whatsapp-791c6c201161)
6. [Facebook Messenger/Whatsapp System Design](https://www.youtube.com/watch?v=RjQjbJ2UJDg) (video)
7. [WhatsApp System Design](https://levelup.gitconnected.com/whatsapp-system-design-14f0fbc1c1f5)
8. [WhatsApp System Design](https://jinlow.medium.com/whatsapp-system-design-3750ef4d8b80)
9. [Telegram System Architecture](https://interviewnoodle.com/telegram-system-architecture-ddf9f7d358de)
10. [Real-time Messaging - Slack Engineering](https://slack.engineering/real-time-messaging/)
11. [Slack System Design](https://faun.pub/slack-system-design-ce91167a8cc7)
12. [Architecture of Chat Service (3 parts) at Riot Games](https://engineering.riotgames.com/news/chat-service-architecture-persistence)
13. [Facebook System Design | Instagram System Design | System Design Interview Question](https://www.youtube.com/watch?v=9-hjBGxuiEs) (video)
14. [Whatsapp System Design: Chat Messaging Systems for Interviews](https://www.youtube.com/watch?v=vvhC64hQZMk&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=9) (video)
15. [Facebook Messenger Optimisations](https://spectrum.ieee.org/how-facebooks-software-engineers-prepare-messenger-for-new-years-eve)
16. [Facebook: An Example Canonical Architecture For Scaling Billions Of Messages](http://highscalability.com/blog/2011/5/17/facebook-an-example-canonical-architecture-for-scaling-billi.html)
17. [Erlang at Facebook](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf)
18. [Facebook Chat](https://www.facebook.com/note.php?note_id=14218138919&id=9445547199&index=0)
19. [Wechat Architecture That Powers 1.67 Billion Monthly Users](https://newsletter.systemdesign.one/p/chat-application-architecture?utm_source=substack&publication_id=1511845&post_id=137935794&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z)