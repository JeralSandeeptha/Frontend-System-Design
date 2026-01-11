# Webhooks

What it is: Server → server communication triggered by events.

Important: This is not for browsers talking to servers in real time.

How it works:
- You give a service a URL
- When something happens, it POSTs data to that URL

Pros
- Event-driven
- No polling
- Very efficient

Cons
- Requires public endpoint
- Needs security & retries

Example use
- Stripe payment notifications
- GitHub push events
- CI/CD pipelines

`🧠 Think: “Call me when this happens.”`