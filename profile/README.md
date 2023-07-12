# Vitruvius

Vitruvius is a framework for view-based software development. It assumes different models to be used for describing a software system,
which are automatically kept consistent by the framework executing (semi-)automated rules that preserve consistency. These models are modified only via views, which are projections from the underlying models.

A bunch of information on what Vitruvius is and how it can be used can be found in the [GitHub wiki](https://github.com/vitruv-tools/.github/wiki).

## Idea

Vitruvius is based on the idea of a _Single Underlying Model (SUM)_, which represents all information about a software system in a single, redundancy-free und inherently consistent model. A SUM requires the definition of one overarching, redundancy-free model for every development project, although in practice different tools for different purposes are used and thus such a SUM is hard to construct and maintain. Vitruvius extends the concept to a _Virtual Single Underlying Model (V-SUM)_. It is _virtual_, because it behaves like a SUM in the sense that it is always consistent, but it does not achieve this by being free of redundancies and implicit dependencies but by having explicit rules that preserve consistency of the different models after they have been changed via views.

_Vitruvius_ stands for "VIew-cenTRic engineering Using a VIrtual Underlying Single model" and is developed at the
[_Dependability of Software-intensive Systems group (DSiS)_](https://dsis.kastel.kit.edu/) at the _Karlsruhe Institute of Technology (KIT)_.

## Usage

In short, Vitruvius is realized as a set of Eclipse plug-ins. It depends on the _Eclipse Modeling Framework (EMF)_ as the modelling environment, on _Xtext_ for language development (in particular the languages for specifying how consistency is preserved), and _Xtend_ and _Java_ for code. 
It can be installed in Eclipse via our [nightly update site](https://vitruv.tools/updatesite/nightly/aggregated). A wiki page provides [detailed instructions for using or extending Vitruvius](https://github.com/vitruv-tools/.github/wiki/Getting-Started).

## Structure

The Vitruvius project is split into several repositories with well defined dependencies:

| Repository | Depends on | Description | &nbsp;&nbsp;&nbsp;CI&nbsp;&nbsp;&nbsp;&nbsp; |
| ---------- | ---------- | ----------- | -- |
| [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change) | - | Core artifacts for representing model changes and for defining their processing to preserve consistency with the central interface `ChangePropagationSpecification`. In addition, interactions to involve the user into the change preservation process are provided. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-Change/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-Change/actions/workflows/ci.yml) |
| [Vitruv-DSLs](https://github.com/vitruv-tools/Vitruv-DSLs) | [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change) | Languages for defining consistency preservation in terms of model transformations. Currently, the `Reactions` and the `Commonalities` language are available with different levels of maturity. The `Reactions` language is most used and best maintained one. The DSLs only depend on the change core artifacts but on no other Vitruvius artifacts, such that they can be used standalone to define and execute model transformations. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-DSLs/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-DSLs/actions/workflows/ci.yml) |
| [Vitruv-V-SUM](https://github.com/vitruv-tools/Vitruv) | [Vitruv-Change](https://github.com/vitruv-tools/Vitruv-Change) | The central Vitruvius framework providing the V-SUM and views concepts. In the implementation, a V-SUM is called `VirtualModel`, which is instantiated with a set of `ChangePropagationSpecifications` (no matter whether they are developed with the DSLs or just as an implementation of the interface defined in the Vitruv-Change repository). The `VirtualModel` then provides functionality to derive and modify views and to propagate the changes in these views back to the `VirtualModel`, which then executes the `ChangePropagationSpecifications` to preserve consistency. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv/actions/workflows/ci.yml) |
| [Vitruv-CaseStudies](https://github.com/vitruv-tools/Vitruv-CaseStudies) | [Vitruv-V-SUM](https://github.com/vitruv-tools/Vitruv), [Vitruv-DSLs](https://github.com/vitruv-tools/Vitruv-DSLs) | Case studies for the Vitruvius framework, particularly consisting of an example application of Vitruvius for component-based systems, providing `ChangePropagationSpecifications` for [PCM](https://palladio-simulator.com), UML and Java. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-CaseStudies/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-CaseStudies/actions/workflows/ci.yml) |
| [Vitruv-Tool-Adapters](https://github.com/vitruv-tools/Vitruv-Tool-Adapters) | [Vitruv-V-SUM](https://github.com/vitruv-tools/Vitruv) | __!Development discontinued!__ A set of adapters for tools to use Vitruvius in, i.e., in which models can be modified and changes are then propagated to and kept consistent in an underlying V-SUM. Currently, adapters for the Eclipse IDE and in particular for modifying EMF models in any kind of editor, as well as a wizard to setup and use a V-SUM in Eclipse are provided. | [![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-Tool-Adapters/actions/workflows/ci.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-Tool-Adapters/actions/workflows/ci.yml) |

These are the primary maintained repositories, of which the first three are the core repositories providing Vitruvius and the last two ones providing a demo application of consistency preservation and adaptation in development tools. There are further repositories in this organization with different experiments we have performed around Vitruvius with individual degrees of maturity and maintenance.

## Build and Deployment

We build, integrate and deploy our projects using Maven Tycho and GitHub Actions. For details see [our wiki](https://github.com/vitruv-tools/Vitruv/wiki/Build-and-Continuous-Integration). The deployment is supported by the two further repositories [updatesite](https://github.com/vitruv-tools/updatesite) and [Vitruv-Build-AggregatedUpdateSite](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite). All repositories generate Eclipse update sites out of their artifacts and deploy them to the [updatesite](https://github.com/vitruv-tools/updatesite) repository. From these artifacts, an aggregated update site is created running the workflow of the [Vitruv-Build-AggregatedUpdateSite](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite) project, from which Vitruvius with all its dependencies can then be installed within Eclipse.
