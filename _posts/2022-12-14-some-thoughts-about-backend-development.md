---
layout: post
title: Some Thoughts About Backend Development
date: 2022-12-14 10:07 +0800
category: [development]
---
## Trunk-based development

> Description: Each developer divides their own work into small batches and merges that work into trunk at least once (and potentially several times) a day.

### Practices

- Have three or fewer active branches in the application's repository.
- Merge branches to trunk at least once a day.
- Don't have code freezes and don't have integration phases.

### Ways to improve trunk-based development

- Develop in small batches.
- Perform synchronous code review.
- Have a fast build.
- Create a core group of advocates and mentors.

## Brainstorm before Development

We should write different plans about web application before we decide to coding. And we start a brainstorm in our team, each member put different opinions. Finally, we make a approved plan as the final development guideline.

### The Contents of Good Development Plan

- There is a brief outline about the whole plan.
- Use flowchart or architecture chart to explain plan.
- Clear and standardized interface document
- Reasonable scheduling

## Estimate Duplicated Processes and Codes

- Use script to estimate duplicated processes
- Use Code generation cli to estimate duplicated codes
