# Identity

Identity is one of the kernel's three jobs, along with security and interoperability. Whatever method a person
uses to prove who they are, the kernel is what every plugin asks "who is this, and what may they do here?"

## What the kernel owns

- Sign-in and accounts. Today that means email codes, with a "keep me signed in" option.
- Who belongs to a space, and the roles each member holds (member, moderator, admin, accountable). A space type
  names these roles in its profile, but it cannot add new ones.
- The space's own DID with its keys, and members' DIDs.
- A place to keep credentials, and checking them: which credentials a space asks for, issuing them, and verifying
  them.
- Linking one person's accounts across spaces.

Ways to sign in, and kinds of credential, plug in as **identity plugins**. The kernel keeps the account, the roles
and the checks. A method is a plugin a space can turn on.

## Who builds what

Mosaic is not building the new identity work in house: bringing your own DID, sign-in through OIDC, and membership
credentials. It works with outside decentralized identity groups instead. These are **OpenVTC** (open verifiable trust
communities) and **VTI** (verifiable trust infrastructure), both part of the First Person Project.

That shapes the kernel's job here. It offers stable seams that this outside work can plug into: the identity plugin
kind, the credential store, and how a space states which credentials it asks for. Mosaic does not write the methods
itself. Sign-in and roles stay with the kernel as they are today, and new methods arrive as identity plugins.

Because nothing in house is in progress, the kernel's move to this repository does not wait on identity work.

The earlier design is in "Identity for Mosaic spaces: draft approach" (Nov 13, 2026). It names five phases: bring
your own DID, OIDC, membership credentials, a credential verification plugin, and an Open VTC plugin. Those phases now
proceed with these partners.
