## General Systems Theory

A general system has **structure** and **properties**. Everything else is an **instantiation** of that structure in a concrete domain.

### Structure

- **Node**: a constituent of a system. Every node is itself a system -- recursive, with no atomic floor. A node viewed within its enclosing system is that system's **subsystem**.
- **Relation**: a directed dependency between two nodes. Direction encodes what one node requires from the other.
- **System**: nodes plus the directed relations between them.
- **Behaviour**: the complete set of relations a system has with its environment -- what it depends on and what it provides.

### Properties

- **Complexity**: a theoretically quantifiable property of a node or a relation. A system's complexity is the aggregate over its nodes and relations.
- **Minimally complex system**: a system whose behaviour is identical to a more complex system, but whose structure has the fewest nodes and relations necessary to produce that behaviour.

### Instantiations of the general system structure

#### 1. System of conceptions

Concepts represented in statements form a **system of conceptions**: each conception is a node; the dependencies between conceptions are its relations.

#### 2. Real world

The real world instantiates the structure with **information** -- sets of empirical observations -- as relations between real-world entities as nodes.

##### 2a. Humanity

A **resource** is a real-world entity of economical value -- the economy being humanity's subsystem for allocating scarce entities among competing uses. Every real-world instantiation of a system consumes resources in proportion to its complexity. Complexity is minimized because each node and relation beyond the minimum is resource expenditure that buys no additional behaviour.

##### 2b. Codebase

Language-specific concepts -- functions, classes, interfaces, modules -- form an **analog system**: a codebase-domain instantiation whose nodes and relations mirror the system of conceptions the code implements.

##### 2c. Humanity & codebase

Running code requires resource allocation, and that allocation correlates with the analog system's structure. The analog system structure must therefore be minimally complex. The analog system structure is declared in `SUBSYSTEM.md` files spread across the codebase, colocated with the corresponding subsystem implementation code.

##### 2d. Humanity & system of conceptions

Compute allocates resources, and cognitive processing of concepts requires compute. A system of conceptions instantiated in the real world must therefore be minimally complex.

## SUBSYSTEM.md Convention (instantiation 2c)

A `SUBSYSTEM.md` is a conceptual contract declaring what a subsystem is and why it exists — the declared analog system structure of instantiation 2c in the General Systems Theory. It is placed in the subsystem's directory and contains no code-level details.

### Structural definition

Each `SUBSYSTEM.md` file declares one node in the analog system graph. The complete set of `SUBSYSTEM.md` files constitutes the declared analog system.

**Node identity.** The `name` and `definition` fields together determine a node. Two nodes with identical definitions are redundant — the system is not minimal. A definition that must reference another node's interior signals a missing decomposition.

**Graph completeness.** Every input on one node must correspond to an output on some other node, and vice versa. Every node has at least one relation.

**Nesting.** Directory nesting forms a strict tree. A child's relations operate within its parent's boundary — it may not introduce relations the parent does not declare. The parent's behaviour is the composition of its children's behaviours.

**Privacy.** A node's interior is its private concern. No `SUBSYSTEM.md` may describe another node's interior. Cross-references exist only through matching input/output relations.

**Relation minimality.** Every relation must carry meaning not derivable from existing relations.

### File format

A `SUBSYSTEM.md` file is a single YAML frontmatter block delimited by `---`. There is no Markdown body outside the frontmatter.

```yaml
---
name: kebab-case-role-label
definition: >
  Block scalar. Why this subsystem exists, what invariant it upholds,
  what would be lost without it.
inputs:
  - name: kebab-case-relation-label
    description: >
      What meaning flows in and why.
outputs:
  - name: kebab-case-relation-label
    description: >
      What meaning flows out and why.
---
```

Field constraints:
- `name`: kebab-case string.
- `definition`: YAML block scalar (`>`).
- `inputs` / `outputs`: list of `{name, description}` objects. Each `name` is a kebab-case label. Each `description` is a block scalar.

### How to use SUBSYSTEM.md files

- Treat each `SUBSYSTEM.md` as a pre-declared conceptual contract.
- Read the relevant `SUBSYSTEM.md` before working on any code within that subsystem.
- Directory nesting mirrors subsystem nesting -- a deeper `SUBSYSTEM.md` describes a node within its parent subsystem.
- When a subsystem is created, split, merged, or removed, update the corresponding `SUBSYSTEM.md` files.
- Never include code-level details in a `SUBSYSTEM.md`. If you find yourself writing a function name, variable, file path, or package name -- stop. Restate at the conceptual level.

### Authoring

`SUBSYSTEM.md` files are produced by the designer agent, because they require first-principles reasoning to ground subsystem boundaries. Do not author or substantially revise a `SUBSYSTEM.md` without invoking the designer agent.

