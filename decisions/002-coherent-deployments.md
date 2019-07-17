# ADR-002: Coherent application deployments

## Status

Proposed

## Context

In the Legal Aid Agency (LAA), we have many older applications.
These applications currently follow **one or more** of the following practices:

### Releases are patches

Most of these applications have one or more of the following:

- Release scripts: scripts that we execute during the release. These files refer to database code files (Oracle PL/SQL (`.pks`, `.pkb`),
  Oracle Loader DaTa (`.ldt`), etc.) in the repository.
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

:raising_hand: **Note**: Please see the definition of "changes" below.

We propose the following checklist:

| Item | Item reversed |
| --- | --- |
| Outside development environments, changes can **only** be applied by putting them through version control. | Changes **cannot** be made to non-development environments without version control updates. |
| [wip] to capture "deployment action outside the servers"<br/>Outside development environments, changes can be applied **without** the applying user logging into the servers. | Users **cannot** log in to application servers or databases to apply changes. |
| [wip] to capture "git sha -> build -> release"<br/>Outside development environments, changes can **only** be applied from a [thing] associated with a git commit. | **Every** change can be tied back to a git commit. |

### Defining "changes"

"Changes" typically include:

- Changes to source code resulting in application behaviour change, for example, Java, Ruby, PL/SQL, HTML, CSS and so on.
- Changes to categorical data which the users cannot change, for example, a list of allowed codes.
- Changes to schemas or structures, for example, database schema migrations for relational databases.

"Changes" do *not* typically include:

- Corrections to user-created production data.
- Additions to production data that could be created by users.

For practical purposes, we recommend treating the different types of changes as different work.
The steps to apply Java changes are different than applying PL/SQL changes.

### Assessment

Teams should

- assess what kinds of "changes" do they have for an application;
- assess where they are on this checklist;
- and collaborate with their product owner and operations team to create a way forward.

### Related literature

- [Build, release, run (12factor.net)](https://12factor.net/build-release-run)
  > The twelve-factor app uses strict separation between the build, release, and run stages.
  >
  > For example, it is impossible to make changes to the code at runtime, since there is no way
  > to propagate those changes back to the build stage.
- [DevOps Culture (martinfowler.com)](https://martinfowler.com/bliki/DevOpsCulture.html)
  > If a development team shares the responsibility of looking after a system over the course of its lifetime,
  > they are able to share the operations staff’s pain and so identify ways to simplify deployment and maintenance
- [Continuous Delivery (martinfowler.com)](https://martinfowler.com/bliki/ContinuousDelivery.html)
  > You’re doing continuous delivery when [...] you can perform push-button deployments of any version of the
  > software to any environment on demand
- [Evolutionary Database Design (martinfowler.com)](https://martinfowler.com/articles/evodb.html)
  > Developers continuously integrate database changes
  >
  > [...] The steps above are just about treating the database code as another piece of source code.
  > As such the database code - DDL, DML, Data, views, triggers, stored procedures - is kept under
  > configuration management in the same way as the source code.

## Consequences

TBC