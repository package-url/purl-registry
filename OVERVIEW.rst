**Summary**
-----------

The internet runs on C/C++ software but there is no registry of C/C++
packages. We need a way to reliably discover, catalog and identify C/C++
software packages used in software products and applications using PURL.
The proposed solution is to:

-  Use `Package-URL (PURL) <https://github.com/package-url/purl-spec>`_, to create an open and distributed registry of
   C/C++ packages keyed by PURL, with associated metadata, but neutral
   regarding any build system;
-  Maintain open source tools to discover and detect C/C++ code commonly
   vendored and patched in software codebases, and
-  Maintain a database of known security vulnerabilities that affect
   these C/C++ packages, also keyed by PURL.

With this data C/C++ software teams will be able to more
efficiently and reliably manage and automate their software supply
chain and vulnerability management operations to improve their security
posture and regulatory security compliance, using open data and open
code.

**Context**
-----------

Most software development teams integrate ever more open source code in
their developments. A large number of software products, and hardware
products with digital content aka. “embedded devices”, rely on open
source software components written using the C or C++ programming
languages, therefore integrating C/C++ compiled code in their
binaries. This includes the vast majority of core internet
infrastructure software like DNS and web servers, as well as program
interpreters, runtimes and compilers (like Python, Java, Ruby,
JavaScript, GCC and LLVM).

Package-URL (PURL) is an Ecma standard (ECMA-427) used 
to consistently identify software packages across different products, systems, 
and applications.

PURL is a mostly universal software package identifier to track what is
in your code – critical for open source compliance, vulnerability and
security management, and software supply chain security. PURL is essential
for SBOMs, VEX, and overall cybersecurity, including upcoming
regulations like the European Cyber Resilience Act (CRA), the Digital
Operational Resilience Act (DORA), and the U.S. Executive Order 14028 –
across programming languages, package ecosystems, APIs, and
vulnerability databases.

PURL enables integration and interoperability between different
tools and standards that use packages to manage modern software supply
chain integrity and security, like referencing packages across SPDX and
CycloneDX SBOM formats. PURL is an Ecma standard: `ECMA-427 <https://tc54.org/purl/>`_, 
and it has been adopted by the CVE.org as a software identifier to
complement CPEs.

PURL is widely adopted across the entire software ecosystem because
PURL’s simple syntax and definition facilitates better compliance
processes for end users. Most organizations worldwide are using PURL as
part of their software supply chain operations supported by SBOMs.

AboutCode originally created PURL to support software identification
across the AboutCode stack of open source tools like ScanCode,
VulnerableCode, and DejaCode; open data like PurlDB; and open SBOM
standards. AboutCode maintainers have fostered the adoption of PURL as
an open standard across the industry through their stewardship of the
PURL standard and core PURL libraries and anchoring a dynamic and
growing community of PURL adopters.

AboutCode maintainers and a community of contributors maintain the PURL
specification, PURL’s parsing and validation libraries for many
programming languages, and other PURL-based open tools. This includes
AboutCode’s own open tools and databases for software supply security
and compliance automation and tools from a growing community of PURL
users and integrators.

**The software supply chain management imperative**
---------------------------------------------------

Open source software origin, security and license compliance is
essential for all business operations that rely on software. Poor
data quality and incorrect identification can impede important
compliance processes – including discovering licenses and known
vulnerabilities in open source packages. This can negatively impact the
efficiency and productivity of software, security, and legal teams
during software development, distribution, and deployment processes –
the entire software lifecycle.

Reliably discovering and identifying software components is a critical
requirement for regulatory compliance, like the European CRA, PLD, NIS2
and DORA regulations, the US NIST guidelines and FedRamp requirements.
Without accurate information about the origin of open source packages
integrated in software supply chains, automation is practically impossible.
Automating these processes will be even more critical with the
upcoming regulations that require all software vendors, projects, and
other providers to share accurate SBOMs that correctly inventory the
software packages used in their products. Without inventory accuracy,
automation is impossible, and without automation, compliance is
cost prohibitive at any scale.

Exposure to security vulnerabilities is directly impacted by the
inability to efficiently discover and identify packages in vulnerability
databases. Components may be vulnerable but queries without correct
software identifiers will not identify the components as vulnerable. The
current alternatives requires extensive human expert intervention, which
is neither scalable nor automatable.

**The problem with C/C++ packages**
-----------------------------------

The PURL standard consistently supports the decentralized identification
of software packages that are published in open component catalogs, like
Maven central, PyPI.org, and npmjs.org, but PURL can be extended to
identify packages that are not part of these packaging ecosystems, or
lack a public catalog.

This is the case for components written in C or C++, two programming
languages widely used for embedded devices that are not part of a
packaging ecosystem. This causes several issues when consuming,
distributing, and deploying C/C++ code, tools, and libraries.

