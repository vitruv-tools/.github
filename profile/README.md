# Vitruvius

[![DOI](https://zenodo.org/badge/67610524.svg)](https://doi.org/10.5281/zenodo.13991787)

Vitruvius is a framework for view-based (software) development.
It assumes different models to be used for describing a system, which are automatically kept consistent by the framework executing (semi-)automated rules that preserve consistency.
These models are modified only via views, which are projections from the underlying models.

A bunch of information on what Vitruvius is and how it can be used can be found in the [GitHub wiki](https://github.com/vitruv-tools/.github/wiki).

## Idea

Vitruvius is based on the idea of a _Single Underlying Model (SUM)_, which represents all information about a system in a single, redundancy-free und inherently consistent model.
A SUM requires the definition of one overarching, redundancy-free model for every development project, although in practice different tools for different purposes are used and thus such a SUM is hard to construct and maintain.
Vitruvius extends the concept to a _Virtual Single Underlying Model (V-SUM)_.
It is _virtual_, because it behaves like a SUM in the sense that it is always consistent, but it does not achieve this by being free of redundancies and implicit dependencies but by having explicit rules that preserve consistency of the different models after they have been changed via views.

_Vitruvius_ stands for "VIew-cenTRic engineering Using a VIrtual Underlying Single model" and is developed at the [_Dependability of Software-intensive Systems group (DSiS)_](https://dsis.kastel.kit.edu/) at the _Karlsruhe Institute of Technology (KIT)_.

## Getting Started

Vitruvius is realized as a set of Maven projects.
It depends on the _Eclipse Modeling Framework (EMF)_ as the modelling environment, on _Xtext_ for language development (in particular the languages for specifying how consistency is preserved), and _Xtend_ and _Java_ for code.
No Eclipse installation is required to build or run a Vitruvius project.

**Prerequisites:** Java 21 and Git. Maven is not required, since all projects come with a Maven wrapper (`mvnw`).

The easiest way to start is the [Methodologist-Template](https://github.com/vitruv-tools/Methodologist-Template), a ready-to-build V-SUM with two metamodels, a set of Reactions, and tests:

```bash
git clone https://github.com/vitruv-tools/Methodologist-Template.git
cd Methodologist-Template
./mvnw clean verify
```

If the build succeeds and all tests pass, continue with the [tutorial](https://github.com/vitruv-tools/Methodologist-Template/blob/main/tutorial.md) in the same repository.
It walks you through extending a metamodel, writing a consistency preservation rule in the Reactions language, and testing it.

Alternatively, you can
- generate a V-SUM project from your own metamodels and Reactions with the [Vitruv-CLI](https://github.com/vitruv-tools/Vitruv-CLI), or
- use the web-based [Methodologist Dashboard](https://[2001:7c0:2313:1:f816:3eff:fe18:4ccf]/) to upload metamodels and edit Reactions in the browser. The Dashboard is currently only reachable via IPv6; a host name will follow.

More background on the concepts, the roles involved in using Vitruvius, and on extending Vitruvius itself is provided in the [Getting Started wiki page](https://github.com/vitruv-tools/.github/wiki/Getting-Started).

## Using Vitruvius in Your Own Maven Project

All artifacts are published under the group ID [`tools.vitruv`](https://central.sonatype.com/namespace/tools.vitruv) on Maven Central.
We recommend inheriting from our parent POM, which manages plugin versions and repositories:

```xml
<parent>
  <groupId>tools.vitruv</groupId>
  <artifactId>parent</artifactId>
  <version>4.0.0</version>
</parent>
```

The artifacts most projects need are:

| Artifact | Purpose |
| -------- | ------- |
| `tools.vitruv.framework.vsum` | Creating and running a V-SUM |
| `tools.vitruv.framework.views` | Accessing and modifying the models in a V-SUM via views |
| `tools.vitruv.change.propagation`, `tools.vitruv.change.interaction` | Change propagation and user interaction during consistency preservation |
| `tools.vitruv.dsls.reactions.language`, `tools.vitruv.dsls.reactions.runtime` | Compiling (via the `xtext-maven-plugin`) and executing consistency preservation rules written in the Reactions language |
| `tools.vitruv.change.testutils.integration` | Test utilities for V-SUMs and their consistency preservation rules |

```xml
<dependency>
  <groupId>tools.vitruv</groupId>
  <artifactId>tools.vitruv.framework.vsum</artifactId>
  <version>4.0.0</version>
</dependency>
```

See the [POMs of the Methodologist-Template](https://github.com/vitruv-tools/Methodologist-Template/blob/main/consistency/pom.xml) for a complete working configuration, including the generation of code from Reactions.

**Snapshot builds** of the current development state are published to the [Maven Central snapshot repository](https://central.sonatype.com/repository/maven-snapshots/).
Projects inheriting from the parent POM resolve snapshots automatically; otherwise add:

```xml
<repositories>
  <repository>
    <id>central-portal-snapshots</id>
    <url>https://central.sonatype.com/repository/maven-snapshots/</url>
    <releases><enabled>false</enabled></releases>
    <snapshots><enabled>true</enabled></snapshots>
  </repository>
</repositories>
```

## Structure

The Vitruvius project is split into several repositories with well defined dependencies:

| Repository | Depends on | Description | &nbsp;&nbsp;&nbsp;&nbsp;CI&nbsp;&nbsp;&nbsp;&nbsp; |
| ---------- | ---------- | ----------- | -- |
| [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)           |                                                                                                              | Underlying definition of changes in Ecore-based models and interfaces for specifying the propagation of changes between models to preserve their consistency, as well as an interface and a default implementation for orchestrating the execution of such specifications. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-Change/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-Change/actions/workflows/ci.yml) |
| [Vitruv-DSLs](https://github.com/vitruv-tools/Vitruv-DSLs)               | [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)                                               | Several languages for specifying consistency preservation rules in terms of model transformations for keeping models consistent. Currently, the `Reactions` and the `Commonalities` language are available with different levels of maturity.                              | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-DSLs/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-DSLs/actions/workflows/ci.yml) |
| [Vitruv](https://github.com/vitruv-tools/Vitruv)                         | [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)                                               | Central Vitruvius framework, providing the definition of a V-SUM (Virtual Single Underlying Model) containing development artifacts to be kept consistent and to be accessed and modified via views.                                                                       | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv/actions/workflows/ci.yml) |
| [Vitruv-Server](https://github.com/vitruv-tools/Vitruv-Server) | [Vitruv](https://github.com/vitruv-tools/Vitruv), [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)  | Vitruv server implementation | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-Server/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-Server/actions/workflows/ci.yml) |
| [Vitruv-CLI](https://github.com/vitruv-tools/Vitruv-CLI) | [Vitruv](https://github.com/vitruv-tools/Vitruv), [Vitruv-Server](https://github.com/vitruv-tools/Vitruv-Server)  | Vitruv CLI implementation | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-CLI/actions/workflows/main.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-CLI/actions/workflows/main.yml) |
| [Vitruv-CaseStudies](https://github.com/vitruv-tools/Vitruv-CaseStudies) | [Vitruv](https://github.com/vitruv-tools/Vitruv), [Vitruv-DSLs](https://github.com/vitruv-tools/Vitruv-DSLs) | Case studies for the Vitruvius framework, in particular an example application of Vitruvius in the domain of component-based systems engineering using Java, UML and [PCM](https://github.com/palladiosimulator) models.                                                   | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-CaseStudies/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-CaseStudies/actions/workflows/ci.yml) |

These are the primary maintained repositories: the first three are the core repositories providing Vitruvius, Vitruv-Server and Vitruv-CLI provide remote access to a V-SUM and the generation of V-SUM projects, and Vitruv-CaseStudies provides a demo application of consistency preservation.
There are further repositories in this organization with different experiments we have performed around Vitruvius with individual degrees of maturity and maintenance.

## Build and Deployment

We build, integrate and deploy our projects using Maven and GitHub Actions. For details see [our wiki](https://github.com/vitruv-tools/.github/wiki/Build-and-Continuous-Integration).
