---
id: how-to-spawn-a-workflow-execution-in-go
title: How to spawn a Workflow Execution in Go
description: Use the `ExecuteWorkflow()` method on the Go SDK `Client`, which is available via `NewClient()` in the `go.temporal.io/sdk/client` package.
tags:
  - developer-guide
  - go
---

import RelatedReadList from '../components/RelatedReadList.js'

To spawn a Workflow Execution, use the `ExecuteWorkflow()` method on the Go SDK [`Client`](https://pkg.go.dev/go.temporal.io/sdk@v1.8.0/client#Client), which is available via [`NewClient()`](https://pkg.go.dev/go.temporal.io/sdk@v1.8.0/client#NewClient) in the [`go.temporal.io/sdk/client`](https://pkg.go.dev/go.temporal.io/sdk@v1.8.0/client) package.

The Go SDK Client should never be used inside a Workflow Definition.
To spawn a Workflow Execution from within another Workflow, use the [`ExecuteChildWorkflow`](https://pkg.go.dev/go.temporal.io/sdk/workflow#ExecuteChildWorkflow) API.
For more information, see [How to spawn a Child Workflow Execution in Go](/docs/content/how-to-spawn-a-child-workflow-execution-in-go).

The `ExecuteWorkflow()` API call requires an instance of [`context.Context`](https://pkg.go.dev/context#Context), an instance of [`StartWorkflowOptions`](https://pkg.go.dev/go.temporal.io/sdk@v1.8.0/client#StartWorkflowOptions), a Workflow Type name, and all variables to be passed to the Workflow Execution.
The `ExecuteWorkflow()` call returns a Future, which can be used to get the result of the Workflow Execution.

```go
package main

import (
  // ...

  "go.temporal.io/sdk/client"
)

func main() {
  c, err := client.NewClient(client.Options{})
  if err != nil {
    // ...
  }
  defer c.Close()
  // ...
  workflowOptions := client.StartWorkflowOptions{
    ID: "Your-Custom-Workflow-Id",
    TaskQueue: "your-task-queue",
  }
  workflowRun, err := c.ExecuteWorkflow(context.Background(), workflowOptions, YourWorkflowDefinition, param)
  if err != nil {
    // ...
  }
  // ...
}

func YourWorkflowDefinition(ctx workflow.Context, param YourWorkflowParam) (YourWorkflowResponse, error) {
  // ...
}
```

Start the preceding process by running `go run <filename>.go`.

Notice that the Task Queue name is the same as the name provided when a Worker is created.

<!--
<RelatedReadList
How to customize the name of the Workflow Type?#?g"
/>
-->

The only field, of the `StartWorkflowOptions` instance, that requires a value is the `TaskQueue`.
A Task Queue name is also provided to the Worker that is registered to execute that particular Workflow Type.
The Task Queue name must be the same for both.

We recommend supplying your own custom Workflow Id that can be used to get the result of the Workflow Execution asynchronously at another point in time.
A custom Workflow Id is intended to correspond to a business-level identifier.

<!--
<RelatedReadList
readliststring="What are Workflow Execution Options?#?e|
How to start a Worker?#?g|
When to care about Task Queues?#?g"
/>
-->

By default, the Workflow Type name is the same as the function name.
If the invocation process has access to the function directly, then the Workflow Type name parameter can be passed as if the function name were a variable, without quotations.

If the invocation process does not have direct access to the statically defined Workflow Definition, for example, if the Workflow Definition is in an un-importable package, or it is written in a completely different language, then the Workflow Type can be provided as a `string`.

```go
workflowRun, err := c.ExecuteWorkflow(context.Background(), workflowOptions, "YourWorkflowDefinition", param)
```

In Go, the name of the Workflow Type can be customized when the Workflow Definition is registered with a Worker.

<RelatedReadList
readlist={[
["What is a Workflow Execution?","/docs/content/what-is-a-workflow-execution","explanation"],
]}
/>

<!--
<RelatedReadList
readliststring="How to develop a Worker Process in Go?#?g|
What is a Workflow Type?#?e|
How to build a polyglot application?#?t"
/>
-->