---
name: astroedgearchitect
description: >
  Use when planning, building, debugging, reviewing, or deploying an Astro
  project. Inspect the existing project, map build/server/browser execution
  boundaries, justify rendering and hydration choices, make focused changes,
  and report observed verification. Prefer Astro-native multi-page architecture
  without unnecessary framework hydration or unsupported deployment claims.
---

# Astro Islands & Edge Optimization Architect

## Purpose

Build and review Astro sites through explicit execution boundaries,
intentional client-side JavaScript, and evidence-based verification.

Do not default to a React single-page application, introduce an unnecessary
client-side router, or hydrate an entire page when a smaller boundary meets
the requirement.

Frameworks, client-side navigation, and on-demand rendering are valid tools.
Use them when the project's requirements justify them.

This skill is self-contained. Do not require premium documents, boilerplate,
or scripts to follow it.

## 1. Select the Workflow

Follow the user's requested scope.

- Plan: inspect and propose decisions; do not edit files.
- Implement: make focused changes within the requested or approved scope.
- Audit: report evidence-backed findings; do not rewrite unless requested.
- Debug: reproduce or narrow the failure before proposing a correction.

If the request is ambiguous, begin with read-only inspection.
Ask a focused question only when the missing answer materially changes
architecture, security, deployment, or scope.

Do not require a separate planning ceremony for a trivial correction.

## 2. Inspect Before Editing

Locate the actual Astro project root before running commands.

Inspect relevant sources of evidence:

- `package.json`, scripts, package-manager declaration, and lockfile.
- `astro.config.*`.
- Affected pages, layouts, components, endpoints, and middleware.
- Existing framework and styling integrations.
- Deployment configuration and environment-variable names.
- Existing checks, tests, and project instructions.

Distinguish dependency ranges from resolved or installed versions.
Do not report a version range as a confirmed installed version.

Identify the intended host and runtime from evidence or the user's statement.
Do not infer the runtime merely from the hosting provider's name.

Preserve working integrations and unrelated configuration.
Use the existing package manager.
Do not install dependencies or regenerate lockfiles merely to inspect a project.

Do not expose secret values while inspecting configuration.

## 3. Record Execution Boundaries

For substantial changes, produce a compact boundary ledger before implementation.

For each affected route or component, identify:

| Boundary | Decision |
|---|---|
| Rendering | Build time, request time, or deferred server rendering |
| Browser behaviour | None, native browser script, or framework island |
| Hydration | None or the selected `client:*` directive |
| Data | Public, request-specific, or sensitive |
| Runtime | Required adapter/runtime, or not applicable |
| Caching | Relevant public/private caching considerations |
| Verification | Checks needed and current evidence |

Record only relevant fields. Use "unknown" rather than inventing an answer.

Example:

```text
Route: /pricing
Rendering: prerendered
Browser behaviour: native billing-period toggle
Framework hydration: none
Data: public
Request-time runtime: not required
Verification: build, displayed prices, keyboard interaction, repeated instances
```

Separate rendering from interactivity:

- A prerendered page may contain an interactive client island.
- Request-time rendering does not itself require browser hydration.
- A server island is not a hydrated framework island.
- CDN delivery does not establish edge execution.

Use the smallest execution boundary that satisfies the requirement,
unless an existing architecture or requirement justifies a broader one.

## 4. Justify New Browser JavaScript

Before adding browser code, consider:

1. Can semantic HTML and CSS satisfy the interaction?
2. Can a focused native browser script handle it?
3. Does a framework's state model, complexity management, or existing
   component ecosystem provide a clear benefit?

For each new script, hydrated component, or client-side dependency, explain:

- The user interaction it enables.
- Why the chosen implementation is appropriate.
- When it must become interactive.
- What remains usable before JavaScript loads or if it fails, where relevant.

Do not claim that native JavaScript is always smaller or better.
Do not claim bundle-size improvements without measuring comparable output.

Preserve an existing framework where replacing it would add unnecessary risk
or exceed the requested scope.

## 5. Astro Pages, Components, and Islands

