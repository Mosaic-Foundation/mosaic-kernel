# Mosaic Space Kernel

The shared core that every Mosaic space runs on. A Civic Hub is one kind of space. A representative's office, a
club, a newsroom or a family could be others. They all run on this kernel, with different plugins on top.

> **Status: split trial.** The kernel was drawn out inside the `civic-hub` repository first (Nov 17 to 19, 2026, merged
> to `civic-hub` dev in PR #22). This repository is where it moves. Until the trial passes, `civic-hub` main and dev
> keep running on their own copy, untouched. See [docs/SPLIT-TRIAL.md](docs/SPLIT-TRIAL.md).

## What the kernel does

The kernel does three things, and only these:

- **Identity**: sign-in, accounts, who belongs to a space, member and space DIDs.
- **Security**: who may do what, the audit log, and what each plugin is allowed to touch.
- **Interoperability**: the public record, the wire format other servers read, and federation (ActivityPub now,
  AT Protocol and others later).

To do these, it also runs the plugin system and the SDK that plugins are built with, plus the services every plugin
shares: settings, storage, export, scheduled jobs, mail, the search index, membership, and the lists of what is
waiting for review.

Everything else that people see and use is a plugin. That includes votes, proposals, events, the search page, the
Members page, pinning, feedback and the admin's daily email. The rule is **"if it can be a plugin, it is one."**

It is a bit like the Linux kernel: a small core with a stable interface, and everything people see is built on top.
A space type plays the part of a distribution. It decides what comes installed.

## The four layers

| Layer | What it is | Example | Where it lives |
| --- | --- | --- | --- |
| Kernel | The shared core above | this repository | `mosaic-kernel` |
| Plugin | A feature an admin can turn on or off: a **process** (votes, events), a **feature** (pinning, digests) or an **identity** method | Events | `mosaic-kernel/plugins/` for general ones, or its author's own repository |
| Space type | A starter kit: a profile (its words and role names), which plugins are required, on by default or off by default, default settings and sample content | Civic Hub | its own repository, e.g. `civic-hub` |
| Space | One running instance, run by its own admins | Floyd Civic Hub | a deployment |

## How a space type uses it

A space type's repository depends on a numbered kernel release, adds its profile, its own plugins and its branding,
and deploys. When the kernel releases, a bot opens an update pull request in each space type's repository. Its tests
and the kernel's suite run there, and merging updates every hosted space of that type at once. Self-hosters update
when they are ready.

## What is in this repository (planned at the split)

| Folder | What | From `civic-hub` today |
| --- | --- | --- |
| `kernel/` | The engine: routes, services, the plugin loader, identity, authorization, federation | `src/` (less hosting) |
| `sdk/` | `@mosaic/plugin-sdk`, the only thing a plugin imports | `kit/sdk/` (`@civic-social/plugin-sdk`) |
| `conformance/` | The suite every plugin, profile and space type passes | `kit/conformance/` |
| `ui/` | The web app shell and the components plugins draw with | `ui/src/` less civic words |
| `plugins/` | General plugins any space type can use | `packages/civic.*`, `features/*` |
| `docs/` | How it works and how to build on it | `docs/KERNEL.md`, `docs/kernel/SURFACE.md`, `docs/BUILD-A-PLUGIN.md` |

The Civic Hub space type (its profile, palette, sample content and civic-only plugins), and each deployment's
hosting (Vercel, Supabase), stay in `civic-hub`.

## Docs

- [Architecture](docs/ARCHITECTURE.md): the kernel, its services, plugins and space types, and where the edge runs.
- [Rules](docs/RULES.md): the eight rules that keep the kernel a kernel.
- [Building a plugin](docs/PLUGINS.md): what a plugin is and where to start.
- [Versions and conformance](docs/VERSIONING.md): releases, compatibility, and the four test layers.
- [Identity](docs/IDENTITY.md): what identity the kernel owns, and the outside groups Mosaic works with.
- [The split trial](docs/SPLIT-TRIAL.md): how this repository is being tried out before anything switches.

The decision record is ADR-020 ("Mosaic Space Kernel") in the Mosaic decisions folder.

## License

The kernel is free software under the **GNU Affero General Public License, version 3 or later (AGPL-3.0-or-later)**,
adopted by the Mosaic Foundation board on 2026-11-12.

Plugins don't have to be AGPL. As an additional permission under section 7 of the AGPL, a plugin that works with the
kernel only through the published plugin SDK and package contract may use any license its author chooses.

The specifications the kernel implements are published separately under Creative Commons Attribution 4.0, so anyone
can build another engine that passes the same conformance suite.

---

The Mosaic Space Kernel is stewarded by the Mosaic Foundation. Questions: contact@civic.social
