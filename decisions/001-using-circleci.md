# ADR-001: Using CircleCI as a continuous integration service

## Status

Proposed

## Context

There are a couple of continuous integration (CI) services used by the Ministry of Justice (MoJ) Digital teams and the
Legal Aid Agency (LAA) Digital teams. The list includes Travis, Semaphore, Concourse, Circle, Jenkins.

Facts in favour of CircleCI:

* Many teams started using CircleCI to integrate with the new Cloud Platform service using workflows
  to implement approvals and deployment.
* CircleCI has grown in features and their support for custom job Docker images and "orbs" as
  reusable commands work well for the majority of our use cases.
* To support private projects, MoJ Digital has invested in private executors on CircleCI.

Given the above context, we feel that defaulting to CircleCI is a sensible decision as it has the following benefits:

* Contributing to applications across LAA Digital will not require learning another CI tool.
* Support from many teams over [`#circleci-users`][circleci-users-slack] Slack channel.
* Sharing similar code, for example, tagging and pushing Docker images to Elastic Container Registry (ECR) and deploying
  to Cloud Platform's Kubernetes platform.

The downsides are:

* A bit of a learning curve compared to inference-based CI tools.
* We might step on each others' toes if we expand our CircleCI usage, resulting in queues on builds and deployments.
  Needs to be monitored.

## Decision

The **default** choice of continuous integration service in the LAA Digital teams is
**[CircleCI](https://circleci.com)**.

## Consequences

**Existing** LAA repositories using other CI services may continue using them until a change is necessary.
New jobs and significant changes should migrate to CircleCI.

**New** LAA applications should start with CircleCI as a default choice unless there is a compelling reason otherwise.

[circleci-users-slack]: https://mojdt.slack.com/messages/CCSD5F397
