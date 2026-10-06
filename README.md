# path

The local path source plugin for daukle. It reads a producer manifest from a directory inside the consuming project, which is how a multi-project repository resolves its own modules without a network.

## What this plugin is

A `daukle.source` named `path`. A source answers one question, **where does a producer's manifest
come from**, and this one answers it with a directory inside the project you are already building.
It reads `<path>/daukle.toml` and nothing else.

It is the simplest acquisition in daukle: no network, no digest, no cache, because a file already in
your checkout is already as pinned as the checkout is.

## When you want it

**A producer and a consumer in one repository.** A multi-project repository resolves its own modules
without reaching anywhere, which is what every other source exists to do across a boundary this one
does not have to cross.

**Any test or example that would otherwise need a network.** It is the source to reach for when the
thing you want to prove is about consumers rather than about fetching, and most fixtures in this
organization use it for exactly that.

Reach for `daukle/github` instead when the producer publishes its manifest as a release asset.

## It cannot read outside the project

A `path` that escapes the project fails and names the file. That containment is not this plugin's
own check: it is the rule `daukle.include` has always had, enforced by the sandbox's path
resolution, so there is one place that decides what a project may read rather than one per plugin.

## Where the rest is

The keys, the exact failure modes and the reasoning live in this repository's `wiki/index.md`,
which is rendered at <https://daukle.github.io/guide/>. `AUTHORING.md` is the measured detail for
anyone changing the plugin.

## License

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
