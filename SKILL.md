---
name: astroedgearchitect
description: >
  Use when building, modifying, reviewing, or deploying an Astro project.
  Enforce Astro-native multi-page architecture, minimal client-side JavaScript,
  deliberate island hydration, statically detectable Tailwind classes, and
  deployment configurations appropriate to the installed Astro version and host.
---

# Astro Islands & Edge Optimization Architect

## Purpose

Build Astro-native multi-page applications with minimal client-side JavaScript.

Do not default to a React single-page application, introduce an unnecessary
client-side router, or turn entire pages into hydrated framework components.

Respect existing project requirements. Explain architectural trade-offs before
adding dependencies or changing rendering and deployment behaviour.

## 1. Inspect Before Editing

Before changing an existing project:

- Read `package.json`, `astro.config.*`, and relevant page, component, and
  middleware files.
- Identify the installed Astro, adapter, framework, and Tailwind versions.
- Identify the intended deployment host and runtime.
- Preserve working integrations and unrelated configuration.
- Do not invent files, installed packages, test results, or deployment status.

Make the smallest change that satisfies the requirement.

## 2. Astro-Native Pages and Islands

- Use Astro file-based routing under `src/pages/`.
- Prefer `.astro` layouts and components for content and presentation.
- Render static HTML by default; add browser JavaScript only when needed.
- Never use React hooks such as `useState` or `useEffect` inside `.astro` files.
- Do not install React or another UI framework solely for simple browser
  interactions. Consider native HTML and a small Astro `<script>` first.
- Use framework components when their state management, complexity, or existing
  ecosystem provides a clear benefit.

For framework components that need browser interactivity, choose hydration
deliberately:

- `client:load`: immediately needed interactive UI.
- `client:idle`: lower-priority interactivity after initial page loading.
- `client:visible`: components that can wait until they enter the viewport.
- `client:media`: interactivity needed only for a matching media query.
- `client:only`: only when server rendering is unsuitable; explain why.

Framework components that only render HTML do not need a client directive.

Do not add hydration directives to `.astro` components.

## 3. Tailwind CSS Conventions

- Inspect the installed Tailwind version and existing integration before editing
  setup or CSS entry files.
- Prefer utility classes directly in markup.
- In `.astro` templates, prefer `class:list` when combining conditional classes.
- Write complete utility class names in source code.
- Use explicit variant maps or conditional complete strings.
- Never construct utilities from fragments such as `bg-${color}-600`.
- For genuinely dynamic values, consider CSS custom properties or inline styles
  rather than generating arbitrary utility names at runtime.
- Avoid `@apply` by default. Use it only when justified by an existing styling
  system or a clear reusable abstraction.
- Check accessible focus indicators, contrast, and disabled states.

Example:

```astro
***
interface Props {
  variant?: 'primary' | 'secondary';
}

const { variant = 'primary' } = Astro.props;
***

<button
  type="button"
  class:list={[
    'rounded-lg px-5 py-3 font-semibold',
    {
      'bg-blue-600 text-white hover:bg-blue-700': variant === 'primary',
      'bg-slate-200 text-slate-900 hover:bg-slate-300': variant === 'secondary',
    },
  ]}
>
  <slot />
</button>
```

## 4. Rendering and Deployment

Choose rendering per route and project requirements.

- Prefer prerendering for content that does not require request-time execution.
- On Astro 5 and later, use default static output with
  `export const prerender = false` on routes that require on-demand rendering.
- Use `output: 'server'` when on-demand rendering should be the project default.
- Do not generate `output: 'hybrid'` for Astro 5 or later.
- Distinguish prerendered endpoints from endpoints that require request-time
  cookies, authentication, personalised responses, or other dynamic inputs.
- Install and configure the deployment adapter whenever any route requires
  on-demand rendering.
- Match the adapter to the intended host. Do not substitute Cloudflare
  configuration for Vercel, or vice versa.

Treat these as separate architectural concerns:

1. Static assets delivered through a CDN.
2. Pages and endpoints rendered on demand.
3. Middleware executing in a specific server or edge runtime.

Do not describe a project as edge-rendered merely because its host has an edge
network.

Check the installed adapter's supported options before generating edge-specific
configuration. Ensure dependencies and APIs are compatible with the runtime
where they execute.

## 5. Middleware and Security

- Account for both exact protected paths and protected descendants.
- Identify which routes are prerendered and which execute on demand.
- Verify when middleware runs for the chosen rendering mode and adapter.
- Never present cookie presence alone as production authentication.
- For real protected routes, validate sessions and enforce authorisation
  server-side before returning protected data.
- Keep secrets out of browser code, public environment variables, examples,
  and repository commits.
- Clearly label demonstration-only security code.

## 6. Validation and Reporting

Run commands from the Astro project's root, where its `package.json` lives.

Before reporting completion:

- Check that changed pages and components render.
- Test relevant interaction and middleware behaviour.
- Run the project's production build script.
- Run configured checks and tests relevant to the changes.
- Report exactly which checks were run and their results.
- Distinguish local development checks, production build validation, and
  deployed runtime verification.
- If a check could not be run, say so.

A successful build does not prove deployed edge execution or production security.

Do not claim that instructions guarantee correct AI output. Apply these rules, inspect the generated code, and verify the result.