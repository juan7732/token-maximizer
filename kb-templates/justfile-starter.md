# Justfile Starter

**Category:** Template · copy into project root as `justfile`.

```just
set shell := ["bash", "-uc"]

default:
    @just --list

build:
    # build command

test:
    # test command

run:
    # run command

fmt:
    # formatter

lint:
    # linter

check: fmt lint test

ci: check build
```

Swap comments for language commands. Run `just` to list, `just check` before commit.
