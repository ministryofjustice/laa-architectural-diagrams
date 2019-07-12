# ADR-002: Atomic application deployments

## Status

Proposed

## Context

In the Legal Aid Agency (LAA), we have many older applications.
These applications currently follow **one or more** of the following practices:

### Releases are patches

Most of these applications have one or more of the following:

- Release scripts: scripts that we execute during the release. These files refer to code files in the repository.
- Release instructions: Word documents or plaintext instructions for the Database Administrator (DBA) on what to do during the release.

We add these for every release.

### Duplicated files

Sometimes files are duplicated and referred to individually in releases, following this pattern:

```
ebiz/db_install/release_01.ksh
# refers ebiz/aol/lookups/name_of_lookup.ldt

ebiz/db_install/release_02.ksh
# refers ebiz/aol/lookups/name_of_lookup_2.ldt
```

These files refer to the same entity, which makes it harder to see which one is the "current" version.

### Inconsistent deployments

We sometimes skip releasing to a specific environment if it is unavailable, which leads to a drift of functionality between the environments.

### Refresh process

To counter the drift of code and data, we periodically "refresh" the environments. In most cases, this means taking a
copy of **production**, anonymising it if necessary and restoring that on the target environment.

However, we sometimes refresh from an older copy of production, which produces the same symptoms as "inconsistent
deployments".

### Manual releases

We manually copy the release-related files to the target server. We do not use unique identifiers similar to a
git commit SHA to **match** the applied changes against the codebase.

### Summary

It is difficult to answer the following questions:

- What runs on the environment? (After refresh, were all patches applied?)
- What do I need to do to get to the latest `master` version of the code? (What patches do I need to apply?)
- What do I need to do to get to a specific version of the code? (What patches have to be rolled back?)

## Proposal

TBC; identify git sha for deployments; establish an easy way to roll forward

### Related literature

- Immutable architecture?
- [Build, release, run (12factor.net)](https://12factor.net/build-release-run)
- [Schema migration (wikipedia.org)](https://en.wikipedia.org/wiki/Schema_migration)
- [Evolutionary Database Design (martinfowler.com)](https://martinfowler.com/articles/evodb.html)

## Consequences

TBC
