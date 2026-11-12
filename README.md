# Astro Islands & Edge Optimization Architect

An open-source AI coding skill for building and reviewing Astro-native
multi-page sites and applications—without defaulting to React single-page
app patterns.

Use it to make rendering and hydration decisions explicit, keep client-side
JavaScript intentional, and separate verified results from untested assumptions.

The public skill is self-contained. No paid references, boilerplate, or
scaffolding scripts are required.

## When to Use It

Use this skill when asking an AI coding assistant to:

- Plan a new Astro page or feature.
- Add interactivity without unnecessarily expanding the client-side application.
- Review an existing project for architecture drift.
- Diagnose rendering, routing, styling, or deployment configuration problems.
- Explain which checks actually passed and which remain unverified.

The goal is not to ban React, other UI frameworks, or client-side navigation.
It is to use them deliberately when the project benefits from them.

## What It Guides

- Astro file-based routing, pages, layouts, and `.astro` components.
- Static HTML by default, with deliberate client-island hydration.
- Small browser scripts where a framework is unnecessary.
- Complete, statically detectable Tailwind utility classes.
- Version-aware rendering and deployment configuration.
- Clear distinctions between CDN delivery, server rendering, and edge middleware.
- Minimal changes that respect an existing project's requirements.
- Honest validation reports and explicitly labelled security demonstrations.

“Edge” is a deployment consideration, not a requirement for every project.

## Public Package

```text
astroedgearchitect/
├── SKILL.md
├── README.md
└── LICENSE
```

`SKILL.md` contains the complete public skill.

## Getting Started

Use your coding assistant's documented mechanism for loading local skills
or project instructions.

If your tool supports `SKILL.md` packages, install the skill in its documented
location. Otherwise, provide `SKILL.md` as project context and explicitly ask
the assistant to follow it.

Do not assume that the file's presence alone enables automatic discovery
or activation.

This skill is intended for use with AI coding assistants such as Claude Code
and Cursor, subject to each tool's supported instruction-loading mechanisms.

### Start With a Read-Only Review

```text
Follow the Astro Islands & Edge Optimization Architect skill.

Inspect this Astro project without changing files.

Identify:
- Installed Astro and relevant integration versions.
- Existing pages, layouts, and component boundaries.
- Rendering configuration and deployment adapter.
- Hydrated components and their interaction requirements.
- Potential unnecessary client-side JavaScript.
- Configuration or verification gaps.

Return prioritised findings with file-level evidence.
Distinguish confirmed findings from questions requiring further investigation.
Do not install dependencies or modify configuration.
```

## Three Practical Workflows

### 1. Plan a Feature

```text
Follow the Astro Islands & Edge Optimization Architect skill.

Plan this feature before implementing it:
[describe the feature]

Inspect the existing project first.

Explain:
- Which routes and components are involved.
- Which content can be prerendered.
- Whether anything requires request-time execution.
- Which interactions need browser JavaScript.
- Whether an isolated framework island is justified.
- Which checks will verify the implementation.

Preserve working configuration where possible.
Wait for approval before making changes.
```

### 2. Implement an Approved Plan

```text
Follow the Astro Islands & Edge Optimization Architect skill.

Implement the approved plan with minimal, focused changes.

Use Astro-native routing and components where appropriate.
Justify any new dependency or client hydration directive.
Preserve unrelated code and working configuration.

Run the available checks.
Report changed files, observed results, unverified behaviour,
and remaining risks.
```

### 3. Audit an Existing Implementation

```text
Follow the Astro Islands & Edge Optimization Architect skill.

Review this implementation for:
- Unnecessary framework hydration.
- Overly broad client-side application boundaries.
- Rendering choices that do not match the route's requirements.
- Fragment-built Tailwind utility names.
- Deployment assumptions unsupported by the installed configuration.
- Claims of security, accessibility, or performance without evidence.

Do not rewrite the application.

For each finding, provide:
- Severity.
- File or configuration evidence.
- Why it matters.
- The smallest reasonable correction.
- How to verify the correction.
```

