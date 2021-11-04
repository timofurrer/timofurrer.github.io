---
title: "Git Rebase vs. Merge"
date: 2021-11-04T21:27:38+02:00
draft: false
categories: [ "git", "software engineering" ]
gitGraph:
  enable: true
---

In every team I've been part of for the last few years we've always had a debate
sooner or later if we should use a `git merge` or `git rebase` to keep
a branch up-to-date with it's source branch.

This post sheds some light on the pros and cons of both and my opinion on what
you should use for your team.

Before I go on, let's get on the same page what a few terms mean throughout this article:

* **public branch**: a stable branch where changes come together. It's usually a protected
  branch and history rewrites are forbidden. In the simplest workflow this branch is
  often called `main`.
* **non-public branch**: every other branch next to the *public branches*.
  These may be feature branches, bugfix branches, ... you name them.
* **source branch**: the branch from where another branch was created from.
  Most often this is a *public branch*.
* **target branch**: the branch where another branch should be merged into.
  Most often this is a *public branch* and the same as the *source branch*.

**tl;dr:** in the summary [here](#summary).

## The Situation

When we develop a change, be it a new feature or a a bugfix, we usually create a dedicated
short living branch for this particular changeset.
Let's assume we created a new feature branch called `feature/spaceship` based on the `main` branch.
The goal is to implement that *spaceship* and eventually merge it back into the `main` branch.

Developing a *spaceship* certainly takes some time and our colleagues already continued merging
other changes into `main` in the meantime. One particular feature, the *engine framework*
is especially interesting for us and we need to get these changes somehow
into our `feature/spaceship` branch.

{{< gitgraph id="baseline" title="Situation with new commits on main" >}}
const main = gitgraph.branch("main");
main.commit("Initial Commit");
main.commit("Add README")
const spaceshipBranch = gitgraph.branch("feature/spaceship");
spaceshipBranch.commit("Implement spaceship blueprint");
main.commit("Implement engine framework")
{{</ gitgraph >}}

To get those new changes from `main` into our `feature/spaceship` branch, git provides us
a variety of options. We'll look at the most promising ones: *merge* and *rebase*.


## Merging changes into feature branch

The most intuitive one may be to perform a *merge*[^1]. However, when performing a *merge*
we'll end up with a new merge commit[^2] on our `feature/spaceship` branch:

{{< gitgraph id="merge-into-feature-branch" title="Merge main changes into feature branch" >}}
const main = gitgraph.branch("main");
main.commit("Initial Commit");
main.commit("Add README")
const spaceshipBranch = gitgraph.branch("feature/spaceship");
spaceshipBranch.commit("Implement spaceship blueprint");
main.commit("Implement engine framework")
spaceshipBranch.merge(main, "Merge updates from main into feature branch")
{{</ gitgraph >}}

We might be okay with that for a single update, but what if we have to merge multiple
times? Every single time we'd get another merge commit, like illustrated here:

{{< gitgraph id="merge-multiple-times-into-feature-branch" title="Merge main changes into feature branch multiple times" >}}
const main = gitgraph.branch("main");
main.commit("Initial Commit");
main.commit("Add README")
const spaceshipBranch = gitgraph.branch("feature/spaceship");
spaceshipBranch.commit("Implement spaceship blueprint");
main.commit("Implement engine framework")
spaceshipBranch.merge(main, "Merge updates from main into feature branch")
spaceshipBranch.commit("Implement spaceship thruster");
main.commit("Improve engine framework")
spaceshipBranch.merge(main, "Merge updates from main into feature branch")
spaceshipBranch.commit("Implement spaceship landing platform");
main.commit("Fix threading in engine framework")
spaceshipBranch.merge(main, "Merge updates from main into feature branch")
{{</ gitgraph >}}

This massively pollutes our `feature/spaceship` branch with unnecessary details about when
we've updated our branch.

This gets especially annoying for reviewers when we want to merge back our `feature/spaceship`
branch into `main`.

In the above figure a reviewer who looks at the change set is often
only interested in the `Implement spaceship blueprint`, `Implement spaceship thruster`
and `Implement spaceship landing platform` commits. However, the `feature/spaceship`
branch at this point contains all the `Merge updates from main into feature branch`
merge commits.

## Rebasing feature branch onto changes

Another option would be to *rebase* our `feature/spaceship` branch onto the latest
version of `main`. A *rebase* always implies that the history of the `feature/spaceship`
branch has to be rewritten. Which in most situation is absolutely fine, because
`feature/spaceship` is a *non-public branch* and no one cares.

Using a *rebase* instead of a *merge* effectively avoids the merge commit and leaves no
traces about the synchronization history of the feature branch with the source branch.

Reusing the first merge example here, and performing a `rebase` instead of a `merge` would
lead to the following graph:

{{< gitgraph id="rebase-feature-branch-onto-changes" title="Rebase feature branch onto main" >}}
const main = gitgraph.branch("main");
main.commit("Initial Commit");
main.commit("Add README")
main.commit("Implement engine framework")
const spaceshipBranch = gitgraph.branch("feature/spaceship");
spaceshipBranch.commit("Implement spaceship blueprint");
{{</ gitgraph >}}

## Summary

The git history is a valuable tool to reason about the work which has been done over time.
Every *non fast-forward merge* introduces a merge commit on the *target branch* which adds
an additional lineage to the git history[^3].

When merging from *public* to *non-public* branches and vice-versa the history becomes
unnecessarily complicated and it becomes hard to reason about it.
In addition, and especially, for active projects and/or longer-living *non-public* branches,
multiple synchronizations with a *public* branch may be performed.
When using merges, this would introduce a merge commit every single time on the *non-public* branch.

**Therefore, a `git rebase` should be used to synchronize the *non-public* branches with *public* branches.**
This ensures a linear history in the *non-public* branch.
When submitting the changes from a *non-public* branch back to a *public* branch use a Pull- or Merge Request
and use an actual merge there.
This will introduce a single merge commit on the *public* branch which is okay,
because the reference to the *non-public* branch is still intact.

[^1]: to a lot of people probably because a `merge` is often introduced first when learning
      git and concepts like Merge / Pull Request usually perform a merge, too.

[^2]: for no fast-forward merges. The merge commit will have two parents,
      one for each of the branches.

[^3]: a new lineage because a merge commit has two parents