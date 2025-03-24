# Frontend System Design

Frontend system design is all about planning and architecting the user interface of an application, ensuring scalability, maintainability, and performance. It involves decisions about component structure, state management, performance optimizations, and UI/UX best practices.

## Table of Contents

- [Frontend System Design](#frontend-system-design) 
- [Component Based Architecture](#component-based-architecture)
- [Authentication and Security](#authentication-and-security)
- [API Handling and Data Fetching](#api-handling-and-data-fetching)
- [UI/UX Design Principles](#ui/ux-design-principles)
- [Performance Optimization](#performance-optimization)
- [Client Side Caching](#client-side-caching)
- [State Management](#state-management)
- [Testing and Quality Assurance](#testing-and-quality-assurance)
- [Micro Frontends](#micro-frontends)
- [Deployment and CI/CD](#deployment-and-cd/cd)

## Frontend System Design

Frontend system design is all about planning and architecting the user interface of an application, ensuring scalability, maintainability, and performance. It involves decisions about component structure, state management, performance optimizations, and UI/UX best practices.

## Client Side Caching

Client-side caching refers to the process of storing web resources (HTML, CSS, JavaScript, images, API responses, etc.) on a user's browser or device to avoid unnecessary network requests and improve loading speeds.

- Faster Page Loads: Reduces the time required to fetch resources.

- Reduced Server Load: Fewer requests to the server improve scalability.

- Offline Availability: Enables Progressive Web Apps (PWAs) and Service Workers to serve content even without an internet connection.

- Reduced Bandwidth Usage: Beneficial for users with limited or expensive data plans.

1. Browser Caching (HTTP Cache)

- Browsers store copies of previously fetched resources and reuse them for subsequent requests.

Key Mechanisms:

- Cache-Control Headers: Define caching policies.

- ETags: Validate cached responses.

- Last-Modified Headers: Check if a resource has changed.

- Expires Headers: Set expiration dates for resources.

```cmd
Cache-Control: max-age=3600, must-revalidate
```

2. Service Workers

- Service Workers act as a proxy between the browser and the network, allowing developers to cache resources and API responses efficiently.

Features:
- Allows offline caching.

- Provides fine-grained control over caching.

- Enables background sync and push notifications.

```js
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("my-cache").then((cache) => {
      return cache.addAll(["/index.html", "/styles.css", "/script.js"]);
    })
  );
});
```

3. Local Storage

- Simple key-value storage mechanisms built into browsers.
- Persistent storage that remains even after the browser is closed.

```js
localStorage.setItem("username", "JohnDoe");
console.log(localStorage.getItem("username")); // "JohnDoe"
```

4. Session Storage

- Simple key-value storage mechanisms built into browsers.
- Data is stored temporarily and is cleared when the session ends.

```js
sessionStorage.setItem("token", "abcd1234");
console.log(sessionStorage.getItem("token")); // "abcd1234"
```

5. IndexedDB

- IndexedDB is a low-level NoSQL database that allows storing structured data on the client side. It is useful for storing large amounts of data offline.

```js
let db;
const request = indexedDB.open("myDatabase", 1);

request.onsuccess = function (event) {
  db = event.target.result;
};

request.onupgradeneeded = function (event) {
  db = event.target.result;
  db.createObjectStore("users", { keyPath: "id" });
};
```

6. Application Cache

Best Practices for Client Side Caching
- Use Long Cache Durations for Static Assets: (e.g., images, fonts)
- Enable Service Workers for Offline Caching.
- Use `Cache-Control` Headers for Dynamic Content.
- Implement Cache Busting Strategies.
- Leverage IndexedDB for Storing Complex Data.
- Use `LocalStorage` / `SessionStorage` for Small, Non-Sensitive Data.
- Avoid Caching Sensitive Information.

Debugging Client-Side Cache Issues
- Open DevTools (`F12` or `Ctrl` + `Shift` + `I`).
- Go to the Network tab.
- Check the `Cache-Control` and `ETag headers`.
- Disable Cache: Network Tab > Disable cache (while DevTools is open).
- Clear Cache: Right-click Reload Button > Empty Cache and Hard Reload.

Real-World Use Cases

- `Progressive Web Apps (PWAs)` - 
- `API Caching` - 
- `Static Site Performance Optimization` - 

Comparison
| Technique         | Storage Limit          | Persistent?                | Use Case                           |
| ----------------- |:----------------------:| :-------------------------:| ---------------------------------: | 
| Cache-Control     | Depends on the browser | Yes (for defined duration) | General resource caching           |
| Service Workers   | Large                  | Yes                        | Offline access, caching strategies | 
| IndexedDB         | Large                  | Yes                        | Storing structured data            |
| LocalStorage      | 5-10MB                 | Yes                        | User preferences, small data       |
| SessionStorage    | 5-10MB                 | No                         | Temporary session-based data       |
