# Contributing to {{ title }}

## The gate

```console
$ pnpm install          # once
$ pnpm lint             # eslint + types + tests
$ pnpm build            # a production bundle in dist/
```

`pnpm lint` is the whole gate, and CI runs that same script rather than a
sequence of its own, so a green terminal means a green pull request.

## Adding a component

Primitives are copied in from the dowel registry into `src/components/ui/`:

```console
$ npx shadcn add {{ registry }}/badge.json
```

A lowercase file there is dowel's copy and stays dowel's: `pnpm lint` compares
each one with the registry of the dowel-ui this project installed, so an
upgrade that forgets to take the copies again fails the gate. A component of
this project's own is named in PascalCase. The full set is listed at
<https://lacodda.github.io/dowel>.

## The look

This project states one thing about its own appearance — its accent, in
`src/styles.css`:

```css
:root {
  --accent-base: {{ accent }};
}
```

Everything else follows from it: the hover shade, the soft fill, the focus
ring, the tint the greys carry, and what colour text sits on an accent fill.

A component never writes a colour down; it names a token, so the theme can swap
it underneath. `pnpm lint` enforces this.
{{#router}}

## Screens

Screens live at addresses, through [React Router](https://reactrouter.com).
`src/App.tsx` lists them - a route is one line there and one component in
`src/pages/` - and an address nothing answers goes home rather than to a blank
page. A reload on a deep address needs the server to answer every path with
`index.html`; `pnpm dev` and `pnpm preview` already do.
{{/router}}
{{#tanstack-query}}

## Server state

Data from a server goes through [TanStack Query](https://tanstack.com/query):
one client in `src/lib/query.ts`, on the line's defaults - fresh for 30
seconds, no refetch on every return to the window, and a failed query fails at
once so the screen can say so instead of spinning through three retries.
{{/tanstack-query}}
{{#auth}}

## Signing in

The session is an HttpOnly cookie, out of reach of any script on the page, so
the app asks the server who is signed in rather than remembering it. Until
somebody is, it shows the sign-in screen. It expects three endpoints:

| Endpoint | Answers |
| --- | --- |
| `GET /api/me` | `{ "email", "name" }`, or 401 when nobody is signed in |
| `POST /api/auth/login` | `{ "email", "password" }` in; 204 and the cookie, or 401 |
| `POST /api/auth/logout` | 204, with the cookie cleared |

In development `/api` is forwarded to `localhost:8080`, where the line's
service form listens.
{{/auth}}
{{#pwa}}

## Installing it

The app installs from the browser and opens without a network:
`public/manifest.webmanifest` names it and its icons, and `public/sw.js`
serves the last build it saw when the network is gone - network first, so a
deploy shows on the next load. The API is never cached: yesterday's data shown
as today's is worse than an error. The worker is registered in production
builds only, where each build gets a cache of its own.
{{/pwa}}

## Layout

| Path | What lives there |
| --- | --- |
| `src/` | The application |
| `src/App.tsx` | Which screen is showing |
| `src/pages/` | The screens |
| `src/components/ui/` | dowel's primitives, copied from the registry |
| `src/lib/utils.ts` | `cn()`, the class-name joiner the primitives use |
{{#tanstack-query}}
| `src/lib/query.ts` | The query client and its defaults |
{{/tanstack-query}}
{{#auth}}
| `src/lib/api.ts`, `src/lib/session.tsx` | The API client and who is signed in |
| `src/test/session.tsx` | The server, as tests stand in for it |
{{/auth}}
| `src/styles.css` | Tailwind, the dowel theme, this product's accent |
| `public/favicon.svg` | The mark at level S; the line's umbrella mark until this product has one |
{{#pwa}}
| `public/manifest.webmanifest`, `public/sw.js` | What an install reads, and what serves offline |
{{/pwa}}
| `tools/check-registry.mjs` | Holds the copies - primitives and favicon - to the installed dowel-ui |
| `tools/check-licenses.mjs` | Holds every production package to the accepted licenses |
| `assets/` | The mark's three master SVGs |
| `docs/adr/` | Architecture decisions, newest last |
| `.github/workflows/ci.yml` | The gate: lint, types, tests, build |
| `.github/workflows/audit.yml` | Advisories and licenses, on every push and every Monday |

## Commits

[Conventional Commits](https://www.conventionalcommits.org/), in English:
`feat:`, `fix:`, `docs:`, `chore:` and the rest. The changelog is generated
from them (`cliff.toml`), so a commit message is the line the release notes
will carry.

## Decisions

An architectural decision gets a record in [`docs/adr/`](docs/adr/README.md):
one file, numbered, newest last. A decision that replaces an earlier one says
so, and the earlier one is marked superseded rather than deleted.

## Dependencies

Updated by hand, at most once a week, in a commit of their own - Dependabot is
off on purpose ([ADR 0002](docs/adr/0002-dependabot-is-off.md)). The audit
workflow fails on a published advisory, and on a dependency whose license is
not on the accepted list.

## Assistant files

Instructions for coding assistants - `CLAUDE.md`, `AGENTS.md`, `.claude/`,
`.cursor/` and the like - stay on the machine they were written for, and
`.gitignore` keeps them out of the repository. What anyone needs to know about
the project, human or agent, is in this file and in [`llms.txt`](llms.txt).

## Conduct and security

Everyone taking part follows the [Code of Conduct](CODE_OF_CONDUCT.md). A
vulnerability is reported privately, as [SECURITY.md](SECURITY.md) describes -
never in a public issue.

## License

By contributing, you agree that your contributions are licensed under the
project's [MIT license](LICENSE).
