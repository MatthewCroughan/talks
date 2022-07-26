---
author: Matthew Croughan
date: MMMM dd, YYYY
paging: Slide %d / %d
---

# Use flake.nix, not Dockerfile

Because builds via Dockerfiles are not reproducible, they are repeatable.

---

# What is Nix?

- Invented by Eelco Dolstra in his PhD thesis in 2003
  - The Purely Functional Software Deployment Model[^1]
  [1]: https://edolstra.github.io/pubs/phd-thesis.pdf
- Domain specific language (DSL) for specifying reproducible builds
- Lazily evaluated
- Purely Functional programming language (Not procedural, ordering doesn't matter)
- Declarative, rather than imperative
- Everything is an expression, no side effects

---

# A basic abstract example

```bash
cat ./examples/abstract.nix
```

---

# Let's evaluate x

```bash
nix eval -f ./examples/abstract.nix x
```

---

# Let's evaluate y


```bash
nix eval -f ./examples/abstract.nix y
```

---

# Let's evaluate z

```bash
nix eval -f ./examples/abstract.nix z
```

---

# A basic concrete example, compiling hello-world from GNU

```bash
cat ./examples/hello-world.nix
```

```bash
nix build -f ./examples/hello-world.nix
ls -lah result/bin
file result/bin/hello
result/bin/hello
```
---

# So why are Dockerfiles not reproducible?

```bash
cat Dockerfile
```
---

# A different result every time?

```bash
cat examples/hashdifference.txt
```

---

# So how do we do this with a Nix Flake instead?

There's a function someone made called

```bash
cat flake.nix
nix flake show
```

We can put any of the 80,000 packages from Nixpkgs we want in that `contents`
argument, or make our own derivation

---

# Let's build and run it

```bash
nix build .#my-container-image
ls -lah result
docker load < result
docker run my-container-image
```

---

# Why?
Docker is repeatable, but not reproducible. It defines instructions, but trusts
the internet unconditionally, without performing any hashing. Dockerfiles do not
provide you with a toolset for performing reproducible builds.

---

# What does Nix guarantee?

## Same inputs, same outputs! (Hopefully)

Nix guarantees that all of the inputs for a build will be gathered reproducibly,
and that the build process will be performed offline, in a sandbox, meaning the
output should be the same every time.

# What does Nix not guarantee?

## Sometimes, build processes are not deterministic despite everything

Nix cannot force the build process to be deterministic, but it does more than
most tools, by implementing a methodology for performing reproducible builds and
providing you with an expression language to define the build.

---

# Presenter, if you have the time please show...

  - Isolated python environments via python3.withPackages
  - Cross compilation for any architecture using pkgsCross
  - nix shell, try before you buy

# Questions?

