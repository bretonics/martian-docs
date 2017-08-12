---
date: 2017-05-21T17:53:15-07:00
title: Running Pipelines
type: post
---

## Invoking a Pipeline

Thus far we have shown how to **define** stages and pipelines in MRO files. To **invoke** a pipeline, write an MRO file containing a pipeline `call` statement with the desired input arguments. This `call` statement is called an **invocation**. To invoke the example pipeline from above:

`invoke.mro`
~~~~
@include "pipeline.mro"

call DUPLICATE_FINDER(
    unsorted = "/home/duplicator_dave/unsorted.txt",
)
~~~~

Typically, an invocation MRO file contains a single `@include` statement that causes the pipeline definition to be included, and a single `call` statement of that pipeline. It is generally discouraged to `call` a pipeline in the same file in which it is defined, because then the pipeline definition cannot be easily reused for other invocations with different input arguments.


## Running mrp

`mrp` is the Martian runtime executable that runs pipelines. When a pipeline is run, the instantiation of it is called a **pipestance**, which is a portmanteau of "pipeline" and "instance".

To start a run, pass the invocation MRO file as the first argument to `mrp`, plus a unique name for this pipestance.

~~~~
$ mrp invoke.mro piperun1
Martian Runtime - 2.2.0

Running preflight checks (please wait)...
2018-01-02 14:23:52 [runtime] (ready)           ID.piperun1.DUPLICATE_FINDER.SORT_ITEMS
2018-01-02 14:23:53 [runtime] (split_complete)  ID.piperun1.DUPLICATE_FINDER.SORT_ITEMS
2018-01-02 14:23:53 [runtime] (run:local)       ID.piperun1.DUPLICATE_FINDER.SORT_ITEMS.fork0.chnk0.main
~~~~

At a high level, `mrp` does the following to run the pipeline:

- Parse and validate MRO file (e.g. invoke.mro)
- Convert the MRO into a graph representation of the pipeline
- Create a directory for the pipestance with the unique run name provided (e.g. piperun1)
- Begin evaluating dependencies and executing the stages of the pipeline
- Continuously monitor stages and advance through the pipeline graph when dependencies are satisfied

### mrp Options

[ WIP - mrp options ]

## Completion and Failure

If the pipestance encounters no errors while running, `mrp` exits with status 0 and writes a `_complete` file in the top level of the pipestance directory.

If the pipestance encounters does an encounter an error, `mrp` exits with status 1. The failed stage(s) will contain an `_errors` file with information about the error.

For more details about how to examine an in-progress, completed, or failed pipestance, see [Inspecting Pipelines](../inspecting-pipelines).

## Restarting

While a pipestance fails, it can be restarted by running `mrp` with the same arguments as before. `mrp` will identify the failed stages, and reset them so that run again from a clean start. Stages that have already completed successfully will not be reset or re-run.