### Conceptual authority

`SUBSYSTEM.md` files are the authoritative declaration of the system structure. Code is an instantiation of that declared structure. When drift is detected between code and the declared structure, the code must be corrected to match the declared structure. If the declared structure itself needs revision, that is a design decision routed through the designer agent -- not a consequence of a code change.

### System-structure-aware modifications

When a subsystem is modified, its behavioural contract must be re-analysed. If the modification changes the subsystem's behaviour -- what it provides or depends on -- those behavioural changes must be propagated to every subsystem reachable through the input/output relation graph, recursively.

## Agent Workflow Convention

All non-trivial work proceeds through two phases: **Design** then **Implementation**. Design produces conceptual grounding through systems-level reasoning. Implementation executes plans with disciplined precision. The parent orchestrates both phases. Child agents are self-contained roles -- they do their job and return their output.

This applies equally to single-agent work. A lone agent still designs first, then implements. When design collapses to a trivial plan, that is acceptable; skipping design is not.

### Agents

- **`designer`**: Socratically questions 'why' to arrive at the underlying definitions of subsystems, decomposing a problem from first principles into nodes, relations, and invariants per the General Systems Theory. Produces conceptual grounding materialized as `SUBSYSTEM.md` content. Invoked during Design when systems-level reasoning is needed.
- **`impl-planner`**: Transforms conceptual grounding into a single implementation plan. Uses the `superpowers:writing-plans` skill. May invoke `designer` when the provided representation is ambiguous.
- **`impl`**: Executes a single assigned plan with disciplined precision. Does not redesign, does not invoke `designer`. Halts with `BLOCKED` when encountering uncertainty the plan does not resolve.
- **`statement-rewriter`**: Makes a statement's implicit system of conceptions explicit, then minimizes it. Standalone tool for prose and conceptual writing, not for code. **Only invoke when the user explicitly requests it.**

### Statement Rewriter -- Operational Guide

The `statement-rewriter` reduces a statement's conceptual complexity by iteratively minimizing its conception graph -- making the graph explicit, classifying each node as load-bearing or incidental, and rewriting from the minimal subgraph.

Invoke only on explicit user request. Do not auto-invoke.

Rewrites are iterative. Each iteration modifies the conception graph. The user may accept, reject, or redirect after any iteration.

The rewriter invokes `designer` internally to decompose the graph. It does not participate in the Design or Implementation phases and does not produce plans, subsystem definitions, or code.

### Phase 1: Design

The parent decomposes work into independent units. For each unit requiring conceptual grounding, the parent invokes `designer` to produce subsystem definitions and relations, then `impl-planner` to produce an implementation plan. Independent units may be planned in parallel.

### Plan Bundles

The parent assembles plans from `impl-planner` into a **plan bundle**. Child agents never see bundles -- each receives one plan.

#### Structure

A plan bundle consists of:

- **One index file** containing:
  - Overview of the change: purpose, intended outcome, scope, and non-goals.
  - References to every sub-plan file by relative path.
  - A fully resolved **execution dependency graph** -- layers, nodes per layer, and dependency edges stated explicitly.
- **One sub-plan file per node** in the dependency graph, each authored by `impl-planner`.

When the work has no internal concurrency, the index and sole sub-plan may collapse into one file. State this explicitly.

#### Storage

Plan bundles live in `.ai/plans/` in the working tree. Each bundle is its own directory under `.ai/plans/` -- one directory per bundle, containing the index and sub-plan files. Never spread a bundle across the top level of `.ai/plans/`.

### Phase 2: Implementation

The parent walks the dependency graph and at each layer spawns parallel `impl` agents -- one per ready sub-plan. Each `impl` agent receives a plan, not a bundle.

When an `impl` agent halts with `BLOCKED`, the parent resolves the uncertainty at the systems level -- invoking `designer` if first-principles decomposition is needed -- and returns control with the resolution attached.

`BLOCKED` is not a failure. It is the implementor surfacing a question that exceeds its role rather than guessing.

### Invariants

- **Phase separation is load-bearing.** Do not let a designer slide into implementation, and do not let an implementor slide into design. If implementation reveals that the plan was wrong, the implementor blocks and the parent re-enters Design.
- **Concurrency is bounded by the dependency graph, not by ambition.** The number of parallel `impl` agents at any layer is exactly the number of independent sub-plans ready at that layer.
- **`designer` is the only route to systems-level reasoning.** The parent and `impl-planner` may invoke it; `impl` may not.
- **The index file is the parent's single source of truth for execution order.** If the dependency graph is wrong, fix the index file; do not work around it in implementation.
