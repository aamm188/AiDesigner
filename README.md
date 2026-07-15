# AiDesigner
# AI Autonomous SolidWorks Designer

## Decision Register - Baseline Architecture

**Document ID:** ADR-BASE-001  
**Version:** 0.1  
**Status:** Draft Baseline  
**Date:** 2026-07-15

---

# 1. Project Objective

The objective of this project is to design and implement an intelligent engineering design system capable of:

* Receiving raw user ideas.
* Extracting engineering requirements.
* Engaging in bidirectional interaction with the user.
* Generating parametric designs.
* Interacting directly with SolidWorks.
* Detecting and correcting design errors.
* Evaluating the design from engineering, manufacturing, and optimization perspectives.

Ultimate Goal:

To create an **Autonomous Engineering Design System**, not merely a CAD Copilot.

---

# 2. High-Level Architectural Decision

## Decision ID: 
ADR-001

## Decision:
The system will be designed as a multi-layered, multi-agent architecture.

Overall Architecture:

```text
User Interface
        |
Conversation Layer
        |
Engineering Intelligence Layer
        |
Universal Design Model
        |
CAD Adapter Layer
        |
SolidWorks Integration
        |
SolidWorks Native Model
```

## Rationale:
Decoupling the design logic from the CAD software ensures scalability and prevents strict vendor lock-in to SolidWorks.

---

# 3. Decision: No Direct SolidWorks Code Generation by LLM

## Decision ID: 
ADR-002

## Decision:
The LLM will not directly generate SolidWorks Macros or VBA code.

The model must first generate:
* Design Intent
* Engineering Logic
* Features
* Parametric Relationships

The CAD Adapter Layer will then execute this information.

## Rationale:
Direct code generation leads to:
* Loss of Design Intent
* Frequent API errors
* Strong dependency on SolidWorks-specific syntax

---

# 4. Decision: Preserving the Native SolidWorks Model

## Decision ID: 
ADR-003

## Decision:
The primary output must be:
* SLDPRT
* SLDASM

STEP will only be used for data exchange or as an auxiliary tool.

## Rationale:
The STEP format does not preserve the following:
* Feature Tree
* Sketch Relations
* Equations
* Configurations
* Design Intent

---

# 5. Decision: Creating a Universal Design Model

## Decision ID: 
ADR-004

## Decision:
A CAD-agnostic intermediate model will be created.

This model will include:
* Geometry
* Features
* Constraints
* Engineering Intent
* Manufacturing Information
* Validation Data

---

# 6. Decision: Engineering Intent over Pure Geometry

## Decision ID: 
ADR-005

## Decision:
The system will not merely generate geometry.

Every feature must have an underlying engineering rationale.

Example:

Instead of:
```text
Hole Diameter = 8 mm
```

The model must understand:
```text
Purpose: M8 bolt mounting
Standard: ISO
Manufacturing: Drilling
Tolerance: Defined
```

---

# 7. Decision: Multi-Agent Architecture

## Decision ID: 
ADR-006

## Decision:
The system will consist of specialized Agents.

Initial Agents:

## Requirement Agent
**Task:** Extract requirements.

## Planner Agent
**Task:** Decompose the design problem.

## Knowledge Agent
**Task:** Manage standards and engineering knowledge.

## CAD Design Agent
**Task:** Generate the design model.

## Verification Agent
**Task:** Validate design correctness.

## Optimization Agent
**Task:** Improve the design.

## Repair Agent
**Task:** Fix errors.

---

# 8. Decision: Utilizing Knowledge Base + RAG

## Decision ID: 
ADR-007

## Decision:
Engineering knowledge will be maintained in a centralized Knowledge Base.

Including:
* Standards
* Materials
* Tolerances
* Manufacturing methods
* Standard parts
* Design rules

---

# 9. Decision: Utilizing a Knowledge Graph

## Decision ID: 
ADR-008

## Decision:
A Knowledge Graph will be used to represent engineering relationships.

Example:
```text
Bolt M8
-> requires -> Hole Diameter
-> requires -> Tolerance
-> depends on -> Material
```

---

# 10. Decision: SolidWorks Connector Architecture

## Decision ID: 
ADR-009

## Decision:
The SolidWorks communication layer will be developed in C#.

Architecture:

```text
AI Engine
(Python)
        |
API Layer
        |
C# Bridge
        |
SolidWorks API
```

## Rationale:
* Greater stability
* Better compatibility with COM API
* Improved management of Assemblies and Events

---

# 11. Decision: Communication Protocol

## Decision ID: 
ADR-010

## Current Decision:
Version 1: REST API  
Future Version: gRPC

## Rationale:
In the initial phase, data model changes will be frequent; flexibility is more critical than communication speed.

---

# 12. Decision: Not Starting the Project with Agents

## Decision ID: 
ADR-011

## Decision:
Before developing Agents, the following must be defined:
1. Data Model
2. Design Intent Schema
3. Knowledge Model

---

# 13. Decision: Phased Project Development

## Decision ID: 
ADR-012

## Phases:

### Phase 1
Simple Parametric Part Design

### Phase 2
Assembly

### Phase 3
Engineering Verification

### Phase 4
Optimization

### Phase 5
Autonomous Design Loop

---

# 14. Decision: Proof of Concept (PoC)

## Decision ID: 
ADR-013

## Selection:
Motor Mount Assembly

Rationale:
Includes:
* Part
* Assembly
* Fasteners
* Constraints
* Material
* Load
* Manufacturing considerations

---

# 15. Decision: Human in the Loop

## Decision ID: 
ADR-014

## Decision:
Initial versions will not be fully autonomous.

Cycle:
```text
AI Proposal
      ↓
Human Approval
      ↓
CAD Generation
```

Upon maturation:
```text
AI
      ↓
Verification
      ↓
Automatic Execution
```

---

# 16. Decision: Design Version Control

## Decision ID: 
ADR-015

## Decision:
The system will maintain a Design History.

Every change will be logged:
* What was changed?
* Why was it changed?
* Which Agent proposed it?
* What was the outcome?

---

# 17. Immutable Project Principles

## Baseline Rules
1. Design Intent must be preserved.
2. The primary output must be Native CAD.
3. AI must not mimic an engineer by merely generating raw geometry; it must model the design logic.
4. The architecture must be extensible to other CAD software.
5. Architectural decisions will not be altered without logging a Revision.

---

# Current Status

This document is the initial Baseline for the project.

Subsequent decisions must be recorded in the form of:
* New Revisions
* New Decisions
* Change Requests

End of Document.