Compared to packaging ecosystems like PyPI for Python, npm for
JavaScript, Maven for Java, and NuGet for .NET, managing C/C++ packages
in the software supply chain is harder because open source C/C++
packages:

-  Do not have an obvious and easily inferred PURL global software
   identifier,
-  Do not have package manifests and structured metadata, and
-  Are scattered across multiple different and unmanaged internet
   locations, instead of a single, centralized or coordinated packaging
   ecosystem.

Without a well-defined software identifier for C/C++ packages,
automating software supply chain security is near impossible, because:

-  Tracking C/C++ packages in SBOMs, VEXs, and dependencies requires
   extensive manual work,
-  Collecting C/C++ package metadata cannot be automated without
   structured manifests, and
-  Finding known vulnerabilities for C/C++ packages in security
   databases is difficult without an obvious and accurate key to query
   these databases.

In addition, it is difficult to discover the C/C++ packages used in
product codebases because:

-  The code is commonly copied and vendored in code trees that also
   contain proprietary code, making third-party, vendored code
   difficult to distinguish from proprietary code;
-  The C/C++ code is commonly modified (albeit minimally) to apply
   patches and adjustments for integration in a build environment, or
   to adapt to a specific architecture or embedded device hardware.
   These modifications, however small, make discovering and matching
   third-party C/C++ code using cryptographic checksums (such as SHA1)
   difficult because these checksums will not be found exactly in 
   modified code, even if minimally modified.

These problems represent a major threat to software supply chain
integrity and security, and regulatory compliance for any software
development team that relies on C and C++ for high performance, system
and embedded device programming.

Existing approaches to general purpose C/C++ package management, such as
Chocolatey for Windows, or conan for C packages, attempt to provide
a complete solution from identification to distribution, including
building code and binary publication. But these approaches do not offer
a universal identification system that is build-neutral and do not help
with package discovery. They only provide package identification within
a specific toolchain and build process.

Linux distributions, either generic like Debian or Fedora, or special 
purpose distributions like Android, OpenWRT,or Yocto/OpenEmbedded offer
a different approach. They do not provide a general purpose solution to
C/C++ package identification. Their approach is limited to the
confines of their specific distribution packaging and build processes
and may not easily be adopted for the common case of C/C++ package 
identification, in particular when the code may not be designed to 
run on their distribution.

**A solution for C/C++ packages**
---------------------------------

We need a solution, based on open data and open source tools, to improve
PURL identification and discovery of C/C++ components. The high level
target outcomes are to:

1. Propose a new PURL type to identify C/C++ packages. The steps are:

   -  Propose a new “cpp” PURL type for registration as a standard PURL type,
   -  Update the PURL test suite for this C/C++ package type, and
   -  Update the PURL Python library to support this type.
|
2. Create an open reference catalog of (most all) C/C++ packages, keyed
   by the cpp PURL type. The goal is to become the reference identifier
   for C/C++ packages by providing the missing catalog for C/C++
   packages. The code for this catalog will utilize PurlDB and related
   AboutCode tools, extending existing basic support for mining C/C++
   packages used in embedded systems. The data will initially be based
   on the most popular packages. and the most common packages in use at
   initial contributors. It can later include additional C/C++ packages
   collected from open data sources or submitted by the community. This
   catalog will be centralized to ensure that cpp PURLs are uniquely and
   consistently named, and easy to share for federated and decentralized
   usage. The steps for this include new code and data:

   -  Collect, index, and store C/C++ package data and fingerprints, in
      async and on-demand modes,
   -  Create code in PurlDB in four package batches, based on a list of
      C/C++ popular and common packages,
   -  Create code to collect the popular upstream C/C++ packages from
      Debian and Fedora Linux distributions to further extend the catalog;
   -  Populate the PurlDB for these packages by running this data mining
      code,
   -  Review and QA the collected PurlDB data for accuracy to inform code
      updates,
   -  Create public Git repositories to store PURLs for C/C++ packages, and
      the tools to populate these from the PurlDB and import its data in a
      local PurlDB instance. This will be based on the FederatedCode
      decentralized design and will enable on-premises private usage at
      any organization with easy replication;
   -  Release the collected data in the public PurlDB and FederatedCode
      system, and
   -  Create documentation.
|
3. Enable content-based discovery and identification of C/C++ packages.
   The goal is to enable finding existing C/C++ packages when
   scanning a product codebase with:

   - Code matching enabled by the PURL catalog matching index.
      -  Code matching enables the content-based discovery of C/C++ packages
         used in a codebase by looking up fingerprints in an index of known
         C/C++ package fingerprints.
      -  For this, we will deploy and document how to use the AboutCode code 
         matching features locally or remotely to detect the presence of code
         exactly and approximately using fingerprints when scanning a
         codebase. We will continuously update and release an index of the
         C/C++ packages available in the registry for easy deployment
         of local code matching.
   
   - Structured or semi-structured package metadata.
      -  Existing package ecosystems like pypi, npm, or RPM have structured
         metadata manifests that enable fast package discovery by seeking and
         parsing the metadata files. There is no such thing for C/C++.
      -  For this project, we will design a simple package manifest file
         format to store the essential metadata that describe a package and
         promote its usage upstream so that it is available at the root,
         together with the code of a package. These metadata files will also
         be available as the core format for the C/C++ package registry which
         can then evolve to be a convenient aggregator of the metadata
         provided directly by each package in the future.