- Use Astro file-based routes under `src/pages/`.
- Prefer `.astro` layouts and components for content and presentation.
- Do not use React hooks inside `.astro` components.
- Keep browser-only APIs out of server-executed frontmatter.
- Do not send secrets through rendered HTML, props, or browser scripts.
- Do not install a UI framework solely for a simple interaction.
- Do not add `client:*` directives to `.astro` components.

For framework components requiring browser interactivity:

- `client:load`: interaction is needed immediately.
- `client:idle`: interaction can wait until the browser is idle.
- `client:visible`: interaction can wait until the component is visible.
- `client:media`: hydration should depend on a matching media query.
- `client:only`: skip server rendering only when justified; provide the
  appropriate framework identifier and explain the trade-off.

Check directive support and options against the installed Astro version
when necessary.

Framework components that only render HTML do not need a client directive.

Do not delay hydration for critical controls merely to reduce initial work.
Explain the usability implications of the chosen timing.

### Server Islands

Consider `server:defer` when a separately rendered dynamic fragment can preserve
a useful static shell.

Do not use it by default.

Before introducing a server island, verify:

- Installed-version and adapter support.
- Whether delayed content and fallback behaviour are acceptable.
- What data and request context the island actually receives.
- Authentication and authorisation within the relevant server boundary.
- Cache behaviour and deployment requirements.

Do not assume that parent-page protection or request context automatically
applies to a separately requested fragment.

## 6. Native Script Reliability

Prefer Astro's normal processed `<script>` behaviour unless there is a
specific reason to opt out.

Do not add `is:inline`, `type="module"`, or other script attributes
without checking how they affect Astro processing.

For reusable interactive components:

- Support multiple instances on the same page.
- Scope selectors and state to the correct component instance.
- Avoid duplicate IDs and accidental global state.
- Prevent duplicate listeners or repeated initialisation.
- Clean up listeners, observers, or timers when the lifecycle requires it.
- Preserve native keyboard and form behaviour.

Inspect whether the project uses Astro's `ClientRouter`.

If it does, account for navigation lifecycle events and test navigation
away and back. Do not assume initial-load scripts behave identically
after client-side navigation.

Do not add a client router solely to make a component script work.

## 7. Tailwind and Component Contracts

Inspect the installed Tailwind version, integration, CSS entry points,
and existing design conventions before changing styling setup.

- Write complete utility names in source.
- Use finite variant maps or conditional complete strings.
- Never construct utilities from fragments such as `bg-${color}-600`.
- Use CSS custom properties or inline styles for genuinely dynamic values.
- Use `class:list` for conditional composition in `.astro` templates.
- Use the framework's appropriate class attribute in framework components.
- Do not assume `class:list` resolves conflicting utilities.
- Preserve working tokens and styling abstractions.
- Avoid `@apply` by default, but respect justified existing conventions.

For reusable components, define the contract:

- Semantic element and intended use.
- Supported props and variants.
- Native attributes and deliberate forwarding.
- Caller-supplied classes and conflict policy.
- Focus, disabled, and loading behaviour where relevant.

Use buttons for actions and links for navigation.
Do not imply that a link gains native disabled-button behaviour from styling.

Example:

```astro
***
interface Props {
  variant?: 'primary' | 'secondary';
}

const { variant = 'primary' } = Astro.props;

const variants = {
  primary: 'bg-blue-600 text-white hover:bg-blue-700',
  secondary: 'bg-slate-200 text-slate-900 hover:bg-slate-300',
};
***

<button
  type="button"
  class:list={[
    'rounded-lg px-5 py-3 font-semibold',
    'focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2',
    variants[variant],
  ]}
>
  <slot />
</button>
```

This illustrates syntax and complete class strings.
It is not a complete reusable-button API or an accessibility certification.

## 8. Rendering and Deployment

Choose rendering from data and request requirements, not from a preference
for a particular host.

- Prefer prerendering when content does not require request-time execution.
- For Astro 5 and later, use static output with route-level
  `export const prerender = false` where on-demand rendering is required.
- Use `output: 'server'` when request-time rendering should be the default.
- Do not generate `output: 'hybrid'` for Astro 5 or later.
- Configure a supported adapter when server execution is required.
- Preserve the user's chosen host.
- Verify installed-adapter options before adding runtime-specific settings.

Keep these concerns distinct:

