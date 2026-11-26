# Rules

The eight rules that keep the kernel a kernel. Every change to the kernel or a plugin is checked against them. They
come from `civic-hub`'s docs/KERNEL.md, where the kernel was drawn out first.

1. **Plugins depend on the kernel, never on each other.** A plugin talks to the kernel's services and events. It
   never imports another plugin's code or reads its tables. When two plugins seem to need each other, in order:
   merge them; move the shared piece into the kernel; listen for an event; an optional enhancement (`enhancedBy`);
   last, a declared `requires` (one level deep, a plain `why` shown to admins).
2. **The kernel never names a plugin.** No `isPluginEnabledSync("brief")`, no import of a package. The kernel emits
   events ("a process closed") and asks for contributions plugins register.
3. **Every combination works.** Switching any plugin off leaves every other plugin working, possibly with less.
   Every process plugin shows its complete outcome on its own closed page with Briefs off (Adam, 2026-11-17).
4. **Words are free, meanings are fixed.** Kernel and plugin code use neutral words. A profile renames them; it never
   invents a new kind of thing or changes what one means. Anything a profile adds names the kernel thing it maps to.
5. **If it can be a plugin, it is one** (Adam, Nov 19; replaces the Nov 17 kernel test). The kernel holds only
   what identity, security and interoperability need, and the shared services plugins are built on. Everything else a
   person sees and uses (beyond signing in and the settings shell) is a plugin, even when nearly every space wants it. When in doubt, it is a plugin. The test:
   could a space of some type reasonably run without it? Then it is a plugin.
   - **The kernel offers services; plugins offer features.** Where a feature needs something every plugin shares,
     the shared part is a kernel service and the feature is a plugin: the search index is the kernel's and the
     search page a plugin; who belongs to a space is the kernel's and the Members page a plugin; the lists of what
     is waiting (review, feedback, the waiting list) are the kernel's and the admin's daily email a plugin.
   - **Settings are a sign.** Something with its own settings and screens is almost always a plugin.
   - **No space must have admins.** The kernel always knows who may change what (that is security), but a space type
     may give every member every role, as a peer group would. Admin features are plugins.
6. **Required plugins are still plugins.** A space type lists each plugin as **required** (on, and its admins cannot
   turn it off), **on by default** or **off by default**. A plugin a space type cannot carry its name without is
   required there, not moved into the kernel; another space type may leave it out. No plugin is required everywhere:
   if every space truly needs it, it is a kernel service.
7. **The public record only grows.** Identifiers already on the wire (`civic.vote`, activity and process addresses)
   never change. Wire changes only add fields; readers ignore fields they don't know. A breaking change gets a new
   version and the old form is served through a support window.
8. **Versioned surfaces.** The plugin SDK and each kernel service carry a semver version. 1.x to 1.y is always safe;
   2.0 breaks something, is announced, and the old surface keeps working through the support window.
