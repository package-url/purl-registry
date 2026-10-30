---
status: proposed
date: 2026-10-30
---

# Design for PURL registry

## Context / Problem

One of the biggest challenges for universal PURL adoption is to define
PURLs for software packages that do not reside in a structured ecosystem
defined by a public package manager or package repository (aka registry) that
can provide unique identifiers and metadata for all of its packages.

This challenge applies in particular to two large software domains:
- C/C++ and other standalone open source projects, such as the Linux Kernel,
  some packages of the Apache Software Foundation, and a significant set of
  essential software that powers the modern internet
- Commercial or proprietary software including internal use only software

### New 'software-id' PURL type

The Package-URL community is creating a new 'software-id' PURL `type` as the
primary structure to address this challenge. It is documented in the
`purl-spec` decision document: [001-Register-software-id-PURL-type](
https://github.com/package-url/purl-spec/blob/main/docs/decisions/001-Register-software-id-PURL-type.md). The decision document includes links to key background discussions and
issues for the 'software-id' PURL `type`.

The key unique feature of the 'software-id' PURL `type` is the definition
of the `namespace` PURL component. This `namespace` definition has two key
elements:
- *Disambiguation through domain*: For 'software-id' PURLs the first segment
  of the `namespace` component is always an internet domain. Different
  software publishers will always be able to provide unique PURLs for their
  software packages using their domain.
- *Flexible namespace segments*: The PURL `namespace` component supports
   multiple slash-separated segments (with no specific limit) so that a
   software publisher who owns a domain used for 'software-id' PURLs can
   document deep software naming hierarchies as needed.

See the **001-Register-software-id-PURL-type** decision document (link above)
for more details.

### PURL registries for the 'software-id' PURL type

The purpose of implementing PURL registries is to support the use of the new
'software-id' PURL `type` by providing a way for:
- Software publishers to register their usage of 'software-id' PURLs
  to make it easier for software consumers to discover their PURL data.
- An organization to host a 'software-id' domain for software publishers
  who want to use PURL but do not have their own domain (or do not want to use
  it for this purpose).

## Requirements

The key high level requirements for the PURL registry are:
- Neutral public registry
- Federated approach with `packageurl.org` as the default registry
- Simple process to register a 'software-id' `namespace` domain
- A `.well-known/purl` URI contains the definitions and references for the
  PURLs defined at the corresponding domain
- All data is keyed by PURL
- Optional storage of package metadata
- Neutral towards any build system or software ecosystem
- Support for extending the PURL registry to other PURL `types`


## Proposed solution design

This design document provide a proposed solution for implementing a
multi-level PURL registry system to support the new 'software-id' PURL `type`.

### Terminology
This design uses the following terminology:
- **Consumer** means means an organization (or person) that consumes
  software using PURLs to identify software packages.
- **Local PURL Registry** means a registry of software package PURLs that is
  maintained by an organization (or possibly a person) that asserts ownership
  and use of an internet domain as the first `namespace` segment for
  'software-id' `type` PURLs.
- **Package** means any software artefact. The most common synonym is
  component.
- **PURL Namespace Registry** means a registry of 'software-id' PURL
  `namespaces` maintained by the Package-URL community to document
  `namespaces` voluntarily registered by software Publishers.
- **Publisher** means an organization (or person) that publishes
  (or distributes) software using PURLs to identify software packages.
- `./well-known/purl` means a [**well-known URI**](https://en.wikipedia.org/wiki/Well-known_URI)
  that is required to be available at each Local PURL Registry. The
  Package-URL community will register the `purl` URI with IANA after we
  complete the design for the minimum data to be provided at this URI and
  validate it with a reasonable number of implementations.

This terminology is subject to change as the design evolves.

The proposed PURL registry solution has three levels:

### Local PURL Registries

A Local PURL Registry is required to provide:
- A `./well-known/purl` URI at the domain used for the first `namespace`
  segment of the PURLs maintained by the publisher.
- A public text file (catalog) listing the publisher's known PURLs with one
  package listed per line (with a newline `\n` ending). This format should be
  optimal for data streaming.

The reason for this requirement is to enable the public to verify the
validity of a 'software-id' PURL from this publisher. This is essential
information for software identification using PURL. Software publishers are
encouraged, but not required, to provide Package metadata or download
information (or both) for its PURLs.

It seems likely that many commercial publishers will limit access to
package metadata or downloads to customers. It also seems likely that large
organizations (commercial or open source) will implement their own internal
mechanisms to make package metadata or downloads available to consumers.

### PURL Namespace Registry

The PURL Namespace Registry is provided to allow a publisher to register its
domain for a 'software-id' PURL `namespace`. This registry will by provided
by the Package-URL community using a dedicated Git repository with:
- Simple YAML or similar files to document:
  - the domain to be registered for a 'software-id' `namespace`
  - description
  - homepage URL
  - contact information for the registration
- A simple issue & PR process to submit and approve registrations
- Basic verification that the requestor is authorized to register the
  `namespace` domain by the domain owner.
- A simple process to update a registration

Registration is optional but strongly encouraged to avoid conflicts and to
enable software consumers to quickly identify active Local PURL Registries.
The registration process is intended to be similar to the process for
registering internet domains.

### Package-URL hosted registry

The Package-URL community will host a shared "local" PURL registry
providing a free central registry for open source projects or other
organizations that want to standardize their PURLs without using their own
domain because they do not own a domain or they have one and prefer not to
use it for this purpose.

PURLs from the Package-URL hosted registry will all start with
`pkg:software-id/packageurl.org/` where 'software-id' is the PURL `type` and
'packageurl.org' is the first `namespace` segment. The Package-URL community
will define:
- A PURL `type` subschema for:
  - identifying each unique publisher within the `namespace` component, and
  - optionally defining rules for the other PURL component data for a
    publisher.
- The data structure for the information to be stored at and served from its
  `./well-known/purl` URI and offer this structure as a template for use by
  other Local PURL Registries.
- A Git-based process for registering each software publisher who wants to use
  the Package-URL hosted registry. This process will be similar to the
  registration process for Local PURL Registries.

Other organizations will have the option to establish a Local PURL Registry
to host multiple software publishers with the general template provided by the
Package-URL community.

## Non-functional requirements

- Persistent domain for the PURL Namespace Registry
- Scalability and security for public access: Support high demand for data
  from `packageurl.org` while avoiding abuse from APIs or agents. This will
  likely require Cloudflare or another CDN.

## Alternative solutions


## More Information

### Out of scope
- Provide an open-source container for self-hosting a lightweight self-service
  web app with a database.
- Aggregate mirrored snapshots of `./well-known/purl` PURL data from public
  Local PURL Registries.

### ASF example
An example of a 'software-id' PURL for Apache Software Foundation projects is:
`pkg:software-id/apache.org/<asf-namespace>/<name>@<version>`

Where:
- The `namespace` domain is fixed to `'apache.org'.
- <asf-namespace> is an ASF-internal segment (most likely the PMC/TLP, e.g.
  logging, commons) which we can finalize within the ASF.
- <name> and <version> follow the usual PURL semantics.

The two primary two software consumer-facing use cases are:
- looking up the correct PURL for a file shown on an ASF download page, and
- including that PURL in an SBOM so it can be matched against vulnerability
  data.

