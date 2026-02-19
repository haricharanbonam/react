#note
- Hooks only works with 'use client' cuz by def next js treats all of them as Server Components
- Server Components run entirely on the server and produce static HTML  so they dont have access to browser handlers
- Client Components are still initially rendered on the server to provide a fast initial page load and SEO benefits,
-  but their JavaScript bundle is then sent to the client and executed (hydrated) in the browser to enable interactivity.

Am I reacting to a user action?
→ Use normal function.

Am I reacting to state change regardless of how it changed?
→ Use useEffect.
