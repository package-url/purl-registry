**Summary**
-----------

The internet runs on C/C++ software but there is no registry of C/C++
packages. We need a way to reliably discover, catalog and identify C/C++
software packages used in software products and applications using PURL.
The proposed solution is to:

-  Use Package-URL (PURL) to create an open and distributed registry of
   C/C++ packages keyed by PURL, with associated metadata, but neutral
   wrt. any build system,
-  Maintain open source tools to discover and detect C/C++ code commonly
   vendored and patched in software codebases,
-  And maintain a database of known security vulnerabilities that affect
   these C/C++ packages, also keyed by PURL.

Equipped with these, C/C++ software teams will be able to more
efficiently and reliably manage and automate their C/C++ software supply
chain and vulnerability management operations to improve their security
posture and regulatory security compliance, using open data and open
code.

**Context**
-----------

Most software development teams integrate ever more open source code in
their developments. A large number of software products, and hardware
products with digital content aka. “embedded devices”, rely on open
source software components written using the C and C++ programming
languages, therefore integrating in C/C++ compiled code in their
binaries. This includes the vast majority of core internet
infrastructure software like DNS and web servers, as well as program
interpreters, runtimes and compilers (like Python, Java, Ruby,
JavaScript, GCC and LLVM).

Package-URL (PURL) is the standard used to consistently identify
software packages across different products, systems, and applications.

PURL is a mostly universal software package identifier to know what is
in your code – critical for open source compliance, vulnerability and
security management, and software supply chain security and essential
for SBOMs, VEX, and overall cybersecurity, including upcoming
regulations like the European Cyber Resilience Act (CRA) and Digital
Operational Resilience Act (DORA), and the U.S. Executive Order 14028 –
across programming languages, package ecosystems, APIs, and
vulnerability databases.

PURL enables integration and interoperability between the different
tools and standards that use packages to manage modern software supply
chain integrity and security, like referencing packages across SPDX and
CycloneDX SBOM formats. PURL is in the process of Ecma standardization,
and considered for adoption by the CVE.org as an alternative and
complement to CPEs.

PURL is widely adopted across the entire software ecosystem, because
PURL’s simple syntax and definition facilitates better compliance
processes for end users. Most organizations worldwide are using PURL as
part of their software supply chain operations supported by SBOMS.

AboutCode originally created PURL to support software identification
across the AboutCode stack of open source tools like ScanCode,
VulnerableCode, and DejaCode; open data like PurlDB; and open SBOM
standards. AboutCode maintainers have fostered the adoption of PURL as
an open standard across the industry through their stewardship of the
PURL standard and core PURL libraries and anchoring a dynamic and
growing community of PURL adopters.

AboutCode maintainers and a community of contributors maintain the PURL
specification, PURL’s parsing and validation libraries for many
programming languages, and PURL-based open tools. This includes
AboutCode’s own open tools and databases for software supply security
and compliance automation, and tools from a growing community of PURL
users and integrators.

**The software supply chain management imperative**
---------------------------------------------------

Open source software origin, security and license compliance is
essential for all business operations that rely on software. But poor
data quality and incorrect identification can impede important
compliance processes – including discovering licenses and known
vulnerabilities in open source packages. This can negatively impact the
efficiency and productivity of all software, security, and legal teams
during software development, distribution, and deployment processes –
the entire software lifecycle.

Reliably discovering and identifying software components is a critical
requirement for regulatory compliance, like the European CRA, PLD, NIS2
and DORA regulations, the US NIST guidelines and FedRamp requirements.
Without accurate information about the origin of open source packages
integrated in software supply chains, automation is near impossible.
Automating these processes will be even more critical with these
upcoming regulations that require all software vendors, projects, and
other providers to share accurate SBOMs that correctly inventory the
software packages used in these products. Without inventory accuracy,
automation is impossible, and without automation, compliance is
impossible and cost prohibitive at any scale.

Exposure to security vulnerabilities is directly impacted by the
inability to efficiently discover and identify packages in vulnerability
databases. Components may be vulnerable but queries without correct
software identifiers will not identify the components as vulnerable. The
existing alternative requires extensive human expert intervention, which
is neither scalable, nor automatable.

