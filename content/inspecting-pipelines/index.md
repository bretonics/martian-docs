---
date: 2017-05-21T17:53:15-07:00
title: Inspecting Pipelines
type: post
---

## Pipestance Layout

`mrp`
All pipestance state generated by `mrp` are stored in the filesystem. There is no separate database or database client required.

`mrp` creates a folder structure that is designed to be easy for humans to navigate. All you need are `cd`, `ls`, and `cat`, and all metadata files are pretty-printed JSON or plaintext in the case of `STDOUT` and `STDERR` logs.

[ WIP - pipestance -> stages -> forks -> chunks ]

`files` folders contain the output files of an individual chunk.

### Metadata
- `_log`
- `_jobinfo`
- `_stdout`
- `_stderr`
- `_errors`
- `_args`
- `_outs`
- `_complete`

## Logging

## Outputs

When a pipestance completes, the formal outputs of the MRO pipeline are copied to the `outs` folder at the top of the pipestance directory. Their original locations inside `files` folders are symlinked to the file in the `outs` folder.

`_uuid` uniquely identifies this pipestance. Useful for integration with other tracking systems.

`_timestamp` reports start and end times of the pipestance.

`invocation.mro` is copied and preserved. `mrosource.mro` contains the fully recursive preprocessed source code that was involved in this pipestance.


## Web Interface

`--uiport=N`

[ WIP - describe web UI ]