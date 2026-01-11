# Long Polling

What it is: Client asks once, server waits until something happens.

We are sending requests to the server and we hold the response unless we get the data. It means requests are long lived. Connection is long lived until you get the new data if we don't get data for a long time then we should have a macahnism to timeout the requets. 

How it works:
Client sends request
Server holds it open
When data is ready → server responds
Client immediately sends another request

Pros
- Feels near real-time
- Less wasted traffic than short polling

Cons
- Still kinda hacky
- Server has to manage lots of open requests

Example use
- Early chat apps
- Notification systems before WebSockets existed

`🧠 Think: calling someone and staying on hold until they have news`