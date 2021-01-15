# [ARCHIVED] Legal Aid Agency Architecture Documentation

The documentation here uses the [C4 model][c4model] by [Simon Brown][simonbrown].
We use the [FC4 toolset][fc4-toolset] to normalise the source of C4 diagrams authored with [Structurizr Express][se].

## Archived status

As of 2021 this repository is no longer the source of truth.  For the latest information
you should look at the replacement repository, [laa-architecture-as-code](https://github.com/ministryofjustice/laa-architecture-as-code).

As a result of the above, this repository has been archived and marked as read only.

## Editing
* Install and setup the fc4 tool https://fundingcircle.github.io/fc4-framework/tool/#setup

*The following helped in installing the java runtime required for this tool.*
```
brew update
brew tap homebrew/cask-versions
brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk11-jre
```

* Follow [this workflow](https://fundingcircle.github.io/fc4-framework/methodology/authoring_workflow.html#optional-using-structurizr-express-for-graphical-editing) to make changes to the yaml files and to generate images

## Requirements

Image files in this repository are tracked through [Git Large File Storage][git-lfs].
Please install it by following the [site instructions][git-lfs].

## Structure

Legal Aid Agency is divided into the following service areas, with their respective locations in the repository:

| Service area | Location | Responsibility |
| ------------ | -------- | -------------- |
| Outside Legal Aid | [`diagrams/outside/`](diagrams/outside/) | Diagrams that describe systems used by us but owned outside the Legal Aid Agency.
| Get Access to Legal Aid | [`diagrams/get-access/`](diagrams/get-access/) | Providing members of the public with information about what is "legal aid" and how to apply for it.<br/>Contracting with legal advisors to provide legal aid for members of the public. |
| Get Legal Aid | [`diagrams/get-legal-aid/`](diagrams/get-legal-aid/) |  |
| Get Paid for Legal Aid | [`diagrams/get-paid/`](diagrams/get-paid/) |  |

## Conventions

### Colours

- Legal Aid Agency software systems, people and elements use the ["Ministry of Justice" websafe colour][moj-websafe] (`#5a5c92`).
- Software systems, people and elements outside the UK Government use `#28a197`.


[se]: https://structurizr.com/help/express
[git-lfs]: https://git-lfs.github.com/
[c4model]: https://c4model.com/
[simonbrown]: http://simonbrown.je/
[fc4-toolset]: https://fundingcircle.github.io/fc4-framework/methodology/toolset.html
[moj-websafe]: https://github.com/alphagov/govuk_frontend_toolkit/blob/03204449dcbf28c2d48e7f6dd217f6abe9b3a518/stylesheets/colours/_organisation.scss#L46
