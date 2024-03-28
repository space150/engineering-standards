[back to table of contents](readme.md)

(_this document is part of the space150 Engineering Standards & Guidelines_)

# Source Control

## Overview

At space150, source control extends beyond just repositories where we securely store our code. Source control is a core component of the development lifecycle. This section will outline the baseline for our use of source control, and our conventions and exceptions.

Today we utilize **Git** and **GitHub** as our source control platforms of choice. In the future this may change, but this is currently our standard and has been for many years.

## Repositories

Repositories are created by engineering managers or IT. We use the naming convention `client-name-project-name`, to help with the organization of the repositories. Internal space150 projects are named `s150-project-name`.

## Commits

Engineers are expected to commit early and push often, minimally daily, but this should be a frequent activity throughout daily coding. "Git only takes full responsibility for your data when you commit"... and push. In other words, your work is not complete unless you commit **and** push to the remote repository. Many engineers may feel the urge to only commit when their code is "pretty"; resist this temptation! If you want to "hide" the ugly bits of your development process, then read more on "sausage making" in [Seth Robertson's document](https://sethrobertson.github.io/GitBestPractices/#sausage).

Frequent commits provide peers, managers, and project managers insights into work completed. Frequent, small commits also allow fellow engineers on team projects to test merges and hopefully head off tedious or massive divergence in branches later on.

Commit messages should provide a useful and concise summary of what the change contains and why it was necessary. Committing changes frequently helps create good commit messages because it keeps the changes small and thus easy to describe. A good commit message looks like this: `git commit -m 'stubbed in the callback function with placeholder variables'`. An unacceptable message looks like this: `git commit -a 'I got it working'`. As exciting as it was for this engineer to get their code working, this type of message does not provide what the feature was or why the edits were made.

### **Dos and Dont's**

- Do commit and push frequently
- Do provide useful commit messages
- Do ensure all of your files are added with your commit
- Don't commit garbage or empty messages
- Don't group unrelated changes into the same commit

## Branching Strategy / Workflow

This section outlines our baseline for branching expectations and strategies. It should be noted that not all projects are created equal, and projects may diverge in workflow on what is needed for branches and environment alignment. The following are the mandatory basics.

`main` - This branch is the default and should be utilized for the highest level release tagging and release management. The latest commit in `main` should always be what is currently running on production. Engineers should never commit directly to `main` other than first initialization. In legacy projects `main` might be called `master` which was Git's original default name.

`staging` - When present, this branch indicates that there is a staging environment, and that code should be merged from `dev` to `staging` and tested prior to being merged into `main`. The latest commit in `staging` should always be what is currently running on the staging environment.

`dev` or `development` - This is where your day-to-day work ends up. If the work is small and concise, you can commit your changes directly to the `dev` branch. Larger feature work should be done in a dedicated branch describing the feature (e.g. `git checkout -b ratings-and-reviews`) and merged into `dev` when in a functional state. If there is a dedicated development environment in the project, the latest commit in `dev` should always be what is currently running there. Test releases are often generated from this branch.

## Pull Requests

Pull requests or PRs for short, are the facility for merging engineers' work into another branch. Typically we are merging into `dev` but a PR can be requested from one branch to any other. PRs provide a workflow for us to review an engineers work, provide constructive feedback and spot any potential issues with code before it moves into QA. It provides a good moment for secure coding review.

It is expected that all engineers create PRs for their work whether or not there are other engineers on their particular project; this provides a mechanism for peer reviews even when working on solo projects.

As an engineer, you might be requested to review a PR. Reviews are a critical component to quality code and project success, so please review PRs thoughtfully, methodically, and in a timely fashion. Make them a priority on your daily task list and try not to skip them just "because there are other engineers on it"; your feedback was solicited for a reason.

### **Dos and Dont's**

- Do provide a description with your pull request that describes what the branch includes along with any helpful notes and requests for reviewers.
- Do choose varied reviewers. Try not to just use the same reviewers for every PR. Give other engineers a chance to look at the codebase; it's likely that you'll both learn something new!
- Don't put people down in PR review comments. Feedback should be constructive and helpful; we're all always learning new things!

### Typical Pull Request Workflow
1. **Create a new branch** of `dev` for your feature and commit+push your code changes to that branch. When your feature is ready...
1. **Create a pull request.** log into GitHub and create a new pull request for merging your branch back into `dev`.
1. **Give a brief description** about your PR. Link to any Trello/JIRA issues that it solves, and note any particularly gnarly or interesting edits. Describe why the feature was necessary. Outline the approach that you took. If you have specific things or questions you want reviewers to look at, ask them here!
1. **Assign reviewers.** We typically try to assign two reviewers to a pull request. Generally try to pick peers who are knowledgeable with the codebase you're working on, but getting a fresh set of eyes is good too! Make sure to give interns & junior developers an opportunity to review your changes as well; pull requests are great learning experiences!
1. **Submit your pull request.** If time is of the essence, please reach out directly to your reviewers and let them know a hot request is coming in.
1. **Give your reviewers time to review.** We consider 24 hours to be a reasonable turn-around time for reviewers to look at a PR, dig in, and give feedback. For those hot requests, try to give your reviewers at least an hour to give feedback.
1. **Work through any comments or updates** that reviewers suggest. Start a conversation if you have questions or disagree with reviewer feedback -- don't just accept feedback blindly! Try not to be offended if reviewers ask you to change something; no codebase is perfect, we're all here to learn!
1. **Make any necessary edits** directly in your branch and commit+push to GitHub as you normally would; the pull request will automatically bring in your changes. Notify your reviewers after making changes, as GitHub does not automatically notify reviewers when changes are made.
1. **Merge the pull request back to the `dev` branch** after the PR is approved by all reviewers. In most cases, you can simply log into GitHub, browse to your PR and click that green `Merge` button at the bottom to close the PR and merge the changes in. If GitHub cannot automatically merge your changes, you'll need to merge the branch locally with Git commands and commit+push.
1. Remember to switch your local Git repository back to the `dev` branch & pull before starting your next work!

### As a Reviewer
- **Be constructive with your feedback!** Never put people down in PR review comments; we're all always learning!
- **Praise areas that you like!** Positive reinforcement locks in good behaviour.
- **Look at general syntax & formatting.** Do the changes match the style of the rest of the application?
- **Are there any snippets that can be improved?** For example, if you find some code doing a `for loop` to convert an array of model data to an array of view-model data, suggest using a `map` function instead of looping over the items manually.
- **Is everything [as secure as it needs to be](./secure-coding.md)?** Watch out for `http://` references and API endpoints without authentication+authorization!
- **Do the changes follow our [coding principles](./coding-principles.md)?** Are functions small & single-purposed? Are variables well-named?

## Tagging

Git tags should be used for marking project releases and milestones so that the rest of the team can track the history of the project. Tags should be considered immutable. Depending on the project type it may make sense to use version numbers, or it may make sense to use a marketing feature name. In either case, be consistent within the project.

## Multiple Repositories

Our standard is to keep everything in the same repository ([monorepo](https://en.wikipedia.org/wiki/Monorepo)), but create directories for each concern. This saves time for engineers maintaining the project due to the large number of clients and repositories we maintain. On occasion, a project may make sense to split up into several repositories but try to avoid this if reasonable.

If a client shares the exact same codebase in their own Git repository, then `git-submodules` or `remotes` should be used.

## Security

- Credentials, secret keys, or environment specific security information should never be added or committed to a repository. We use environment variables for this purpose and the security credentials should be stored in Bitwarden in the respective client collection. space150 IT controls access to these credentials.
- Access to repositories is controlled by engineering managers or IT.

## Exceptions

At times, a client agreement may dictate that an alternative source control system and/or standards be used away from our standards here. Your manager will review these cases with you as they arise.
