# Short Polling

What it is: The client keeps asking the server over and over

We are sending requests to the server for regular intervals. We may can get the data or we may can not get updated data. There are no holding the requests. It means requests are short lived.

“Anything new?”
“Anything new?”
“Anything new?”

How it works:
- Client sends a request every X seconds
- Server responds immediately (even if there’s nothing new)

Pros
- Dead simple
- Works everywhere

Cons
- Wasteful (lots of useless requests)
- Not very real-time

Example use
- Old-school dashboards
- Simple apps where delays don’t matter

`🧠 Think: refreshing your inbox manually