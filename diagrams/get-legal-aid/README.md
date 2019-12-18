# Get Legal Aid service area

## High-level diagrams

**Intended audience**: Everyone (technical and non-technical).

These diagrams aim to inform about the _users_ and _integrations_ around the system in focus. The drawings are intended
to support these questions:

- Who uses the system (humans, internal or external)
- What is the use-case for the system, what problem does it solve
- What integrations exist with other systems

| Diagram | Description |
| --- | --- |
| <img src="ccms-ebs-system-context.png" width="280"/> | Shows the context of CCMS (Client and Cost Management System and E-Business Suite. |
| <img src="criminal-legal-aid-application-system-landscape.png" width="280"/> | Shows the context of the criminal legal aid applications. |
| <img src="ccms-api-landscape.png" width="280"/> | Shows the context of the CCMS API. |
| <img src="court-data-adaptor-system-context.png" width="280"/> | Shows the context of the Court Data Adaptor. |

## Container diagrams

**Intended audience**: Service teams involved in developing the systems.

These diagrams support these questions:

- What are the moving parts of the system?
- Which part do users or other systems use?
- How does it all work together?

| Diagram | Description |
| --- | --- |
| <img src="ccms-ebs-containers.png" width="280"/> | Container diagram for CCMS, focusing on the [provider user interface](https://github.com/ministryofjustice/laa-ccms-pui). |
| <img src="ccms-api-containers.png" width="280"/> | Container diagram for [CCMS API](https://github.com/ministryofjustice/laa-ccms-provider-details-api). |

## Deployment diagrams

**Intended audience**: Technical people inside and outside of the software development team; including software architects, developers and operations/support staff.

These diagrams support these questions:

- Where is the system deployed?
- What infrastructure components do we have for the system?

| Diagram | Description |
| --- | --- |
| <img src="ccms-api-deployment.png" width="280"/> | Deployment diagram for [CCMS API](https://github.com/ministryofjustice/laa-ccms-provider-details-api). |

## Authoring doccuments

### C4

C4 models might benefit from using [Structurizr Express](https://structurizr.com/express) and [fc4](https://fundingcircle.github.io/fc4-framework/tool/)

```sh
$ brew tap fundingcircle/floss
$ brew install fc4
```

fc4 keeps the C4 YAML tidy.

### PlantUML

We're using Make to capture the commands

```sh
$ brew install plantuml
```
