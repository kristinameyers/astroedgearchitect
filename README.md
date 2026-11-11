# Astro Islands & Edge Optimization Architect

An open-source AI coding skill for building Astro-native multi-page
applications instead of defaulting to React single-page app patterns.

Designed for use with AI coding assistants such as Claude Code and Cursor,
subject to each tool's supported instruction and skill-loading mechanisms.

## What It Guides

- Astro file-based routing and `.astro` components.
- Static HTML by default, with deliberate island hydration.
- Small browser scripts instead of unnecessary framework dependencies.
- Complete, statically detectable Tailwind utility classes.
- Version-aware rendering and deployment configuration.
- Clear distinctions between CDN delivery, server rendering, and edge middleware.
- Honest validation reports and explicitly labelled security demonstrations.

## Public Package

```text
astroedgearchitect/
├── SKILL.md
├── README.md
└── LICENSE
```

`SKILL.md` is self-contained. It does not require paid reference documents,
boilerplate, or scaffolding scripts.

## Usage

Use your coding assistant's documented mechanism for loading local skills or
project instructions.

If your tool supports `SKILL.md` packages, install the skill in its documented
location. Otherwise, provide `SKILL.md` as project context and explicitly ask
the assistant to follow it.

Do not assume the file's presence alone enables automatic discovery or
activation in every coding tool.

Example prompt:

> Follow the Astro Islands & Edge Optimization Architect skill.
> Inspect the installed versions and existing configuration first.
> Build this feature using Astro-native routing and minimal client JavaScript.
> Explain the rendering and hydration choices, then report the checks you ran.

## Architectural Defaults

1. Prefer Astro pages, layouts, and components.
2. Add client-side JavaScript only where needed.
3. Choose hydration directives according to interaction priority.
4. Keep Tailwind class names complete and detectable.
5. Prefer prerendering unless a route needs request-time execution.
6. Match server adapters and runtime configuration to the deployment target.
7. Verify behaviour instead of treating generated code as correct by default.

These are defaults, not a reason to ignore an existing project's requirements.

## Validation Record

The companion development sandbox was locally tested with:

- Astro 7.4.11.
- @astrojs/vercel 11.0.21.

Observed results:

- Primary, secondary, and disabled button variants rendered with the intended
  styling.
- An admin-route demonstration redirected requests without a session cookie.
- The same demonstration allowed a request containing a dummy session cookie.
- The trailing-slash admin route was also redirected without a cookie.
- Production builds completed successfully with the Vercel adapter.

These checks do not establish production authentication, comprehensive
accessibility, or deployed Vercel edge execution.

The cookie example demonstrates routing behaviour only. Cookie presence is
not session validation.

This record describes the tested sandbox, not a guarantee of compatibility
with every Astro release, project, or deployment platform.

## Public and Premium Editions

The public edition provides standalone architectural guardrails.

The separate Agensi premium edition adds implementation resources:

- Local reference documents.
- Custom boilerplate assets.
- Scaffolding scripts.
- A routing skill that directs the assistant to those resources.

Premium assets are not included in this repository.

## Limitations

AI assistants can misinterpret instructions or generate incorrect code.

This skill guides architecture and verification; it does not replace code
review, security review, runtime testing, or deployment validation.

## Author

OptiWeb / Syntax Automations

## License

MIT. See `LICENSE`.