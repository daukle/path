# Authoring notes

`plugin.lua` is the whole plugin. It is published as a release asset and acquired by a `[plugins]`
table entry naming `daukle/path@<range>`.

## What this plugin owns

Resolving a producer manifest from a directory inside the consuming project. A source block needs
`path`, and the manifest is read from `<path>/daukle.toml`.

**It cannot read outside the project directory**, and that containment is deliberate rather than
incidental: it is the same rule `daukle.include` has always had, enforced by the sandbox's path
resolution rather than by this plugin. A `path` escaping the project fails naming the file.
## Conventions this repository is held to

These are set here because they are cheap to set at publication and expensive to change
afterwards. They apply to all five extracted plugins.

**An optional key with the wrong type.** Whether it raises is a per-plugin decision, not a rule:
`daukle/github` raises on a non-string `asset` or `tag`, `daukle/c` does not raise on its optional
`sha256`, and both are faithful to the C they replaced. Each repository states its own answer
rather than leaving the reader to infer one.

**Every error carries a `plugin.lua:<line>:` prefix.** Lua's `error()` adds it and the C this
replaced never had it. A test asserting on a message must assert on a clause, never on a token that
could also appear in the file path the message echoes.

## Tests

`test/run.sh` runs every directory under `test/cases/` against a real daukle, because this
plugin's output is a host verb's formatting and a stub of that verb would be testing the stub.

- a case with `expected/` must sync cleanly and match every file in it, byte for byte
- a case with `expect-error.txt` must fail with a message carrying that clause
- every success case is synced **twice** and must match after both, so applying twice equals
  applying once for every case rather than only the one that remembered to say so
- a case whose `expected/` is empty is a failure, not a pass

```sh
DAUKLE=/path/to/daukle sh test/run.sh
```

With no `DAUKLE`, the runner looks for a build under `.daukle/`, which is where CI checks
`daukle/daukle` out.

`.gitattributes` pins `* -text`, and it is load bearing rather than tidy. daukle writes LF on every
platform, so a checkout under `core.autocrlf=true` rewrites the fixtures and the byte-exact cases
fail on Windows for a reason that has nothing to do with the plugin. Measured on Windows, not
assumed: removing the file and re-checking out reproduces the failures.

## Provenance

`source_path.c` had no dedicated test file in `daukle/daukle`; it was covered through the
end-to-end suite. These cases were therefore authored here rather than recovered, and they are the
first tests this behaviour has had of its own.

Because this is a source plugin, its output is only observable through a language. The cases use a
minimal `plaintext` fixture language that prints `<project> <module>` per resolved entry, which is
deliberately not a copy of any of the four real language plugins.
