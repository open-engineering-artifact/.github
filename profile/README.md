# Open Engineering Artifact

A standard way to describe, build, verify, publish, and consume engineering artifacts.

![Open Engineering Artifact hero-banner.png](../assets/.gitkeep

Open Engineering Artifact defines what an Artifact is within the Open Engineering ecosystem.

An Artifact is a durable, identifiable, machine-readable engineering output produced by people, tools, Picos, workflows, or other Open Engineering components.

Artifacts turn engineering activity into something that can be stored, inspected, verified, referenced, composed, reproduced, and reused.

⸻

Why Artifact?

Engineering systems continuously produce useful outputs:

* architecture diagrams
* Architecture Decision Records
* reports
* specifications
* source packages
* generated websites
* images and media
* build outputs
* test results
* evidence
* manifests
* datasets
* deployment bundles
* documentation
* simulation results

Without a common abstraction, every system invents its own way of describing and transporting these outputs.

Open Engineering Artifact provides that common abstraction.

Engineering activity
        │
        ▼
┌──────────────────────┐
│       Artifact       │
│                      │
│  identity            │
│  type                │
│  content             │
│  metadata            │
│  provenance          │
│  relationships       │
│  integrity           │
│  lifecycle           │
└──────────┬───────────┘
           │
           ▼
 store • inspect • verify
 publish • compose • reuse

An Artifact is therefore more than a file.

A file may be its payload.

The Artifact is the engineering object around that payload.

⸻

Definition and Implementation

Open Engineering deliberately separates the definition of an engineering concept from its implementation.

open-engineering-artifacts
          │
          │ defines
          ▼
      Artifact
          │
          │ implemented by
          ▼
 open-engineering-artifact

Open Engineering Artifacts

open-engineering-artifacts defines the Artifact domain:

* conceptual model
* schemas
* conventions
* lifecycle
* relationships
* validation rules
* interoperability contracts

Open Engineering Artifact

open-engineering-artifact provides the reference implementation of that definition.

This organization is concerned with making Artifacts operational.

⸻

Artifact Model

At its simplest:

apiVersion: open-engineering.io/v1alpha1
kind: Artifact
metadata:
  name: example
spec:
  type: document
  content:
    mediaType: text/markdown
    location: ./README.md
  provenance:
    producer: example-workflow
  relationships: []
  integrity: {}

The exact schemas may evolve, but the architectural principle remains stable:

Content becomes an engineering Artifact when it gains identity, context, provenance, and lifecycle.

⸻

Identity

Every Artifact should be independently addressable.

Open Engineering identifiers provide stable identities that allow an Artifact to be referenced without depending solely on filenames, repository paths, URLs, or storage implementations.

Artifact
   │
   ├── identifier
   ├── human-readable name
   ├── version
   └── type

This allows other Open Engineering objects to refer to an Artifact explicitly.

⸻

Content

Artifact content can take many forms.

Artifact
├── Markdown
├── JSON / YAML
├── source code
├── package
├── container metadata
├── diagram
├── image
├── video
├── audio
├── dataset
├── report
├── evidence
└── arbitrary binary content

The Artifact abstraction does not replace these formats.

It provides a consistent engineering envelope around them.

⸻

Metadata

Artifacts carry enough metadata to explain what they are and how they should be handled.

Typical metadata includes:

metadata:
  name: architecture-report
  version: 1.0.0
  labels:
    domain: architecture
    project: example

Metadata enables discovery, automation, policy, composition, and reporting.

⸻

Provenance

An Artifact should be able to answer:

Where did this come from?

Provenance may identify:

* the human who authored it
* the Pico that generated it
* the workflow that produced it
* the source repository
* the input Artifacts
* the tool or model used
* the execution that created it
* the timestamp and version

Conceptually:

Inputs
   │
   ▼
Execution
   │
   ▼
Artifact
   │
   └── provenance

This makes generated engineering outputs explainable and traceable.

⸻

Relationships

Artifacts rarely exist in isolation.

An Artifact can relate to other Artifacts and engineering objects.

Architecture
     │
     ├── described-by ──► Documentation
     │
     ├── decided-by ────► ADR
     │
     ├── evidenced-by ──► Test Result
     │
     └── rendered-as ───► Diagram

Relationships turn collections of files into an engineering knowledge graph.

⸻

Integrity

Artifacts should be verifiable.

Integrity information can include:

* cryptographic digest
* checksum
* signature
* version
* provenance
* validation status

For example:

integrity:
  algorithm: sha256
  digest: "..."

This enables systems to determine whether an Artifact is the same object that was originally produced.

⸻

Lifecycle

Artifacts move through an engineering lifecycle.

create
  │
  ▼
validate
  │
  ▼
publish
  │
  ▼
consume
  │
  ▼
compose
  │
  ▼
supersede / archive

Different Artifact types may introduce additional lifecycle states while retaining this common foundation.

⸻

Artifacts and Evidence

Artifacts are especially important to Open Engineering’s Evidence primitive.

An engineering claim can point to the Artifact that supports it.

Claim
  │
  └── supported by
          │
          ▼
       Evidence
          │
          └── represented by
                  │
                  ▼
               Artifact

This creates a foundation for auditable engineering systems where conclusions can be traced back to their supporting material.

⸻

Artifacts and Picos

A Pico can both consume and produce Artifacts.

        Artifact
           │
           ▼
        ┌──────┐
        │ Pico │
        └──┬───┘
           │
           ▼
        Artifact

For example, a Pico might consume:

requirements.md
architecture.yaml
source-code.tar

and produce:

analysis.json
report.md
diagram.svg
evidence.json

Because each result is an Artifact, downstream Picos and workflows can consume those outputs using the same abstraction.

⸻

Artifacts and Workflows

Artifacts provide durable boundaries between workflow stages.

Investigation
     │
     ▼
  Artifact
     │
     ▼
Execution
     │
     ▼
  Artifact
     │
     ▼
Reporting

This is preferable to hidden state passing between tools because intermediate engineering outputs remain inspectable and reusable.

⸻

Composition

Artifacts are designed for composition.

Artifact A ─┐
Artifact B ─┼──► Composer ──► Artifact D
Artifact C ─┘

A Composer can combine multiple Artifacts into a new Artifact while retaining their identities and provenance.

Examples include:

ADRs + architecture model
        ↓
architecture report
source + configuration
        ↓
deployable package
observations + evidence
        ↓
investigation report
images + audio + screenplay
        ↓
motion-picture project

Composition is therefore a first-class capability rather than an accidental consequence of copying files.

⸻

Storage Is Not the Model

Artifacts may ultimately live in many systems:

Git
OCI registry
object storage
package registry
database
filesystem
artifact repository
content-addressed storage

The Artifact definition should not depend on any one of them.

                Artifact
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
      Git         OCI       Object
                             Storage

Storage is an implementation concern.

Artifact semantics remain portable.

⸻

Open Engineering Architecture

Artifacts connect several important parts of Open Engineering.

Definitions
     │
     ▼
Conventions
     │
     ▼
Parsers
     │
     ▼
Rules
     │
     ▼
Capsules
     │
     ▼
Picos
     │
     ▼
Executions
     │
     ▼
Artifacts
     │
     ▼
Evidence / Reporting / Composition

This makes Artifact one of the connective abstractions between engineering intent and durable engineering output.

⸻

Design Principles

The Open Engineering Artifact implementation should remain:

Open
Based on documented, portable formats and interfaces.

Inspectable
Humans and machines should be able to understand what an Artifact contains.

Traceable
Artifacts retain their origin and relevant lineage.

Immutable where practical
Published Artifact versions should represent stable engineering facts.

Composable
Artifacts can become inputs to subsequent engineering activity.

Verifiable
Integrity and provenance can be checked independently.

Portable
Artifacts must not depend on a single vendor, cloud, registry, or runtime.

Automation-friendly
Picos, workflows, CI/CD systems, and Kubernetes controllers should be able to manipulate Artifacts programmatically.

⸻

Repository Direction

Repositories within this organization may provide reference implementations for capabilities such as:

artifact
├── model
├── schema
├── parser
├── validator
├── CLI
├── SDK
├── storage
├── provenance
├── integrity
├── resolver
└── Kubernetes integration

Implementations should remain modular so that an Artifact can be useful without requiring the entire Open Engineering platform.

⸻

Example

Imagine an architecture analysis performed by a Pico.

workspace
   │
   ▼
architecture.yaml
   │
   ▼
┌────────────────────┐
│ Architecture Pico  │
└─────────┬──────────┘
          │
          ├──► findings.json
          ├──► architecture.svg
          └──► report.md

Instead of treating these simply as three files, Open Engineering can represent them as three related Artifacts:

Architecture Analysis
        │
        ├── findings
        │      └── findings.json
        │
        ├── visualization
        │      └── architecture.svg
        │
        └── report
               └── report.md

Each can carry its own identity, provenance, integrity information, and relationships while remaining part of the same engineering execution.

⸻

The Bigger Idea

Software engineering has traditionally focused heavily on source code.

Modern engineering produces much more.

AI agents generate analyses.
Picos create observations.
Workflows generate evidence.
Systems create diagrams.
Composers assemble documents.
Simulations produce datasets.
Build systems produce packages.

Open Engineering treats these outputs as first-class engineering objects.

             Open Engineering
                   │
                   ▼
              Engineering
                Activity
                   │
                   ▼
                Artifact
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Inspect     Verify      Reuse
        │          │          │
        └──────────┼──────────┘
                   ▼
              Engineering
               Knowledge

Open Engineering Artifact

Engineering output should not disappear into files and folders.

It should become something that can be identified, understood, verified, connected, and reused.

That something is an Artifact.

