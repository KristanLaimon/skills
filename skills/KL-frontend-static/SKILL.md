---
name: kl-frontend-static
description: "Build or review minimal Vite + React + TypeScript frontends with explicit per-page choices for static HTML, hydration, and SPA behavior. Use for SEO-focused sites, interactive pages, or SPA sections without unnecessary frameworks."
metadata:
  display-name: KL-frontend-static
  version: "1.0.2"
---

# KL-frontend-static

Build the smallest maintainable frontend that meets the requirements. Prefer **Vite + React + TypeScript**, semantic HTML, native CSS, and browser APIs. JSX is fine for layout-only pages. Reuse the project's existing conventions; don't migrate or restructure without a reason.

**Static generation, hydration, and SPA behavior are independent, opt-in decisions per page or section.** Do not turn this preference into a framework.

## Choose the simplest rendering mode

| Need | Approach |
| --- | --- |
| Simple content, no component reuse | Plain HTML/CSS |
| Build-time content, JSX layout, SEO, no interactivity | React rendered to static HTML at build; no client React |
| SEO-readable content plus React interactivity | Prerendered HTML + hydration, preferably only for interactive parts |
| Small interaction on an otherwise static page | Native JS/TS; no hydration |
| Authenticated app, dashboard, complex shared state | Client-side React / SPA only where needed |

A statically hosted React shell is **not** prerendered, SEO-readable HTML. For SEO pages, essential content must exist in the delivered HTML. Do not prerender private or user-specific data.

## Rules

- **No default framework:** don't add Astro, Next.js, React Router, or similar unless requirements justify it; ask before introducing a full framework. Respect an explicitly requested stack.
- **No default SSR, SSG, hydration, router, or SPA:** select only what each route needs. No request-time SSR or production Node server unless requested.
- **No unnecessary dependencies or abstractions:** use Vite's multipage HTML support, native APIs, and existing project patterns before adding libraries, generators, or wrappers. Don't build a generic microframework.
- **Don't ship React to static-only pages** when build-time HTML is enough. Keep heavy SPA imports out of public-page bundles.
- **Render correctly:** `renderToStaticMarkup` is for HTML that will **not** be hydrated. Hydratable output needs `renderToString` (or compatible prerendering) and `hydrateRoot` with matching initial markup. Use `createRoot` for client-only React.
- **Vite does not prerender TSX automatically.** If needed, create only a small project-specific build step to compile server-side TSX, render required routes, emit complete HTML documents, and reference Vite's built assets.
- **Preserve fundamentals:** accessibility, semantic links/headings, validation, error handling, security, and correct 404s. Keep deep-link rewrites scoped to SPA routes.

## Workflow

1. Inspect the codebase and affected routes. Determine SEO needs, build-time vs. private content, interactivity, hosting, and what existing code must change.
2. Pick the least complex rendering mode **for each page**, not the whole site. Build only the requested functionality; avoid speculative structure and options.
3. For indexable pages, provide real HTML content, meaningful titles/descriptions, correct links and headings. Add canonical, sitemap, structured data, etc. **only when useful**.
4. Test the applicable output: build and preview, HTML content, no accidental JS on static pages, hydration without mismatches, SPA deep-link refresh, assets/base paths, and real 404s. Include a small test for new nontrivial logic.
5. Reply briefly: what mode you chose and why, what changed, what you checked, and any unverified risk. Do not claim checks you did not run.

**Default bias:** delete complexity before adding it. Optimize for the smallest complete change, not the fewest lines at the expense of correctness.

## Rendering Modes & Libraries

Choose per page. All modes are opt-in.

| Need | Libraries / APIs |
| --- | --- |
| Simple static content | HTML/CSS + Vite |
| JSX layouts, SEO, no interactivity | React + `react-dom/server` (`renderToStaticMarkup`) |
| SEO + React interactivity | `react-dom/server` (`renderToString`) + `react-dom/client` (`hydrateRoot`) |
| Small interactions | Native JS/TS + DOM APIs |
| Complex app / SPA | React + `react-dom/client` (`createRoot`) |

### Rules
- Use Vite for development and builds; TypeScript by default.
- For SSG, use a minimal build script to generate HTML. Vite does not prerender React automatically.
- Hydrate only what needs React interactivity.
- Never hydrate `renderToStaticMarkup` output.
- Use Vite's multi-page support when needed.
- No additional frameworks, routers, or libraries unless justified.
- Prefer static hosting; no runtime SSR unless requested.
- Keep each rendering mode independent.
