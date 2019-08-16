# ADR-002: Use in-support Oracle DB versions

## Status

Accepted

## Context

Many of the applications we support use Oracle as a database.

Oracle provides three levels of support: [premier support, extended support, and sustaining support](https://www.oracle.com/support/lifetime-support/resources.html). Of these, "sustaining support" is not viable because it does not provide new fixes, updates, and critical patch updates, and it is not certified to work with new versions of other Oracle products. Extended support allows customers to retain support for older versions after the premier support ends.

Customers on supported versions are still expected to apply patch level updates from Oracle - this is one of the first things they will ask you when making a support call. Patches are typically released quarterly.

The app modernisation team will update the version of the database used by the Client and Cost Management System (CCMS). This will affect multiple applications within the CCMS system, including:

* The temporary data store used by the [Provider User Interface and Connector](https://github.com/ministryofjustice/laa-ccms-pui)
* The underlying databases backing E-Business suite, SOA, OPA which are all Oracle products we host

At the time of writing, CCMS uses 11.2, which is on extended support. This support will run out next year.

There is a later version available, 12.1. This is also no longer in premier support, but has extended support available until July 2021. The extra cost of extended support is waived for customers who use E-Business Suite (which we do).

As part of the CCMS modernisation project we are provisioning new infrastructure in AWS, which will include Oracle databases.

We would recommend that other teams using Oracle databases also upgrade to this version.

## Decision

- We will move to version 12.1 of the database, which will keep us in support until July 2021
- We will upgrade to a later version before that deadline, and will continue to upgrade major versions as the versions we're using become unsupported
- We will apply patches quarterly, no more than one quarter after the patch was released
- We will subscribe to [critical patch update notifications from Oracle](https://www.oracle.com/technetwork/topics/security/alerts-086861.html)

## Consequences
We remain within a supported version of Oracle, so we can call the vendor if something goes wrong.

We can keep pace with regular updates from Oracle.

We can respond more quickly to security alerts and critical patches from Oracle.
