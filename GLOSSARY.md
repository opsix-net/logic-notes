# Glossary

New concepts are appended as research notes are published.


<!-- publication:2026-10-02 -->

## 2026-10-02

Source note: [2026-10-02](articles/2026-10-02-trading-applications-data-protocols-safe-boundaries.md).

### FIX

Financial Information eXchange, a messaging protocol used to communicate financial information and instructions. It provides conventions for encoding messages, but implementations can differ by protocol version and venue-specific requirements, so support for FIX alone does not guarantee drop-in compatibility.

### Tag

A numeric field identifier in a FIX message, paired with a value in `tag=value` form. Tags identify items such as message type or instrument. Applications should use a clear internal mapping and validate required fields against the relevant protocol and venue rules.

### SOH

Start of Header, the ASCII control byte `0x01` used to separate FIX fields. A printable character such as a vertical bar may represent SOH in documentation, but must not replace the actual byte in a transmitted message.

### BodyLength

The value in FIX tag 9, giving the byte length of the message body: the bytes after the delimiter ending tag 9 and before the start of tag 10. It must be calculated from the serialized byte sequence, including field delimiters.

### Checksum

The value in FIX tag 10, calculated from the message bytes preceding that tag according to the protocol’s checksum rule. It is a transmission-format check, not proof that the message is valid for a venue or that an order is safe or accepted.

### Session

An established exchange of messages between identified sender and target systems. A session includes connection and lifecycle behavior such as logon and logout; exact authentication, recovery, and required message fields depend on the counterparty’s specification.

### Venue adapter

A software boundary that translates internal application concepts into a particular broker’s or trading venue’s interface. It isolates differences in fields, supported order types, and connection conventions from strategy and domain logic.

### Syntactic validity

Conformance to a message’s structural rules, such as field encoding and required formatting. A syntactically valid request may still be semantically inappropriate, unauthorized, unsupported, or outside risk limits.

### Ragged row

A CSV data record whose number of fields differs from the number of header columns. In this utility, such rows are reported by file line number and excluded from per-column statistics because their values cannot be safely aligned to the header.

### Finite numeric value

A value successfully parsed as a number that is neither positive infinity, negative infinity, nor NaN. The CSV profiler includes finite numeric values in counts and summaries, while excluding nonfinite values from those calculations.


<!-- publication:2026-10-05 -->

## 2026-10-05

Source note: [2026-10-05](articles/2026-10-05-flexible-json-reliable-data-workflows.md).

### JSON

JavaScript Object Notation, a text format for representing values such as objects, arrays, strings, numbers, booleans, and null. Its flexibility makes it useful for exchanging data, but records in one file may have different keys or nested shapes, so successful parsing does not guarantee consistent meaning.

### Semi-structured data

Data that has some organization, such as named fields or nested objects, without requiring every record to follow one fixed table schema. JSON and HTML-derived records are common examples. Consumers usually need to inspect and normalize the structure before treating values as consistent analytical variables.

### Data validation

Checks that data and configuration meet declared expectations, such as required fields, types, permitted ranges, or a valid graph shape. Validation can detect specified problems, but it cannot establish that the expectations are scientifically appropriate or that untested data properties are correct.

### Dependency graph

A representation of tasks as nodes and prerequisite relationships as directed edges. An edge from task A to task B means B requires A to finish first. Making these relationships explicit helps teams review execution order and spot missing or contradictory prerequisites.

### Directed acyclic graph (DAG)

A directed graph with no directed path that loops back to its starting node. A DAG admits at least one topological ordering, making it useful for workflows where tasks have prerequisites but no iterative cycles.

### Topological order

A sequence of a graph’s nodes in which each node appears after all of its prerequisites. Such an order exists for a directed acyclic graph and may not be unique. The order satisfies declared constraints but does not by itself optimize runtime or validate the tasks.

### Cycle

A directed path that returns to its starting node. In a dependency graph, a cycle means that tasks in the loop cannot all be scheduled in a one-pass prerequisite order. It may indicate an erroneous dependency or an iterative process that needs a different workflow model.

### Kahn’s algorithm

A topological-sorting method that repeatedly removes nodes with no remaining prerequisites and releases their dependents. If nodes remain when no more nodes are ready, the graph contains a cycle or nodes blocked downstream from one.

### Deterministic ordering

A rule that produces the same sequence from the same graph and implementation choices. Choosing the lexicographically smallest currently ready node makes this utility’s output repeatable, but does not make task results deterministic if inputs or computations vary.

### Workflow orchestration

The coordination of tasks and their dependencies, often including when tasks run and how failures are handled. A topological ordering provides a possible sequence for an acyclic workflow, while production orchestration may additionally require scheduling, retries, monitoring, and provenance.
