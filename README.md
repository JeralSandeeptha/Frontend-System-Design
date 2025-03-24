# Frontend System Design

Frontend system design is all about planning and architecting the user interface of an application, ensuring scalability, maintainability, and performance. It involves decisions about component structure, state management, performance optimizations, and UI/UX best practices.

## Table of Contents

- [Frontend System Design](#frontend-system-design) 
- [Frontend Architecture](#frontend-architecture)
- [Frontend Technologies](#frontend-technologies)
- [Component Design Principles](#component-design-principles)
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
- [Scalability Strategies](#scalability-strategies)
- [Best Practices](#best-practices)

## Frontend System Design

Frontend system design is all about planning and architecting the user interface of an application, ensuring scalability, maintainability, and performance. It involves decisions about component structure, state management, performance optimizations, and UI/UX best practices.

## Frontend Architecture

Frontend architecture defines how a web application is structured, ensuring modularity, reusability, and scalability.

1. Monolithic Frontend

- All UI components and logic exist in a single repository.
- Example: Traditional MVC frameworks (Angular, old React apps).

2. Micro Frontends

- Different parts of the UI are developed and deployed independently.
- Example: Different teams own separate modules (React for one, Vue for another).

3. Server-Driven UI

- The backend dictates the UI structure dynamically.
- Example: GraphQL-powered frontends, React Native with Backend-driven UI.

4. Progressive Web Apps (PWAs)
- Web apps that behave like native mobile apps with offline support.

## Frontend Technologies

| Framework     | Pros                                                     | Cons                                                |
| :------------ |:--------------------------------------------------------:| --------------------------------------------------: |
| React         | Component-based, Virtual DOM, Huge community             | Requires additional libraries for state management  |
| Vue.js        | Simplicity, Reactive data binding, Great for small apps  | Smaller ecosystem than React                        |
| Angular       | Full-fledged framework, Two-way data binding             | Steeper learning curve                              |
| Svelte        | No Virtual DOM, Highly optimized                         | Smaller community                                   |

📌 Best for:

- React → Scalable applications

- Vue → Small to mid-sized apps

- Angular → Enterprise-grade apps

- Svelte → High-performance apps

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

```ts
const fetchData = async () => {
  const cachedData = localStorage.getItem('myData');

  if (cachedData) return JSON.parse(cachedData);

  const response = await fetch('https://api.example.com/data');
  const data = await response.json();

  localStorage.setItem('myData', JSON.stringify(data)); // Cache the response
  return data;
};

export default function MyComponent() {
  const [data, setData] = React.useState(null);

  React.useEffect(() => {
    fetchData().then(setData);
  }, []);

  return <div>{JSON.stringify(data)}</div>;
}
```

4. Session Storage

- Simple key-value storage mechanisms built into browsers.
- Data is stored temporarily and is cleared when the session ends.

```js
sessionStorage.setItem("token", "abcd1234");
console.log(sessionStorage.getItem("token")); // "abcd1234"
```

```ts
const fetchData = async () => {
  const cachedData = sessionStorage.getItem('myData');

  if (cachedData) return JSON.parse(cachedData);

  const response = await fetch('https://api.example.com/data');
  const data = await response.json();

  sessionStorage.setItem('myData', JSON.stringify(data));
  return data;
};
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

This is a React example
```cmd
npm install idb-keyval
```

```ts
import { get, set } from 'idb-keyval';

const fetchData = async () => {
  const cachedData = await get('myData');
  if (cachedData) return cachedData;

  const response = await fetch('https://api.example.com/data');
  const data = await response.json();

  await set('myData', data);
  return data;
};
```

6.  In-Memory Caching (Context API / React Query)

```cmd
npm install @tanstack/react-query
```

This is Context API
```ts
import React, { createContext, useContext, useState } from 'react';

const CacheContext = createContext(null);

export const CacheProvider = ({ children }) => {
  const [cache, setCache] = useState({});

  const setCachedData = (key, data) => {
    setCache((prev) => ({ ...prev, [key]: data }));
  };

  return (
    <CacheContext.Provider value={{ cache, setCachedData }}>
      {children}
    </CacheContext.Provider>
  );
};

export const useCache = () => useContext(CacheContext);
```

```ts
import { useCache } from './CacheContext';

const MyComponent = () => {
  const { cache, setCachedData } = useCache();

  React.useEffect(() => {
    if (!cache['myData']) {
      fetch('https://api.example.com/data')
        .then((res) => res.json())
        .then((data) => setCachedData('myData', data));
    }
  }, [cache, setCachedData]);

  return <div>{JSON.stringify(cache['myData'])}</div>;
};
```

This is react query
```ts
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

const fetchData = async () => {
  const { data } = await axios.get('https://api.example.com/data');
  return data;
};

export default function MyComponent() {
  const { data, error, isLoading } = useQuery({
    queryKey: ['myData'], // Cached key
    queryFn: fetchData, 
    staleTime: 1000 * 60 * 5, // Cache for 5 minutes
  });

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return <div>{JSON.stringify(data)}</div>;
}
```

7. Application Cache

- HTML5 introduced Application Cache (AppCache), but it has been deprecated in favor of Service Workers.

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

## Testing and Quality Assurance

Testing ensures reliability and stability.

- Unit Tests (Jest, Mocha) – Test small components.

- Integration Tests (React Testing Library) – Test interaction between components.

- End-to-End (E2E) Tests (Cypress, Playwright) – Simulate real user behavior.

📌 Best Practices: 
✔️ Follow the AAA Pattern (Arrange, Act, Assert).
✔️ Use mocking for API calls.
✔️ Aim for 80%+ test coverage.

## State Management

Managing state is crucial for a scalable frontend system.

1. Local State (Component-level state)

- Managed using useState in React.

- Global State (Shared across multiple components)

2. Context API – Lightweight, good for small apps.

- Redux – Centralized store, good for large apps.

- Recoil / Zustand – Alternative lightweight state managers.

3. Server State (Remote data fetched from APIs)

- React Query, SWR – Helps manage caching and API fetching efficiently.

## Deployment and CI/CD

Deploying frontend apps requires automation and scalability.

1. Hosting Options:

- Static Hosting → `Vercel`, `Netlify`, `GitHub Pages`.

- Cloud-Based Hosting → `AWS S3`, `Firebase` Hosting.

- Server-Side Rendering (SSR) → `Next.js` with `Vercel`.

2. CI/CD Pipeline:

Code Push → `GitHub` / `GitLab Actions` trigger build.

- Lint & Test → `ESLint`, `Jest` run checks.

- Build & Optimize → `Webpack`, `Babel` process code.

- Deploy → Push to `Netlify`/`Vercel`/`AWS`.

## Scalability Strategies

For large-scale applications, consider:

**Monorepo vs. Polyrepo**
- `Monorepo (Nx, Turborepo)` → One repo for multiple frontend modules.

- `Polyrepo` → Separate repositories for different frontend services.

**Edge Computing & CDNs**

- `CDN (Cloudflare, Fastly)` → Distributes content globally for faster load times.

- `Edge Functions` → Run logic closer to the user (e.g., Cloudflare Workers).

## Best Practices

✔️ Use modular components for reusability.
✔️ Optimize for mobile-first and responsive design.
✔️ Follow SOLID principles for frontend architecture.
✔️ Use Linting & Prettier for code consistency.
✔️ Always version APIs for backward compatibility.
✔️ Use design systems like Material UI, Tailwind CSS, or Chakra UI.