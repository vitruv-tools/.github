# Vitruvius

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

## Usage

In short, Vitruvius is realized as a set of Eclipse plug-ins.
It depends on the _Eclipse Modeling Framework (EMF)_ as the modelling environment, on _Xtext_ for language development (in particular the languages for specifying how consistency is preserved), and _Xtend_ and _Java_ for code. 
It can be installed in Eclipse via our [nightly update site](https://vitruv.tools/updatesite/nightly/aggregated).
A wiki page provides [detailed instructions for using or extending Vitruvius](https://github.com/vitruv-tools/.github/wiki/Getting-Started).

## Structure

The Vitruvius project is split into several repositories with well defined dependencies:

| Repository | Depends on | Description | &nbsp;&nbsp;&nbsp;&nbsp;CI&nbsp;&nbsp;&nbsp;&nbsp; |
| ---------- | ---------- | ----------- | -- |
| [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)           |                                                                                                              | Underlying definition of changes in Ecore-based models and interfaces for specifying the propagation of changes between models to preserve their consistency, as well as an interface and a default implementation for orchestrating the execution of such specifications. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-Change/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-Change/actions/workflows/ci.yml) |
| [Vitruv-DSLs](https://github.com/vitruv-tools/Vitruv-DSLs)               | [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)                                               | Several languages for specifying consistency preservation rules in terms of model transformations for keeping models consistent. Currently, the `Reactions` and the `Commonalities` language are available with different levels of maturity.                              | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-DSLs/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-DSLs/actions/workflows/ci.yml) |
| [Vitruv](https://github.com/vitruv-tools/Vitruv)                         | [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change)                                               | Central Vitruvius framework, providing the definition of a V-SUM (Virtual Single Underlying Model) containing development artifacts to be kept consistent and to be accessed and modified via views.                                                                       | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv/actions/workflows/ci.yml) |
| [Vitruv-CaseStudies](https://github.com/vitruv-tools/Vitruv-CaseStudies) | [Vitruv](https://github.com/vitruv-tools/Vitruv), [Vitruv-DSLs](https://github.com/vitruv-tools/Vitruv-DSLs) | Case studies for the Vitruvius framework, in particular an example application of Vitruvius in the domain of component-based systems engineering using Java, UML and [PCM](https://github.com/palladiosimulator) models.                                                   | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-CaseStudies/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-CaseStudies/actions/workflows/ci.yml) |

These are the primary maintained repositories, of which the first three are the core repositories providing Vitruvius and the last one provides a demo application of consistency preservation.
There are further repositories in this organization with different experiments we have performed around Vitruvius with individual degrees of maturity and maintenance.

## Build and Deployment

We build, integrate and deploy our projects using Maven Tycho and GitHub Actions. For details see [our wiki](https://github.com/vitruv-tools/Vitruv/wiki/Build-and-Continuous-Integration).
The deployment is supported by the two further repositories [updatesite](https://github.com/vitruv-tools/updatesite) and [Vitruv-Build-AggregatedUpdateSite](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite).
All repositories generate Eclipse update sites out of their artifacts and deploy them to the [updatesite](https://github.com/vitruv-tools/updatesite) repository.
From these artifacts, an aggregated update site is created running the workflow of the [Vitruv-Build-AggregatedUpdateSite](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite) project, from which Vitruvius with all its dependencies can then be installed within Eclipse.
