---
id: how-to-log-from-a-workflow-in-python
title: How to log from a Workflow in Python
sidebar_label: Log from a Workflow
description: To log from a Workflow in Python, import the logging module `logging` and set your logging configuration to a level you want to expose logs to. Then in your Workflow, set your [`logger`](https://python.temporal.io/temporalio.workflow.html#logger) and level on the Workflow.
tags:
- python
- workflow
- logging
- python sdk
- code sample
---

<!-- DO NOT EDIT THIS FILE DIRECTLY.
THIS FILE IS GENERATED from https://github.com/temporalio/documentation-samples-python/blob/main/your_loggers/your_workflow_dacx.py. -->

You can log from a Workflow using Python's standard library, by importing the logging module `logging`.

Set your logging configuration to a level you want to expose logs to.
The following example sets the logging information level to `INFO`.

```python
logging.basicConfig(level=logging.INFO)
```

Then in your Workflow, set your [`logger`](https://python.temporal.io/temporalio.workflow.html#logger) and level on the Workflow. The following example logs the Workflow.

<div class="copycode-notice-container"><div class="copycode-notice"><img data-style="copycode-icon" src="/icons/copycode.png" alt="Copy code icon" /> Sample application code information <img id="i-c937f69a-9ce0-440a-8971-13acd388119a" data-event="clickable-copycode-info" data-style="chevron-icon" src="/icons/chevron.png" alt="Chevron icon" /></div><div id="copycode-info-c937f69a-9ce0-440a-8971-13acd388119a" class="copycode-info">The following code sample comes from a working and tested sample application. The code sample might be abridged within the guide to highlight key aspects. Visit the source repository to <a href="https://github.com/temporalio/documentation-samples-python/blob/main/your_loggers/your_workflow_dacx.py">view the source code</a> in the context of the rest of the application code.</div></div>

```python
# ...
        workflow.logger.info("Workflow input parameter: %s" % name)
```