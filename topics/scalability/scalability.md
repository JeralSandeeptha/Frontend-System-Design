# Scalability Strategies

For large-scale applications, consider:

1. **Monorepo vs. Polyrepo**
- `Monorepo (Nx, Turborepo)` → One repo for multiple frontend modules.
- `Polyrepo` → Separate repositories for different frontend services.

2. **Edge Computing & CDNs**

- `CDN (Cloudflare, Fastly)` → Distributes content globally for faster load times.
- `Edge Functions` → Run logic closer to the user (e.g., Cloudflare Workers).
