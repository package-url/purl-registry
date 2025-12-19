================================
       PURL registry
================================

This repository is a public, shared, open registry of Package-URLs for packages
that do not live in a structured ecosystem and therefore may not have a PURL.


The whole internet and most devices run on C/C++ code. There are many build and
packaging systems for C/C++ but no common registry making it difficult to
discover, catalog and identify C/C++ packages used in products, devices and
apps.

This problem also extends beyond just C and C++ packages to any package
in any programming languages that is not living in a structured ecosystem with
a package repository (aka. registry) that can provide unique names to all its
packages.

This project bootstraps a solution to resolve this:

- Use Package-URL (PURL) to create an open and distributed registry of C/C++
  packages keyed by PURL, with associated metadata, but neutral towards any
  build system.

- Promoted and maintain open source tools to discover and detect C/C++ code
  commonly vendored and patched in software codebases.

- Establish a database of known security vulnerabilities that affect these C/C++
  packages, also keyed by these PURL. This becomes even more imporant with
  CVE now supporting PURL since schema version 5.2.

The goal is to establish this registray as a community-maintained neutral place
to enable a distributed creation of unique PURLs with a simple process and
minimal overhead.

The C/C++ Package Registry should enable C/C++ software teams to more
efficiently and reliably manage and automate their C/C++ software supply chain
and vulnerability management operations, including tracking unambiguously
these packages in SBOMs, VEXs, tools, and vulnerability databases.

This in turn can help improve their security posture and regulatory security
compliance, using open data and open code. 

The roadmap is to first lay the foundations and design the essential data
formats, create the core code to collect and index C/C++ packages and build a
solid documentation to later foster and anchor a community.