**The problem with C/C++ packages**
-----------------------------------

The PURL standard consistently supports the decentralized identification
of software packages that are published in open component catalogs, like
Maven central, PyPI.org, and npmjs.org, but PURL could improve to
identify packages that are not part of these packaging ecosystems, or
lack a public catalog.

This is the case for components written in C and C++, two programming
languages widely used for embedded devices that are not part of a
packaging ecosystem. This causes several issues when consuming,
distributing, and deploying C/C++ code, tools, and libraries.

Compared to packaging ecosystems like PyPI for Python, npm for
JavaScript, Maven for Java, and NuGet for .NET, managing C/C++ packages
in the software supply chain is harder because open source C/C++
packages:

-  Do not have an obvious and easily inferred PURL global software
   identifier;
-  Do not have package manifests and structured metadata; and
-  Are scattered across multiple different and unmanaged internet
   locations, instead of a single, centralized or coordinated packaging
   ecosystem.

Without a well-defined software identifier for C/C++ packages,
automating software supply chain security is near impossible, because:

-  Tracking C/C++ packages in SBOMs, VEXs, and dependencies requires
   extensive manual work;
-  Collecting C/C++ package metadata cannot be automated without
   structured manifests; and
-  Finding known vulnerabilities for C/C++ packages in security
   databases is difficult without an obvious and accurate key to query
   these databases.

In addition, it is difficult to discover the C/C++ packages used in
product codebases because:

-  The code is commonly copied and vendored in code trees that also
   contain proprietary code, making third-party, vendored code code
   difficult to distinguish from proprietary code; and
-  The C/C++ code is commonly modified (albeit minimally) to apply
   patches and adjustments for integration in the build environment, or
   to adapt to a specific architecture or embedded device hardware.
   These modifications, however small, make discovering and matching
   third-party C/C++ code to known checksums using cryptographic
   checksums (such as SHA1) difficult, because these checksums will not
   be found exactly in modified code, even if minimally modified.

These problems represent a major threat to software supply chain
integrity and security (and regulatory compliance) at all software
development teams that rely on C and C++, for high performance, system
and embedded device programming.

Existing approaches to general purpose C/C++ package management, such as
with Chocolatey for Windows, or conan for C packages, attempt to provide
a complete solution from identification to distribution, including
building code and binary publication. But these approaches do not offer
a universal identification system that is build-neutral and do not help
with package discovery - they only provide package identification within
a specific toolchain and build process.

Another set of approaches is with Linux distributions, either generic
like Debian or Fedora, or for a special purpose like Android, OpenWRT,
or Yocto/OpenEmbedded. They do not provide a general purpose solution to
C/C++ package identifications. Instead, their approach is limited to the
confines of the distribution packaging and build processes and may not
easily be adopted for the common case of C/C++ package identification,
in particular when the code may not be designed to run on these
distributions.

**A solution for C/C++ packages**
---------------------------------

We need a solution, based on open data and open source tools, to improve
PURL identification and discovery of C/C++ components. The high level
outcomes would be to:

1. Propose a new PURL type to identify C/C++ packages. The high-level
   results are:

-  Propose a new “cpp” PURL type, submitted for addition to the PURL
   specification;
-  Update the PURL test suite for this C/C++ package type; and
-  Update the PURL Python library to support this type.

2. Create an open reference catalog of (most all) C/C++ packages, keyed
   by the cpp PURL type. The ultimate goal is to become the reference
   for C/C++ packages by providing the missing catalog for C/C++
   packages. The code for this catalog will utilize PurlDB and related
   AboutCode tools, extending existing basic support for mining C/C++
   packages used in embedded systems. The data will initially be based
   on the most popular packages, and the most common packages in use at
   initial contributors, and can later include additional C/C++ packages
   collected from open data sources, or submitted by the community. This
   catalog will be centralized to ensure that cpp PURLs are uniquely and
   consistently named, and easy to share for federated and decentralized
   usage. The high-level results for this include new code and data:

-  Collect, index, and store C/C++ package data and fingerprints, in
   async and on-demand modes:
-  Create code in PurlDB in four package batches, based on a list of
   C/C++ popular and common packages, and
