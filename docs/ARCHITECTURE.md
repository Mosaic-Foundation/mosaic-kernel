# Architecture

## One kernel, many space types

Every Mosaic space (a Civic Hub, a representative's space, a club, a family, a newsroom) runs on the same kernel. The
kernel is small on purpose. It does three things:

- **Identity**: sign-in, accounts, who belongs to a space, member and space DIDs.
- **Security**: permission checks, the audit log, and what each plugin may touch.
- **Interoperability**: the public record, the wire format other servers read, and federation (ActivityPub now, AT
  Protocol and others later).

To do them it also runs the plugin system and the SDK plugins are built with, and the shared services every plugin
needs. It is not a space type and it carries no field's words.

| Term | Meaning | Example |
| --- | --- | --- |
| Kernel | The shared core. One codebase, stewarded by Mosaic. Replaces "space engine". | `mosaic-kernel` |
| Engine | A complete implementation that can run spaces. Mosaic's is the kernel plus plugins. Spaces are portable between engines. | Mosaic's engine |
| Profile | One field of work's words, roles, credential types and stricter rules, bound to the kernel. A document for people plus a small file the software reads. Shared by every space type in that field. | the civic profile |
| Space type | A starter kit: one profile, and for each plugin whether it is required, on by default or off by default; default settings; which samples a new space starts with. Consulted when a space is created. | Civic Hub |
| Space | One running instance, configured freely by its admins after it is created. | Floyd Civic Hub |
| Plugin | Anything in the plugin registry. Kinds: **process** (votes, events), **feature** (pinning, digests, Activity, Fediverse), **identity** (sign-in methods, credential exchange). A space's admins turn it on or off, unless its space type requires it. | Events |

The specs and the conformance suite are the kernel's constitution. They make it the reference implementation of an
open standard, not a proprietary centre, so a second engine that passes the suite is as good as ours.

## Services the kernel offers

Where a feature needs something every plugin shares, the shared part is a kernel service and the feature is a plugin.

| Kernel service | The plugin built on it |
| --- | --- |
| The search index, and one query over it | Search (the page and the top bar's box) |
| Membership: who belongs, the person's own "hide me" switch, Share this space | Members (the count, faces and the Members page) |
| The lists of what is waiting (reviews, feedback, the waiting list) and the counting of what is new | Admin digest (the admin's daily email), Feedback |
| The items service (read any item, whatever plugin made it) | Pinning (pin anything; urgent pins) |
| Process results and the "process closed" event | Briefs (the published record sent to officials) |
| Audited secret ballots and receipts | Votes |
| The Code of Conduct check (moderation) | Writing assistant (AI help while drafting) |
| Settings, storage, export, scheduled jobs, mail, signed unsubscribe links | every plugin |

Plugins reach these only through the plugin SDK (`@mosaic/plugin-sdk`, version 1). The full list of what the SDK
offers, and what each name became, is in `civic-hub`'s docs/kernel/SURFACE.md until it moves here.

## Roles and words

Kernel roles: **member**, **moderator**, **admin**, and **accountable** (a person who holds a public role and answers
for it: the civic profile calls them "official"; a club might say "coach"). Profiles name them; they cannot add a
role the kernel does not know.

Kernel terms (the terms service, `term("member", n)`; resolved space setting, then the profile's word for the space's
kind, then the profile's word, then the kernel's neutral word): `member`, `accountable`, `space`. `group` and
`outcome` are planned and join when two plugins need them. Add a term only when two plugins need it.

## Plugins draw into slots

The kernel's pages name no plugin. A plugin adds to them through generic slots: an item's action row (the Pin
button), the right column's cards (the Pinned card), the top of every page (the urgent line), the top bar's tools
(search), the space's links (Feedback), a section of its own (Members), the home page and the About card. A process
type adds its own pages, its feed card and filter pill, its colours (light, dark and email) and its sample content.

## Where the edge runs

| Layer | Today, in `civic-hub` | After the split |
| --- | --- | --- |
| Kernel | `src/` (the engine directories), `kit/` (SDK, conformance), `ui/src/kit/` and the host components plugins use | `mosaic-kernel` |
| Hosting | `src/hosting/`, `api/`, `vercel.json`, `supabase/config.toml` | stays with each deployment |
| Platform | `mosaic-console` (its own repository) | unchanged |
| General plugins | `packages/civic.*`, `features/*` | `mosaic-kernel/plugins/` |
| Civic space type | `spaces/civic/` (its record, profile, palette, sample content and development seed) | `civic-hub`: the civic profile, civic-only plugins, branding, hosting |
