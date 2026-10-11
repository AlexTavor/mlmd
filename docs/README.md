# How mlmd is built

mlmd is built with its own process ([process.md](../process.md)), by hand until the plugin exists.
This folder holds the project's documents as the process defines them, so every assumption, PoC
and decision is on the record.

- [assumptions.md](assumptions.md): what mlmd's design depends on that nobody has proved yet.
- [footguns.md](footguns.md): traps found on the way, where something looks like it does one
  thing and does another.
- [requirements.md](requirements.md): proposed requirements for the business side and adoption
  (roles, the decider, change requests, traceability), by Frank. Its status line names what
  still waits for Alex.
- [ux/](ux/): how mlmd feels to the operator: the first session (`initial-experience.md`) and the
  flow through a product (`flow.md`).
- [pocs/](pocs/): one document per PoC, with the question, what would be a no, where it ran,
  what was done, and the verdict. The scripts each PoC ran are in a folder next to its document.

The product is described so far in the repository's [README](../README.md) and in
[process.md](../process.md). mlmd's own Vision, behaviors and architecture documents haven't been
written yet.
