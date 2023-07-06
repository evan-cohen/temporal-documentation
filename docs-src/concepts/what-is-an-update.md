---
id: what-is-an-update
title: What is an Update?
sidebar_label: Update
description: An Update is a request to and a response from Workflow Execution.
ssdi:
  - Released in [Temporal Server version 1.21](https://github.com/temporalio/temporal/releases/tag/v1.21.0)
  - Available in the Go SDK since [v1.23.0](https://pkg.go.dev/go.temporal.io/sdk/client?tab=versions)
tags:
  - term
  - updates
  - explanation
---

An Update is a request to and a response from a Temporal Client to a [Workflow Execution](/concepts/what-is-a-workflow-execution).

- [How to develop, send, and handle Updates in Go](/go/updates)

The Workflow must have a function to handle the Update.
Unlike a [Signal](/concepts/what-is-a-signal) handler, the Update handler function can mutate the state of the Workflow while also returning a value to the caller.
The Update handler listens for Updates by the Update's name.

When there is the potential for multiple Updates to cause a duplication problem, Temporal recommends adding idempotency logic to your Update handler that checks for duplicates.

An Update has four phases.

1. **Admission.** The Temporal Cluster first validates Update submissions against the configured resource usage limits.
   For example, limits apply to concurrent requests and requests per second.
   See the [Temporal Platform limits sheet](/kb/temporal-platform-limits-sheet) for more details.
   When this phase is complete, the Platform changes the status of the Update to **Admitted**.
   At this stage, the Platform hasn't yet persisted the Update to the Workflow Execution's Event History or sent it to a Worker.
1. **Validation.** An optional developer provided function that performs request validation.
   This validation code, similar to a [Query](/concepts/what-is-a-query) handler, can observe but not change the Workflow state.
   This means that the validation of an Update request may depend on the Workflow state at runtime.
   If an Update request doesn't pass validation at this stage, the system rejects the request and doesn't record anything in the Workflow Event History to indicate that the Update ever happened.
   The Update processing doesn't proceed to later phases.
   When the Update completes the validation stage, the Platform changes its state to **Accepted**.
   A [WorkflowExecutionUpdateAcceptedEvent](/references/events#workflowexecutionupdateacceptedevent) Event in the Workflow Execution [Event History](#event-history) denotes the acceptance of an Update.
1. **Execution.** Accepted Update requests move to the execution phase.
   In this phase, the Worker delivers the request to the Update handler.
   Like every bit of code in a Workflow, Update handlers must be [deterministic](/concepts/what-is-a-workflow-definition#deterministic-constraints).
1. **Completion.** The Update handler can return a result or a language-appropriate error/exception to indicate its completion.
   The Platform sends the Update outcome back to the original invoking entity as an Update response.
   A [WorkflowExecutionUpdateCompletedEvent](/references/events#workflowexecutionupdatecompletedevent) Event in the Workflow Execution Event History denotes the completion of an Update.