## Architectural Defaults

These are starting points, not absolute rules.

1. Prefer Astro pages, layouts, and components.
2. Prefer prerendering when content does not require request-time execution.
3. Use HTML and CSS where they satisfy the requirement.
4. Add small browser scripts for focused interactions when appropriate.
5. Use framework islands when their state, interaction, or component ecosystem
   justifies them.
6. Choose hydration directives according to when interaction is needed.
7. Keep Tailwind utility names complete and statically detectable.
8. Match adapters and runtime configuration to the chosen deployment target.
9. Inspect installed versions before proposing version-specific configuration.
10. Verify behaviour instead of treating generated code as correct by default.

An existing project's requirements may justify a different choice.
The assistant should explain that choice rather than silently imposing a
different architecture.

## Make Architecture Decisions Visible

Before substantial changes, ask for a short decision record.

Example:

```text
Route: /pricing
Rendering: prerendered
Interaction: billing-period toggle
Implementation: small native browser script
Framework hydration: none
Request-time server execution: not required
Reason: page content does not depend on the incoming request

Planned verification:
- Production build
- Toggle behaviour
- Keyboard interaction
- Displayed price accuracy
```

This is an illustrative plan, not a report of completed checks.

For more complex features, the record should also explain deployment
requirements, data access, authentication boundaries, and unresolved questions.

## Expected Completion Report

A useful implementation response should separate four things:

```text
Changed
- Files modified and why.

Verified
- Commands actually executed.
- Results actually observed.
- Manual checks actually performed.

Not verified
- Checks that were unavailable or not run.
- Deployment behaviour not observed.
- Browser or assistive-technology testing not performed.

Remaining risks
- Specific unresolved limitations.
- Recommended follow-up checks.
```

A suggested check is not a completed check.
A successful local build is not proof of deployed runtime behaviour.

## Validation Record

The following record describes the companion development sandbox.
It is not a compatibility matrix or a benchmark of AI coding assistants.

### Recorded Environment

- Astro 7.4.11.
- `@astrojs/vercel` 11.0.21.

### Recorded Observations

- Primary, secondary, and disabled button variants rendered with the intended
  styling.
- An admin-route demonstration redirected requests without a session cookie.
- The same demonstration allowed a request containing a dummy session cookie.
- The trailing-slash admin route also redirected without a cookie.
- Production builds completed successfully with the Vercel adapter.

### What These Checks Do Not Establish

- Production authentication or authorisation.
- Comprehensive accessibility.
- Measured performance improvements.
- Deployed Vercel edge execution.
- Compatibility with every Astro release or deployment platform.
- Reliable behaviour across every AI coding assistant.

The cookie example demonstrates routing behaviour only.
Cookie presence is not session validation.

## Public and Premium Editions

The public edition provides standalone architectural guardrails.
It does not require the premium edition.

The separate Agensi premium toolkit is intended to add implementation
resources such as:

- Local reference documents.
- Reusable boilerplate assets.
- Scaffolding scripts.
- A routing skill that directs the assistant to those resources.

Premium assets are not included in this repository.
Their availability and licensing are separate from this public release.

## Reporting Problems

When reporting a problem, include:

- The coding assistant used.
- Relevant Astro and integration versions.
- A minimal description of the project and deployment target.
- The prompt or instruction that triggered the problem.
- The generated behaviour.
- The behaviour you expected.
- Relevant error output or a minimal reproduction.

Remove secrets, session cookies, tokens, and private project information
before sharing examples.

Reports showing unnecessary hydration, unsupported configuration changes,
or unverified success claims are especially useful.

## Limitations

AI assistants can misinterpret instructions and generate incorrect code.

This skill guides architecture and verification. It does not replace code
review, security review, accessibility testing, performance measurement,
or deployment validation.

It does not guarantee a particular output, runtime, performance score,
or production-readiness standard.

## Author

OptiWeb / Syntax Automations

## License

MIT. See `LICENSE`.