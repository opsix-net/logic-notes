# From Flexible JSON to Reliable Workflows: Make Data Dependencies Explicit

A JSON file arrives from an API, and the first record looks tidy. The next one has an optional field missing. A third contains a nested object where your script expected a string. Before analysis can begin, someone has to inspect the shape, decide what the data means, and make a series of transformations in the right order. That work is easier to trust when its dependencies are visible rather than buried in a long script.

This article connects two practical ideas: anticipate irregularities in semi-structured data, and represent a multi-step workflow as a directed graph. A topological ordering of that graph gives a valid execution sequence when the dependencies contain no cycle. It does not clean data for you, but it can help a team plan and check the sequence of cleaning and analysis tasks.

## Flexible data needs deliberate first checks

The selected section of *Python Data Cleaning Cookbook* introduces work with HTML, JSON, and Spark data. It describes the growing need to handle semi-structured formats, including JSON, and notes that related concepts can extend to XML and NoSQL stores. It also points to common issues in web scraping and to distributed processing, such as Apache Spark, when data volume exceeds what is practical on local resources. Its recipe list includes importing simple and more complicated JSON, importing data from web pages, working with Spark data, persisting JSON, and versioning data.

The section’s JSON discussion makes a useful point: flexibility is both a strength and a source of messiness. JSON supports readable, widely consumable data with structures that are not restricted to a single table shape. A file can contain keys with different structures, or a key can appear in one record and not another. So successful parsing is not the same as successful interpretation.

A practical first pass asks questions such as these:

- Is the top-level value an object, an array, or something else?
- How many records are present, and are they all the same kind of object?
- Which keys occur, and which are optional?
- Do values have the expected types and units?
- Are nested values consistently shaped?
- Is the source’s missing-value convention distinguishable from a legitimate value?

These checks are not a substitute for domain knowledge. For example, a missing measurement might mean “not collected,” while a zero might be a real measurement. Silently treating them as equivalent can change downstream results. Likewise, flattening a nested object into columns is a modeling decision, not merely a parsing trick.

## Turn task order into a graph

Suppose a research team receives JSON records and wants to publish a cleaned dataset. The work might include validating the file, normalizing records, checking identifiers, producing a quality report, and exporting a final dataset. Some tasks can run independently. Others cannot begin until prerequisites finish.

Represent each task as a node. Draw a directed edge from a prerequisite to the task that depends on it. If task `normalize` requires `validate`, the edge is `validate -> normalize`. A directed acyclic graph, or DAG, is a graph with no directed cycle. A topological order is a sequence of its nodes in which every prerequisite appears before the task that depends on it.

For example, consider this dependency mapping:

```json
{
  "validate": [],
  "normalize": ["validate"],
  "quality_report": ["normalize"],
  "identifier_check": ["validate"],
  "publish": ["quality_report", "identifier_check"]
}
```

A list under a task names that task’s dependencies. Here, `normalize` depends on `validate`; the other entries follow the same rule. One valid order is `validate`, `identifier_check`, `normalize`, `quality_report`, `publish`. Another valid order may place `normalize` before `identifier_check`. Both respect every dependency.

The graph has five nodes and five dependency edges: one each into `normalize`, `quality_report`, and `identifier_check`, plus two into `publish`. Checking the proposed order, `validate` precedes both `normalize` and `identifier_check`; `normalize` precedes `quality_report`; and both `quality_report` and `identifier_check` precede `publish`. Thus, all five edges point forward in the sequence. The arithmetic is small but useful: a node count of five confirms that every named task appears, while checking each of the five edges confirms the ordering constraints.

A topological order is generally not unique. The utility documented below makes its choice deterministic: whenever multiple tasks are currently available, it selects the lexicographically smallest. That helps produce repeatable output for review and automation. It does not mean that this order is faster or scientifically preferable. If tasks have no dependency between them, their relative ordering is a convention, not a discovery about the data.

## A cycle is a planning signal

Now imagine changing `validate` so it depends on `publish`. The original edges already lead from `validate` through the checks to `publish`; adding the reverse dependency creates a cycle. No task in that cycle can be first while still satisfying every dependency. A topological ordering cannot include all nodes, so an orchestrator should not pretend the workflow is ready to run.

Cycles can reveal an incorrect dependency, or a genuine feedback process modeled with the wrong abstraction. If a quality check triggers a new cleaning pass, for instance, that may be an iterative loop with a stopping condition, not a one-time DAG. Model that loop explicitly in a workflow system rather than trying to force it into an acyclic graph.

A cycle detector also cannot decide whether the dependencies themselves are correct. If a file is mistakenly declared independent of the validation step, a valid ordering can still be produced. Human review, tests, and domain checks remain essential.

## Where this helps in practice

For a data-cleaning pipeline, explicit dependencies make it easier to keep validation before normalization and export. For a web-scraping workflow, fetching pages, parsing HTML, checking extracted fields, and persisting results can be separate tasks with visible prerequisites. For a scientific workflow, a derived measurement should not be generated before its source data has passed the required checks. In build systems, compilation or packaging tasks likewise depend on inputs being prepared first.

This is a connection between the chapter’s subject and a software engineering practice, not a claim that the book specifies a dependency-ordering utility. The section motivates careful handling of varied sources and processing environments. A dependency graph is one practical way to organize the work that follows, particularly when a process spans scripts, people, machines, or scheduled jobs.

At larger scale, the graph still describes logical prerequisites, not resource allocation. It does not say how many workers to use, how to retry a failed task, how to version an input, or how to preserve intermediate data. Those concerns need additional orchestration and data-management policies. Spark can distribute computation, but distribution does not remove the need to understand input shape or task dependencies.

## Assumptions and common pitfalls

The basic model assumes a finite set of named tasks and fixed prerequisite relationships. Every listed dependency means “must be completed before this task,” not “would be convenient to run first.” Task names should be stable and meaningful. If a dependency refers to a task that has no explicit entry of its own, some tools may treat that name as an implicit node; check the specific tool’s behavior before relying on it.

A graph cannot validate the contents of JSON, determine whether a scraped page changed its markup, or establish that a transformation preserves scientific meaning. It orders tasks only. Also distinguish a task that is absent from a configuration from a task that has run and produced no output. Those states have different operational meanings.

Finally, do not confuse deterministic ordering with deterministic computation. A stable task sequence does not guarantee stable outputs if upstream data changes, code is nondeterministic, or external services return different results. Record inputs and versions when reproducibility matters.

## Exercises with short answers

1. In the example graph, can `identifier_check` run before `normalize`? **Answer:** Yes. Neither depends on the other; both require `validate`.
2. If `publish` depends only on `quality_report`, what check has been removed? **Answer:** The explicit requirement that `identifier_check` finish before publishing.
3. Does finding a valid order prove that the JSON records are clean? **Answer:** No. It proves only that the declared task dependencies are acyclic and can be ordered.

The useful habit is to make assumptions inspectable. First inspect the data’s shape. Then state what each processing step needs. A small graph cannot solve every data-quality problem, but it can stop one avoidable class of mistake: running a task before its declared prerequisites are ready.

---

Published 2026-10-05.

**Source:** Python Data Cleaning Cookbook, section **2 Anticipating Data Cleaning Issues When Working with HTML, JSON, and Spark Data**.

This is an original explanatory note; the source book is not redistributed.
