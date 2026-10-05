# daukle/path

A **source**: it says where a producer manifest comes from. This one reads it from a directory
inside the consuming project.

## Declaring it

```toml
[plugins]
path = "daukle/path@^1"

[sources."example/greeter"]
kind = "path"
path = "./producer"
```

The manifest is read from `<path>/daukle.toml`.

## It cannot read outside the project

That containment is deliberate rather than incidental, and it is not this plugin's own check: it is
the same rule `daukle.include` has always had, enforced by the sandbox's path resolution. A `path`
escaping the project fails, naming the file.

## What it is for

A producer and a consumer in one repository, and every test fixture that would otherwise need a
network. It is the simplest source there is, which is why it is the one to reach for when you want
to prove something about consumers rather than about fetching.
