# Server Side Events (SSE)

What it is: A one-way stream from server → client over HTTP.

How it works:
- Client opens a connection
- Server pushes updates when they happen
- Client cannot send messages back on that channel

Pros
- Simpler than WebSockets
- Automatic reconnection
- Great for streaming updates

Cons
- One direction only
- Not ideal for highly interactive apps

Example use
- Live scores
- Stock prices
- Activity feeds

`🧠 Think: radio broadcast — you listen, you don’t talk back.`