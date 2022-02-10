---
title: Pulp Docs SIG Findings & Conclusions
author: Melanie Corr
tags:
  - community, documentation, workflows
---

Over the last few weeks, a few of us have been meeting to discuss Pulp's documentation.

## Problem statement

The Pulp project currently has no user path into the project apart from our user documentation.

The way our documentation is currently organized means that users have to jump back and forth from Pulpcore docs, installation docs, to Plugin docs.

Our Pulpcore documentation provides general content and feature descriptions to remain plugin-neutral. This lack of specificity does not help the users get what they need.  

Our Pulpcore documentation talks about workflows without any indication if they apply to specific plugins or not.

Because of the current state of our documentation, we have blocked off the front door into our project from all but those who are skilled enough install and run Pulp in spite of the status quo.

Our current documentation blocks further growth of our community, which means that we do not get to take advantage of the benefits of being a FOSS project.

### Solution

We need to shift away from documenting general workflows in Pulpcore to specific, tutorial style  workflows for each plugin.

The desired end state would mean that post-installation, the users would have everything they need to work with every workflow of that content type within each set of plugin documentation.

The plugin documentation would become the single source of truth for everything relating to the concept type.

This would require increasing the workflow documentation for each Pulp plugin.

The Pulpcore documentation would be whittled away to provide only high level Pulpcore conceptual docs and high-level generic use cases, and links to where the user can provide specific docs by content type.

### Lessons Learned: Good docs pay dividends

Think of docs as an investment. We can also think of them as a snowball, rolling downhill, getting bigger and bigger.

Whatever time we save in not writing docs, we repay in answering support questions from the community, supporting colleagues in the projects Pulp feeds into, and in the bugs we need to fix months later when enterprise solutions that integrate Pulp in their products encounter them for the first time during upgrades or in production environments.

### What can we do?

Here are a few ideas for prioritizing docs as part of our everyday workflows.

#### Incorporate workflow docs into feature completion

Instead of simply describing it when a new feature is implemented in a plugin, the developer walks through the user scenario recording the steps end to end, completing the
"As a user, I want to achieve x" workflow, and documents the workflow as part of the closure process for the feature.

#### Look at all future doc work as if it were a recipe.

1. Define what are their needs before the user begins.
2. Write end to end steps to achieve this.

#### Perform a gap analysis of plugin workflows vs pulpcore workflows

As a mini team, look at the workflows defined in Pulpcore and make a list of those available for the plugin you support.

Check whether CLI commands exist for any documentation that references API calls. If so, replace with the CLI.

If any missing workflow docs can be paired with current feature work, use the opportunity to refactor and improve that docset.

#### Your suggestions please

We would love to hear any feedback or suggestions that you might have for us to make the necessary improvements.
