# stack-inconsistent-deps

This repo is a minimal proof-of-concept of a Stack error we came across.

## Problem

The symptoms:

```
$ stack build --only-dependencies
Progress 0/2
Inconsistent dependencies were discovered while executing your build plan. This should never happen, please report it as a bug to the stack team.
```

Our actual project is huge, but the general gist (as documented in this repo)
is that we have a package that we import in all of our custom `Setup.hs` files:

```
foo
  - library needs bar
bar
  - Setup.hs needs custom-setup-pkg
custom-setup-pkg
  - library needs prelude-pkg
prelude-pkg
```

This setup works when all of the packages are in the `packages` list in
`stack.yaml`, but the error came about when `bar` is put into `extra-deps`.

## Reproduce

1. Clone this repo
1. `stack build --only-dependencies`
