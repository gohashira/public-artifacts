# CLAUDE.md

## SUBSYSTEM.md Convention

`SUBSYSTEM.md` files are conceptual contracts that describe what a subsystem IS and WHY it exists. They are placed in the directory where the subsystem's business logic lives.

They contain zero code implementation details. No variables, functions, file paths, package names, or implementation specifics. Only the conceptual grounding.

### Format

Every `SUBSYSTEM.md` contains exactly four fields:

#### name

The role this subsystem plays in the larger system. Philosophical, not implementational.

#### definition

The whys — the concept this subsystem embodies, what invariant it upholds, what purpose it serves. This is the most important field. It answers: why does this subsystem exist? What would be lost if it didn't?

#### inputs

Inbound directed relations — kinds of meaning this subsystem conceptually depends on from its environment.

#### outputs

Outbound directed relations — kinds of meaning this subsystem conceptually provides to its environment.

#### Relation format (for inputs and outputs)

Each relation has two fields:
- **name**: A semantically precise label for the kind of meaning that flows. Concept-level, never function-level.
- **description**: What the meaning is and why it must flow. Never how it is transmitted.

### How to use SUBSYSTEM.md files

- Treat each `SUBSYSTEM.md` as a pre-declared node with its conceptual contract already specified.
- Read the relevant `SUBSYSTEM.md` before working on any code within that subsystem. Use it to bootstrap understanding rather than re-deriving the subsystem's purpose from code.
- Nesting in the directory tree mirrors recursive decomposition — a `SUBSYSTEM.md` at a deeper level describes a node within its parent subsystem.
- When analysis concludes a subsystem should be created, split, merged, or removed, update the corresponding `SUBSYSTEM.md` files.
- Never copy code-level details into a `SUBSYSTEM.md`. If you find yourself writing a function name, variable, file path, or package name — stop. Restate at the conceptual level.

### Authoring

`SUBSYSTEM.md` files are produced by the thinker agent. It reasons from first principles — decomposing a system into nodes, relations, and invariants — and is the appropriate tool for deriving or revising subsystem boundaries. Do not author or substantially revise a `SUBSYSTEM.md` without invoking the thinker agent to ground the changes in systems-level reasoning.

### Conceptual ripple awareness

Subsystems are connected through their inputs and outputs. When editing code, be aware of which subsystem you are operating within and which subsystems it relates to. If a change alters what a subsystem conceptually provides (outputs) or depends on (inputs), the `SUBSYSTEM.md` for that subsystem — and potentially for its neighbors — must be updated to reflect the new conceptual contract. A code change that shifts a subsystem's inputs or outputs without a corresponding `SUBSYSTEM.md` update leaves the conceptual grounding out of sync with reality.
