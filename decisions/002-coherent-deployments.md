# ADR-002: Coherent application deployments

## Status

Proposed

## Context

In the Legal Aid Agency (LAA), we have many older applications.
These applications currently follow **one or more** of the following practices:

### Symptoms of discontinuous deployments

#### Releases are patches

Most of these applications have one or more of the following:

- Release scripts: scripts that we execute during the release. These files, in turn, refer to database code files (Oracle PL/SQL (`.pks`, `.pkb`),
  Oracle Loader DaTa (`.ldt`) and others) in the repository.
- Release instructions: Word documents or plaintext instructions for the Database Administrator (DBA) on what to do during the release.

We add these for every release.

#### Copying as version control

We have files defining an entity (procedure, seed data) in version control that were duplicated and changed in the new file:

```
-rw-r--r--  1 user  staff   1.6M  <redacted>_phase1.sql
-rw-r--r--  1 user  staff   1.6M  <redacted>_Phase1.1.sql
-rw-r--r--  1 user  staff   1.6M  <redacted>_phase1.2.sql
-rw-r--r--  1 user  staff   1.6M  <redacted>_phase1.3.sql
-rw-r--r--  1 user  staff   1.7M  <redacted>_phase1.4.sql
-rw-r--r--  1 user  staff   1.7M  <redacted>_phase1.41.sql
-rw-r--r--  1 user  staff   1.9M  <redacted>_phase1.5.sql
-rw-r--r--  1 user  staff   2.0M  <redacted>_phase1.61.sql
-rw-r--r--  1 user  staff   1.9M  <redacted>_phase1.7.sql
-rw-r--r--  1 user  staff   1.9M  <redacted>_phase1.8.sql
```

These files are referred in separate installation scripts or release instructions.

It's sufficient to only change the original file as the history is retained as part of the version control system.

#### Inconsistent deployments

We sometimes skip releasing to a specific environment if it is unavailable, which leads to a drift of functionality between the environments.

#### Refresh process

To counter the drift of code and data, we periodically "refresh" the environments. In most cases, this means taking a
copy of **production** (configuration, code, data, indexes, views, triggers, functions, procedures and packages).

The copy of code (including database functions and procedures) is necessary to resolve the effect of long-term
[Releases are patches](#releases-are-patches), because we are unable to restore production functionality otherwise.

We anonymise the production data if necessary and make the data available on the target environment.

Sometimes we refresh from an older copy of production, which produces the same symptoms as
[Inconsistent deployments](#inconsistent-deployments).

#### Manual releases

We manually copy the release-related files to the target server. We do not use unique identifiers similar to a
git commit SHA to **match** the applied changes against the codebase.

### State awareness in environments

It is difficult to answer the following questions:

- What version of this application is currently running on this environment?
- What do I need to do to get to the latest `master` version of the code?
- What do I need to do to get to a specific version of the code?

## Proposal

We propose that each service **must** work towards satisfying the following checklist for **non-development** environments:

### Check 1: Version control

We can **only** apply [changes](#defining-changes) by putting them through version control.
&RightArrowLeftArrow; [Changes](#defining-changes) **cannot** be made without version control updates.

- More frequent use of version control resolves [Copying as version control](#copying-as-version-control).
- If everything is in git, it enables us to refer to versions of the application by the [git commit SHA][git-sha].

### Check 2: Automation

[Changes](#defining-changes) can be applied **without** the applying user logging into the application servers or databases.
&RightArrowLeftArrow; Users **cannot** log in application servers or databases to apply [changes](#defining-changes).

- We want to avoid the possibility to do manual changes as part of typical releases.
- Resolves [Manual releases](#manual-releases) by preventing manual deployments.
- Opens the possibility to resolve [Inconsistent deployments](#inconsistent-deployments) by enabling pipelines.

### Check 3: Coherence

[Changes](#defining-changes) are applied or rolled back together.
&RightArrowLeftArrow; It is impossible to deploy a version and have artefacts from earlier deployments still alive.

- Resolves [Releases are patches](#releases-are-patches) by treating code as a whole.
- Contributes to resolving [Inconsistent deployments](#inconsistent-deployments) by enabling pipelines.

### Defining "changes"

"Changes" typically include:

- Changes to source code resulting in application behaviour change, for example, Java, Ruby, PL/SQL, HTML, CSS, and similar.
- Changes to categorical data which the users cannot change, for example, a list of allowed codes.
- Changes to schemas or structures, for example, database schema migrations for relational databases.
    - :memo: See also: [ADR-042: Database pipeline][hosting-adr-42] in the [hosting architectural decisions][hosting-adrs].
- [Changes to runtime configuration](https://12factor.net/config), either secrets or public.
- [Changes to infrastructure][IaC].

"Changes" do *not* typically include:

- Corrections to user-created production data.
- Additions to production data that could be created by users.

### Example assessment

To measure the necessary changes required, teams could

- assess what kinds of "changes" do they have for an application;
- assess where they are on this checklist;
- collaborate with their product owner and operations team to create a way forward.

We created a [demo checklist spreadsheet](https://docs.google.com/spreadsheets/d/1oveCM853tk602N6-BYaxRgUV8eB2jfUnkQmaBYzbXog/edit?usp=sharing) as a starting point, in case it's useful.

### Related literature

- [Build, release, run (12factor.net)](https://12factor.net/build-release-run)
  > The twelve-factor app uses strict separation between the build, release, and run stages.
  >
  > For example, it is impossible to make changes to the code at runtime, since there is no way
  > to propagate those changes back to the build stage.

- [Release It! 2nd Edition](https://pragprog.com/book/mnee2/release-it-second-edition), Chapter 8, "Code"
  > [...] making sure that you know exactly what goes into the code on the instance.
  > It is vital to establish a strong “chain of custody” that stretches from the developer through to the
  > production instance. It must be impossible for an unauthorized party to sneak code into your system.

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

- [97 Things Every Software Architect Should Know](https://www.amazon.co.uk/Things-Every-Software-Architect-Should/dp/059652269X), Chapter 86, "Control the Data, Not Just the Code".
  > You need to be able to build the entire application, including the database, as one unit.
  > Make data and schema management a seamless part of your automated build and testing process early on and
  > include an undo button; it will pay large dividends.

## Consequences

- We only need to [refresh](#refresh-process) production-like **data**, if needed. We no longer need **code** refreshes.
- We can deploy our applications with automation tools, ensuring we can reliably release any time.
- We can answer the questions in [State awareness in environments](#state-awareness-in-environments), making it easier
  to prepare environments for testing or demos.
- We can deploy our applications on-demand, and we can expect the deployed behaviour to exactly match the revision's
  behaviour, leaving no surprises.

### Support

The Legal Aid Agency (LAA) architecture community will support applications moving to conform to this checklist by:

- Setting up a Slack channel, [`#ask-laa-architects`][support-channel], where people can discuss and debate and offer
  feedback.
- Joining the **hands-on** work as there _is_ a dedicated architect for every LAA service area.
  We will collate feedback and evolve this document.

[IaC]: https://www.thoughtworks.com/insights/blog/infrastructure-code-reason-smile
[hosting-adr-42]: https://github.com/ministryofjustice/laa-hosting-architectural-decisions/blob/master/doc/adr/0042-database-pipeline.md
[hosting-adrs]: https://github.com/ministryofjustice/laa-hosting-architectural-decisions
[git-sha]: https://git-scm.com/book/en/v1/Getting-Started-Git-Basics#Git-Has-Integrity
[support-channel]: https://mojdt.slack.com/messages/CM9JJ1E87