-  Create code to collect the popular upstream C/C++ packages from
   Debian and Fedora Linux distributions to further extend the catalog;
-  Populate the PurlDB for these packages by running this data mining
   code;
-  Review and QA the collected PurlDB data for accuracy to inform code
   updates;
-  Create public Git repositories to store PURLs for C/C++ packages, and
   the tools to populate these from the PurlDB and import its data in a
   local PurlDB instance. This will be based on the FederatedCode
   decentralized design, and will enable on-premises private usage at
   any organization with an easy replication;
-  Release the collected data in the public PurlDB and FederatedCode
   system; and
-  Create documentation and guide for these items.

3. Enable content-based discovery and identification of C/C++ packages.
   The outcome is to enable finding existing C/C++ packages when
   scanning a product codebase with either:

-  Code matching enabled by the PURL catalog matching index; or
-  Code matching enables the content-based discovery of C/C++ packages
   used in a codebase by looking up fingerprints in an index of known
   C/C++ package fingerrints.
-  For this, we will deploy and document how to use locally or remotely
   the AboutCode code matching features to detect the presence of code
   exactly and approximately using fingerprints when scanning a
   codebase. We will also continuously update and release an index of
   all the C/C++ packages available in the registry for easy deployment
   of local code matching.
-  Structured or semi-structured package metadata.
-  Existing package ecosystems like pypi, npm, or RPM have structured
   metadata manifests that enable fast package discovery by seeking and
   parsing the metadata files. There is no such thing for C/C++.
-  For this project, we will design a simple package manifest file
   format to store the essential metadata that describe a package and
   promote its usage upstream so that it is available at the root,
   together with the code of A package. These metadata files will also
   be available as the core format for the C/C++ package registry that
   can then evolve to be just a convenient aggregator of the metadata
   provided directly by each package in the future.

The high-level results include evolving AboutCode tools:

-  Update ScanCode Toolkit to collect semi-structured C/C++ package
   metadata;
-  Create ScanCode.io pipeline for C/C++ codebase analysis, including
   metadata collection and code matching;
-  Update MatchCode matching pipeline for C/C++ support using the PurlDB
   catalog; and
-  Create documentation and guide for these items.

4. Extend ABOUT file format to serve as the “missing” metadata manifest
   for C/C++ packages. ABOUT files are simple YAML data files to
   document origin, license and other metadata about a software package.
   C/C++ do not have a common, shared format for metadata. The outcome
   will be to enable support for C/C++ packages in ABOUT files. ABOUT
   file is a simple format to store metadata, nothing more than a YAML
   file with simple conventions. See the section“ABOUT file metadata
   format” for details and examples. The high-level results include:

-  Document key fields, and usage; update AboutCode Toolkit with latest
   PURL support.
-  Add capability to export ABOUT file export from PurlDB.

5. Create an open mapping between CVEs and C/C++ PURLs to enable known
   vulnerability lookups using C/C++ PURLs in the VulnerableCode
   vulnerability database. Note that CVE.org is considering adopting
   PURLs as a key identifier. The high-level results include:

-  Update the VulnerableCode open vulnerability database to ensure CVEs
   and security advisories are collected for the tracked C/C++ packages
   and assigned the defined PURLs; and
-  Create a mapping to improve automation, and/or create data improvers
   to support the correct creation of relationships between
   vulnerabilities and C/C++ PURLs.

Beyond these initial steps, we are planning for future steps to maintain
an up-to-date, evergreen reference catalog of PURLs for C/C++ packages:

-  Engage the open source and vulnerability management communities and
   other key actors in the industry struggling with software
   identification to support the long-term sustainability of these
   efforts;
-  Reach out to key open source projects to “own” the PURL definitions
   for their C/C++ projects; and
-  Reach out to upstream CNAs and CVE.org to add the affected cpp PURLs
   to their security advisories and vulnerability databases to support
   their usage for SBOMs.

**Benefits**
~~~~~~~~~~~~

PURLs and SBOMs are essential to software supply chain management
automation and compliance.

