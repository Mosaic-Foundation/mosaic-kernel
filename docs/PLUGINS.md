# Building a plugin

A plugin adds something to a space: a **process** type (a vote, an event, a straw poll), a **feature** (pinning, a
digest, search) or an **identity** method (a way to sign in, a credential to check). The kernel gives it a home: who may
take part, the review queue, the public record, the feed, closing on a date, archiving, export and federation. The
plugin says what its type is and what its actions do.

A plugin is an npm-style package:

```
straw-poll/
  package.json     its name and version
  index.ts         what the kernel loads
  manifest.ts      what the type is, as plain data (its words, colours, settings, what it may touch)
  handler.ts       what its actions do
  store.ts         the records it keeps, in its own table
  samples.ts       its sample content for a demo space
  ui.tsx           its pages and what it draws into the kernel's slots
  migrations/      the SQL that makes its table (additive only)
```

## The rules a plugin keeps

- It imports only the plugin SDK (`@mosaic/plugin-sdk`), its own files, Node's built-ins and its declared peer
  dependencies. Never another plugin, never the kernel's internals.
- It works with every other plugin switched off. A process shows its complete outcome on its own closed page.
- It uses the kernel's neutral words (`member`, `accountable`, `space`) through `term()`, so each space type's
  profile can name them.
- Its migrations only add.

## From folder to running in a space

1. **Build it** against the SDK. The full guide, with a worked straw poll of about 250 lines, is
   `civic-hub`'s docs/BUILD-A-PLUGIN.md, and moves here at the split.
2. **Run the conformance suite** on your machine (`npx civic-conformance ./straw-poll`, to be renamed). It starts a
   real space with your plugin in it and walks your type through every step it claims: creation, review, every action
   and who may take it, deadlines, archive, switched off and on, the events it writes, its feed card and pages.
3. **Submit it** to Mosaic with the suite's output and a short note. A person reads the code, checks that the grant
   (what it asks to touch, in the words an admin will read) is true and no wider than it needs.
4. **Approval is per version and signed.** The signature covers the package's name, version, a fingerprint of its
   files and its grant. A space runs a plugin only when its files are exactly the signed ones.
5. **Each space's admins choose** to install it, after reading its grant. A new version that asks for more waits for
   their OK.

Plugins don't have to be AGPL: one that works with the kernel only through the SDK and the package contract may use
any license its author chooses.
