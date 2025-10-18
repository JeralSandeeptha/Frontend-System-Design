# Logging and Monitoring

## What Is Frontend Logging and Monitoring?

`Logging` = Recording events, errors, and behaviors that occur in the frontend (browser or app)
`Monitoring` = Observing those logs + performance + usage over time through a dashboard or alert system

In the frontend, logging and monitoring help you:
- Detect JavaScript errors and crashes in real time
- Track slow page loads or API response times
- Monitor user actions that lead to bugs
- Understand how real users experience your app (RUM – Real User Monitoring)

## Logging Architecture

```bash
Frontend (React/Vue/Angular)
   ↓ Logs via SDK (Sentry, LogRocket, etc.)
Monitoring Backend (SaaS or self-hosted ELK)
   ↓
Dashboards, Alerts, Reports
```

- Optional: Use your own API gateway or Node.js middleware to preprocess logs

## What You Can Log in the Frontend

| Category              | Example Data                                | Why It’s Useful                           |
| --------------------- | ------------------------------------------- | ----------------------------------------- |
| **Errors**            | Uncaught JS errors, React component crashes | Detect bugs early                         |
| **Network**           | API request/response times, failures        | Diagnose backend issues from the frontend |
| **Performance**       | Page load time, render time, memory usage   | Optimize UX and speed                     |
| **User interactions** | Button clicks, form submissions             | Understand behavior and funnels           |
| **Custom events**     | Feature toggles, A/B test exposure          | Business or feature analysis              |
| **Session info**      | Browser type, OS, IP (careful with privacy) | Debug environment-specific bugs           |

- Performance Metrics
    - Web Vitals
    - API response time
    - Feature/Sceenario time
    - Resource timing
    - Paint Times
    - Frame rate
    - Network

- Resource Errors
    - Client Exceptions
    - Network Errors
    - API Failuire
    - 5XX, 4XX Status Codes

- User Intersctions
    - Clicks
    - Scrolls
    - Form Submissions
    - Browser Events

- Resource Utilizations
    - Resource Usages (CPU, Memory)

- Custom Events

## Tools and Services
- You typically use logging libraries + monitoring platforms
    - `Sentry` – error tracking & performance monitoring (most popular)
    - `LogRocket` – records user sessions (you can replay what users did before a crash)
    - `Datadog RUM` – real user monitoring + logging
    - `New Relic Browser` – performance and error analytics
    - `Elastic APM (with ELK stack)` – open source option
    - `Firebase Crashlytics` – for React Native or mobile apps
    - `Microsoft Clarity`
    - `Google Analytics`

## Implement Logging in the Frontend

### Sentry
```bash
npm install @sentry/react @sentry/tracing
```

```js
// index.js
import * as Sentry from "@sentry/react";
import { BrowserTracing } from "@sentry/tracing";

Sentry.init({
  dsn: "https://<YOUR_DSN>.ingest.sentry.io/<PROJECT_ID>",
  integrations: [new ()],
  tracesSampleRate: 1BrowserTracing.0, // 0.1 for production
});
```

This automatically logs:
- Uncaught exceptions
- Unhandled promise rejections
- Performance metrics
- User sessions (if enabled)

You can also manually capture errors:
```js
try {
  riskyFunction();
} catch (error) {
  Sentry.captureException(error);
}
```

### Browser APIs

Use Browser APIs
- Performance API (window.performance) gives metrics like load time, render time, etc.
- Navigation Timing and Resource Timing APIs help collect granular data

```js
window.addEventListener("load", () => {
  const timing = performance.timing;
  const pageLoadTime = timing.loadEventEnd - timing.navigationStart;
  console.log("Page load time:", pageLoadTime, "ms");
});
```

- You can send these metrics to a monitoring backend or analytics service (like Google Analytics 4 or a custom API)

### DIY Custom Logging

You can create your own minimal logger:

```js
function logFrontendError(error, info) {
  fetch("/api/logs", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      message: error.toString(),
      stack: error.stack,
      info,
      userAgent: navigator.userAgent,
      timestamp: new Date().toISOString(),
    }),
  });
}

window.addEventListener("error", (e) => logFrontendError(e.error, "window error"));
window.addEventListener("unhandledrejection", (e) => logFrontendError(e.reason, "unhandled promise"));
```

- Then your backend can store these logs in a database (e.g., MongoDB) and visualize them via Kibana or Grafana.

### Grafana Pharo Dashboards

## Best Practices

- ✅ Don’t log sensitive info (passwords, tokens, credit card data)
- ✅ Use environment-based logging (less verbose in production)
- ✅ Batch logs to avoid slowing down the app
- ✅ Integrate alerts — notify team on critical errors (via Slack, email, etc)
- ✅ Correlate logs with backend using a request ID or session ID

## Summary

| Aspect                      | Tool/Approach                 |
| --------------------------- | ----------------------------- |
| **Error tracking**          | Sentry, LogRocket             |
| **Session replay**          | LogRocket, FullStory          |
| **Performance monitoring**  | Datadog RUM, New Relic        |
| **Analytics**               | GA4, PostHog                  |
| **Custom logs**             | REST API + ELK stack          |
| **Filtering (by IP, user)** | Handled in backend dashboards |