The high-level tasks include evolving AboutCode tools to:
   -  Update ScanCode Toolkit to collect semi-structured C/C++ package metadata,
   -  Create ScanCode.io pipeline for C/C++ codebase analysis, including metadata collection
      and code matching,
   -  Update MatchCode matching pipeline for C/C++ support using the PurlDB catalog, and
   -  Create documentation.
|
4. Extend the ABOUT file format to serve as the “missing” metadata manifest
   for C/C++ packages. ABOUT files are simple YAML data files to
   document origin, license and other metadata about a software package.
   C/C++ do not have a common, shared format for metadata. The goal
   will be to enable support for C/C++ package metadata in ABOUT files. 
   See the section“ABOUT file metadata
   format” for details and examples. The high-level steps include:

   -  Document key fields, and usage; update AboutCode Toolkit with latest
      PURL support.
   -  Add capability to export ABOUT file export from PurlDB.
|
5. Create an open mapping between CVEs and C/C++ PURLs to enable known
   vulnerability lookups using C/C++ PURLs in the VulnerableCode
   vulnerability database. Note that CVE.org has adopted
   PURL as a key identifier. The high-level steps include:

   -  Update the VulnerableCode open vulnerability database to ensure CVEs
      and security advisories are collected for the tracked C/C++ packages
      and assigned the defined PURLs, and
   -  Create a mapping to improve automation, and/or create data improvers
      to support the correct creation of relationships between
      vulnerabilities and C/C++ PURLs.

Beyond these initial steps, we are planning for future steps to maintain
an up-to-date, evergreen reference catalog of PURLs for C/C++ packages:

-  Engage the open source and vulnerability management communities and
   other key actors in the industry struggling with software
   identification to support the long-term sustainability of these
   efforts,
-  Reach out to key open source projects to “own” the PURL definitions
   for their C/C++ projects, and
-  Reach out to upstream CNAs and CVE.org to add the affected cpp PURLs
   to their security advisories and vulnerability databases to support
   their usage for SBOMs.

**Benefits**
~~~~~~~~~~~~

PURLs and SBOMs are essential to software supply chain management
automation and compliance.

This solution directly resolves a significant pain point for C/C++
development teams and these are the backbone of core software built
and deployed today, from Linux, to JavaScript and the internet
infrastructure and core business and database systems, as well as most
embedded devices. Enabling more accurate software identification with
PURL for C/C++ packages, including in SBOMs that teams publish
and consume, will directly improve software supply
chain integrity and security, and enable compliance automation processes.
Combining open source tools and open data extends these benefits to all
constituents in the larger software ecosystem.

Additional benefits of this project are:

1) Work with the creators of PURL.

AboutCode maintainers created PURL to resolve challenges identifying
software across different open source tools for Software Composition
Analysis (SCA) and vulnerability management. As the creators of PURL,
AboutCode maintainers are uniquely qualified to lead the definition of a new
PURL type for C/C++, resolve issues related to PURL, and share these
with the larger community to improve usage of SBOMs and PURLs, as
well as the ecosystems that use SBOMs with C/C++ PURLs .

2) Ensure SBOM and PURL accuracy for C/C++ packages.

PURL correctness is essential to enable the safe reuse of FOSS. With a
correct PURL, users can find correct and timely data related to
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
and share SBOMs for compliance and share vulnerability exploitability information.
Automation is necessary to manage the increased volume of SBOMs
generated by and provided to software creators, vendors, and users.
Without accurate software identification, automated compliance processes
are impossible.

Any tool, process, or organization that needs to produce or read SBOMs
or VEXs uses PURL, including CycloneDX, SPDX, CSAF, or OpenVEX. A
well-defined software identifier for C/C++ packages in PURL ensures that
SBOMs and other regulatory documents are correct across tools and
standards used to generate the documents, facilitates efficient
compliance process automation, and improves the overall cybersecurity
posture of adopters and their associated ecosystem actors.

4) Streamline ecosystem-wide software supply chain with open catalog for
   C/C++ packages.

An open reference catalog of C/C++ packages enables ecosystem-wide,
shared and accurate identification, and improves the efficiency of
software supply chain automation for these packages. This directly
benefits adopters, ensuring global and accurate C/C++ packages
identification in SBOMs, VEXs, and vulnerability databases.

As a public catalog, these benefits extend to the C/C++ community,
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
