---
status: proposed
date: 2026-10-29
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
- **Disambiguation through domain**: For 'software-id' PURLs the first segment
  of the `namespace` component is always an internet domain. Different
  software publishers will always be able to provide unique PURLs for their
  software packages using their domain.
- **Flexible namespace segments**: The PURL `namespace` component supports
   multiple slash-separated segments (with no specific limit) so that a
   software publisher who owns a domain used for 'software-id' PURLs can
   document deep software naming hierarchies as needed.

See the **001-Register-software-id-PURL-type** decision document for more
details.

### New PURL registry

The purpose of implementing a PURL registry is to support the use of the new
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

The proposed PURL registry solution has two primary levels:

### PURL Namespace Registry

The central registry keeps track of all the orgs and prevents two real-world
orgs using the same identifier. This is conceptually similar to the domain
name system. First come, first served. We would need to record some contact
information, website etc. (more below) for each org in order to be able to do
housekeeping over the years. Also, some proof of association between contact
person and org should be required. Otherwise, it works via GitHub PRs, no
extra infrastructure needed.

exists only once, run by the PURL people)

- Where do we track author/owner, contact and license information
    ---> in GH as part of the registration process
  - privacy concerns
  - how often does the data change
  - license complexity

Each domain acts as a PURL registry that enforces uniqueness. The shared
packageurl.org registry enforces uniqueness within its domain during the
registration process, where each registration is documented by files published
at a `.well-known` URI.

The shared packageurl.org domain serving a .well-known/purl URI of registered
PURL namespaces and providing a centralized registry for open source projects
or other organizations that want to standardize their PURLs without having to
host their own .well-known/purl URI, either because they do not control a
domain or they prefer centralized registration.

The packageurl.org domain will host the shared registry of PURLs prefixed with
 pkg:software-id/packageurl.org/ and also serve as an aggregator for other
 domains to make discovery easier with mirrored snapshots of their
 `./well-known/purl` PURL definitions and catalogs.

The packageurl.org domain will support a simple process to register a
'software-id' PURL `namespace`. The data in this PURL registry will be stored
as JSON data files in a public git repository, and that data will be served
directly from `packageurl.org/.well-known/purl`. The git repository is at
https://github.com/package-url/purl-registry/


### Local registries

Some organizations produce a large number of components and therefore PURLs.
The local registry helps them keep track, and make sure they've got at least
the same metadata as the global registry, but for each of their local
components. This is potentially an optional thing - maybe some orgs don't need
 it, and we should not require a local registry?

From a PURL perspective, we don't need to do much here. Each org would be on
their own to manage their internal components. Maybe we'll have an open-source
container for self-hosting a little self-service web app with a database one
day. Maybe it's just a Git repo like the global registry. It's the org's decision.

Either way, the local registry must always produce an up-to-date text file
listing all their components -  one component per line. This is provided via
a download URL given in the global registry, so the public can verify the correctness of a PURL.

- a local registry (exists once per org, run by the org)

How much does the software-id type definition mandate the structure of data
available at ./well-known/purl URI
- At a minimum a list of valid PURLs?
- Store as text with defined line editing / best for stream parsing
- Internal redirects very likely inside commercial companies

- Define software-id "subschema" (examples at purl-registry)

### packageurl.org registry



## Non-functional requirements

- Persistent domain for the registry
- Scalability and security for public access: Support high demand for data from
  packageurl.org while avoiding abuse from APIs or agents. Will likely require
  Cloudfare or another CDN.


## Alternative solutions


## More Information

### ASF example
An example of a 'software-id' PURL for Apache Software Foundation projects is:
`pkg:software-id/apache.org/<asf-namespace>/<name>@<version>`

Where:
- The `namespace` domain is fixed to `apache.org`.
- <asf-namespace> is an ASF-internal segment (most likely the PMC/TLP, e.g.
  logging, commons) which we can finalize within the ASF.
- <name> and <version> follow the usual PURL semantics.

The two primary two software consumer-facing use cases are:
- looking up the correct PURL for a file shown on an ASF download page, and
- including that PURL in an SBOM so it can be matched against vulnerability
  data.