1. CDN delivery of static output.
2. On-demand page or endpoint rendering.
3. Deferred server islands.
4. Middleware execution.
5. Cache behaviour.

Do not assume that all five use the same runtime or caching rules.

For request-specific responses, inspect whether shared caching could expose
personalised or protected data.

Do not describe a project as edge-rendered merely because its host has an
edge network.

A configuration option is not evidence that the deployed runtime was verified.

## 9. Middleware and Security Boundaries

- Match exact protected paths and intended descendants without accidentally
  matching unrelated routes.
- Identify which routes are prerendered and which execute on demand.
- Verify when middleware runs for the rendering mode and adapter.
- Never describe cookie presence alone as production authentication.
- Validate sessions and enforce authorisation before returning protected data.
- Protect relevant endpoints and separately requested fragments, not only pages.
- Do not rely on hidden links or client-side redirects as access control.
- Label demonstration-only security code explicitly.
- Keep secrets out of browser output, logs, examples, and repository commits.

Do not substitute a demonstration guard for an existing authentication system.

If security behaviour cannot be verified, report the specific uncertainty.

## 10. Diagnose Before Rewriting

For failures, separate the observation from the hypothesis.

Use this sequence:

1. Record the exact symptom or error.
2. Inspect the directly involved files and configuration.
3. Reproduce or narrow the failure when possible.
4. State the most plausible cause and supporting evidence.
5. Apply the smallest reasonable correction.
6. Repeat the relevant check.

Examples:

- Missing UI: inspect component contents, imports, props, and conditional
  rendering before replacing the styling system.
- Missing styles: inspect complete classes, source detection, CSS imports,
  and integration before reinstalling Tailwind.
- Broken interaction: inspect script execution, hydration, selectors,
  console errors, and navigation lifecycle before changing frameworks.
- Build failure: confirm the project root, command, and relevant configuration
  before adding packages.
- Unexpected redirect: inspect path matching, rendering mode, cookies,
  and middleware execution before weakening protection.

Do not turn an unconfirmed hypothesis into a broad rewrite.

## 11. Audit Findings

For each material finding, provide:

- Severity: critical, high, medium, or low.
- Evidence: file and relevant code or configuration.
- Consequence: what can fail and under what conditions.
- Correction: the smallest reasonable change.
- Verification: how to test the correction.
- Confidence: confirmed or requires investigation.

Use "critical" for plausible severe exposure or failure, not stylistic disagreement.
Do not present architectural preferences as defects without explaining
their concrete consequences.

Do not invent line numbers.
Keep confirmed findings separate from optional improvements.

## 12. Validate According to Risk

Run commands from the actual Astro project root.

Inspect available scripts before selecting commands.
Do not invent a test command or silently add tooling to obtain a green result.

For relevant changes:

- Run configured checks and tests.
- Run the production build.
- Check affected routes and interactions.
- Test repeated component instances where applicable.
- Test client-side navigation if present.
- Test middleware path boundaries and expected access outcomes.
- Inspect generated or deployed output when making claims about it.

Match verification depth to the change.
A documentation-only edit does not require pretending to test runtime behaviour.

Do not perform a deployment, destructive command, or unrelated environment
change merely to complete validation without appropriate user approval.

Distinguish:

- Static inspection.
- Local development behaviour.
- Production build success.
- Production-like local execution.
- Deployed runtime verification.

A successful build does not establish production authentication,
comprehensive accessibility, measured performance, or deployed edge execution.

## 13. Completion Report

End substantive implementation or debugging work with:

### Changed

Files modified and the purpose of each change.

### Decisions

Relevant rendering, browser-code, hydration, and runtime choices.
Keep this short; refer to the boundary ledger when already provided.

### Verified

Commands actually executed, observed results, and manual checks performed.

### Not Verified

Checks not run, unavailable environments, and unobserved deployment behaviour.

### Remaining Risks

Specific unresolved limitations or required follow-up.

Never describe a planned check as completed.
Never invent test output, performance figures, browser coverage,
installed versions, or deployment status.

For small changes, compress the report rather than repeating empty sections.

## Final Principle

Make the architecture explicit, keep the change proportionate,
and make every success claim traceable to evidence.