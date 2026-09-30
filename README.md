# template-spa

> Single-page app: Vite, React, TypeScript, Tailwind, dowel

A [lyrn](https://lyrn.lacodda.com) template of the `spa` form. lyrn generates from it only at a tag whose CI passed, and the CI here generates every combination of its add-ons and runs the gate each generated project ships with.

```console
$ lyrn new my-app --template lacodda/template-spa@v1.0.0
```

## Add-ons

- `--with router` - Screens at addresses: react-router, unknown ones sent home
- `--with tanstack-query` - Server state through TanStack Query, on the line's defaults
- `--with auth` - A cookie session and a sign-in screen until someone signs in
- `--with pwa` - Installable and offline: a manifest, its icons, a service worker

## Changing it

The project's files live under `template/`; `template.toml` says which belong to which add-on. `lyrn template check .` holds the template to what lyrn will accept. See [writing a template](https://lyrn.lacodda.com/reference/template/).
