# The split trial

How the kernel moves out of `civic-hub` into this repository without putting any running hub at risk. Agreed with
Adam on 2026-11-18: the split is tried first, and `civic-hub` main and dev stay untouched until every test passes.

## What moves, what stays

| Moves here | Stays in `civic-hub` |
| --- | --- |
| The engine (`src/`, apart from hosting) | Hosting: `src/hosting/`, `api/`, `vercel.json`, Supabase config |
| The SDK and the conformance suite (`kit/`) | The Civic Hub space type: `spaces/civic/` (profile, palette, sample words, seed) |
| The web app shell and the UI kit (`ui/src/`, less civic words) | Civic-only plugins, if any are named |
| General plugins (`packages/civic.*`, `features/*`) | Branding, legal pages, the release scripts and runbooks |
| The kernel's database migrations | The deployments' data (nothing is migrated by the split) |
| docs/KERNEL.md, docs/kernel/SURFACE.md, docs/BUILD-A-PLUGIN.md | HANDOFF.md and the hub's own docs |

## Steps

1. **Repository.** Adam creates the empty private `Mosaic-Foundation/mosaic-kernel`. This README and docs go in
   first.
2. **Copy, don't move.** The kernel's files are copied here from a named `civic-hub` commit. The first commit names
   that commit, and the full history stays in `civic-hub`. Nothing is deleted from `civic-hub` yet.
3. **Make it build on its own.** Set up npm workspaces (`kernel/`, `sdk/`, `conformance/`, `ui/`, `plugins/*`) and
   its own CI: unit tests, API tests against a bare Postgres and PostgREST (the run-anywhere job `civic-hub` already
   has), and the conformance suite on each plugin.
4. **A trial branch in `civic-hub`** (`kernel-split-trial`, from dev). It deletes the copied code and depends on the
   kernel at a pinned commit. Its CI runs everything: unit, API in both modes, E2E including the release checks, and
   conformance.
5. **A dev preview.** A Vercel preview of the trial branch runs, and the smoke suite and release checks pass against
   it.
6. **Switch,** only when steps 3 to 5 are green: merge the trial branch into dev and release dev as usual. Production
   follows with its normal release. To roll back, revert that one merge.

## Things to decide along the way

- **Package names.** The rename from `@civic-social/...` to `@mosaic/...` changes each plugin's signed catalog entry,
  because the signature covers the package's name. That means new entries and new approvals in the console. The
  trial keeps the current names, and the rename is its own step afterwards.
- **Private repository access.** While `mosaic-kernel` is private, `civic-hub`'s CI and Vercel builds need a
  read token to install it. The kernel is AGPL and meant to be open, so making it public at the switch removes this.
  That choice is Adam's.
- **Hashes.** A plugin's approval covers a fingerprint of its files. Copying a plugin unchanged keeps its fingerprint.
  The trial checks this, so the switch needs no new approvals.

## Done when

- `mosaic-kernel` builds and passes its own CI.
- `civic-hub` on the trial branch passes every suite it passes today.
- The dev preview passes smoke and the release checks.
- Every plugin's catalog fingerprint is unchanged.

## Size

The kernel was drawn out inside `civic-hub` over about three days (steps 1 to 4, Nov 17 to 19). The copy, the
workspace setup and the trial branch should take about two build sessions. Allow one more for whatever the trial
turns up.
