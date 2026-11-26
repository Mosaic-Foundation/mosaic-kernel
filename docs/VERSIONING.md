# Versions and conformance

## Versions, updates, compatibility

- The kernel repo releases numbered versions. A space type's repo depends on one (`"@mosaic/kernel": "^1.4.0"`).
- Before a release, the kernel runs Civic Hub's tests against itself. A bot opens an update pull request in each space
  type's repo; its tests and the kernel suite run there; merging and deploying updates every hosted space of that
  type at once. Self-hosters take the release when ready.
- A plugin declares the kernel range it works with; the loader refuses a plugin the kernel cannot satisfy.
- Plugin capability changes still wait for each space's admin (ADR-015).

## Conformance

Four layers: the kernel suite (every space type passes it: sign-in, permissions, the public record, plugin loading,
export); the profile check (every word maps to a kernel term; nothing redefines one); the plugin suite run under two
profiles and with each other plugin off and every optional service empty; the space-type check (defaults and pins
load on a fresh space, which serves its public feed, discovery file and a full export). Today only the plugin suite
exists (`civic-conformance`).