This solution directly resolves a significant pain point for C/C++
development teams and these are the backbone of all core software built
and deployed today, from Linux, to JavaScript and the internet
infrastructure and core business and database systems, as well as most
embedded devices. Enabling more accurate software identification with
PURL for C/C++ packages, including in CycloneDX SBOMs that teams share
and receive, will directly improve these software team software supply
chain integrity and security and enable compliance automation processes.
Combining open source tools and open data extends these benefits to all
constituents in the larger software ecosystem.

Additional benefits of this project are:

1) Work with the creators of PURL.

AboutCode maintainers created PURL to resolve challenges identifying
software across different open source tools for Software Composition
Analysis (SCA) and vulnerability management. As the creators of PURL,
AboutCode maintainers uniquely qualified to lead the definition of a new
PURL type for C/C++, resolve issues related to PURL, and share these
with the larger community to benefit every usage of SBOMs and PURLs, as
well as the ecosystem that will transact SBOMs with C/C++ PURLs .

2) Ensure SBOM and PURL accuracy for C/C++ packages.

PURL correctness is essential to enable the safe reuse of FOSS. With a
correct PURL, users can find the correct and timely data related to
known vulnerabilities, end-of-life packages, and other critical software
supply chain data to resolve problems. A well-defined software
identifier for C/C++ packages will ensure that packages are discoverable
and identified correctly, and that known vulnerabilities are found for
the correct package.

Proposing this identifier for standardization with the community
benefits the larger ecosystem that experiences similar problems with
PURLs for C/C++, and will establish an example for better software
supply chain practices.

3) Automate compliance processes for software supply chain security and
   integrity with C/C++ packages .

Regulatory requirements like NIST, PLD, CRA and DORA compel any
organization consuming, distributing, or deploying software to generate
and share SBOMs for compliance; and share vulnerability exploitability.
Automation is necessary to manage the increased influx in SBOMs
generated and provided to software creators, vendors, and users. But
without accurate software identification, automated compliance processes
are impossible.

Any tool, process, or organization that needs to produce or read SBOMs
and VEXs does use PURL, be it with CycloneDX, SPDX, CSAF, or OpenVEX. A
well-defined software identifier for C/C++ packages in PURL ensures that
SBOMs and other regulatory documents are correct across tools and
standards used to generate the documents; facilitates efficient
compliance process automation; and improves the overall cybersecurity
posture of adopters and their associated ecosystem actors.

4) Streamline ecosystem-wide software supply chain with open catalog for
   C/C++ packages.

An open reference catalog of C/C++ packages enables ecosystem-wide,
shared and accurate identification, and improves the efficiency of
software supply chain automation for these packages. This directly
benefits adopters, ensuring global and accurate C/C++ packages
identification in SBOMs, VEXs, and vulnerability databases.

And as a public catalog, these benefits extend to the C/C++ community,
and improve their overall security posture.

5) 100% open source and open data solution.

Everything described here is free and open source code with open data
supported by open standards and contributed to AboutCode and other
projects. Components of the AboutCode stack are already in use
throughout the industry, including ScanCode and PURL.

**Proposal for C/C++ PURL Pilot**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Under this plan, the goals are to:

1. Design and propose a PURL type for C/C++ packages using PURL, as part
   of the PURL specification.
2. Create an open reference catalog of C/C++ packages, keyed by the cpp
   PURL type, focusing on common packages.
3. Enable content-based discovery and identification of C/C++ packages,
   supported by the PURL catalog.
4. Extend the ABOUT file data format as the “missing” metadata manifest
   for C/C++ packages.
5. Create an open mapping between CVEs and C/C++ PURLs to enable known
   vulnerability lookups using C/C++ PURLs in the vulnerability
   database.

The goal of this project is to confirm the feasibility of our proposed
approach for using PURL to identify C/C++ software packages and their
vulnerabilities using functional and tested extensions to the AboutCode
stack code and data.

If this pilot proposal is successful, there will be significant efforts
and resources required to operate and sustain this open C/C++ catalog in
the future, but this will be using a strong base.

[1] https://nexb.com/purl-universal-software-package-identification/

[2] https://github.com/package-url/purl-spec/

[3] https://ecma-international.org/task-groups/tc54-tg2/
