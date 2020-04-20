# Git Workflow
As a software system is evolving, it's useful to be able to understand how this evolution is happening, especially if it's being carried out by more than one instigator.

This document describes best practices; a bit of explanation behind the choices; as well as practical tools and tips for adherence. It's assumed that readers are already familiar with [Git](https://git-scm.com/doc), such that they could vaguely describe most of the commands. The practices discussed will be mostly generalisable to other version control systems.

## Extracting Value from Commits
A legible history is not just useful for managing merge commits − it can also provide insight for project managers; for your fellow programmers; and for your robotic assistants:
* How many lines of code were committed each day? ([Hercules](https://github.com/src-d/hercules))
* How much impact was made from each commit? Which people are making the most impact? ([Gitprime](https://blog.gitprime.com/impact-a-better-way-to-measure-codebase-change/))
* Which commits introduced bugs? Which recent commits follow this pattern and seem like they might be new bugs? ([Gitrisky](https://medium.com/civis-analytics/predicting-code-bug-risk-with-git-metadata-bc0b675ad28f))

If you're trying to implement a feature similar to some existing functionality, sure you can try to copy bits of it, but how does it all fit together? This can be especially tricky in the presence of [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection). If you have a succinct, comprehensible commit message like `Adding autocompletion to the wine detail page`, then you can revisit that commit later, seeing all the changes that needed to be made to support this behaviour.

A good way to find pertinent commits is to use [`git blame`](https://git-scm.com/docs/git-blame), typically available in IDEs with a less combative name, like "[annotate](https://www.jetbrains.com/help/idea/investigate-changes.html)" (JetBrains).

![Annotations in Pycharm](./assets/images/Pycharm_annotations.png)

Going further, let's consider a detailed commit like [this one in Linux](https://github.com/torvalds/linux/commit/6e88abb862898f55d083071e4423000983dcfe63). It doesn't just say what changed; it discusses in prose why it's happening, going so far as to teach the reader. It even includes references.

## Embedding Value in Commits

Like any other stream of engineering, our decisions are best when they're documented. It's not just for auditing, it's to allow ourselves to get into a repeatable process; and because dependencies will inevitably change − maybe we'll end up having access to an [optical computer](https://en.wikipedia.org/wiki/Optical_computing), a [quantum computer](https://en.wikipedia.org/wiki/Quantum_computing) or just a cheaper cloud host.

So at the opposite end of the spectrum to the discussion-piece commit, some common patterns are:
* `Fixing a bug` / `Quick fix` − if we encounter this in the log (or equivalently, the [History page](https://help.github.com/en/desktop/contributing-to-projects/viewing-the-branch-history) of GitHub), then it's going to be meaningless, because we don't know which file changed − the bug could've been anywhere. If we encounter it in a file view (`git blame`), it's more useful, but even then, it gives us a false sense of security − this line must apparently be bug-free! What if there were multiple bugs?

  [What counts as a bug anyway?](https://www.quora.com/Can-you-explain-in-simple-terms-what-a-software-bug-is) Abstractly, it's a mismatch between real vs expected results, but who's to say it's the software's fault that there's a mismatch? The code and the commit history are intended for engineers, not for philosophers − please say which bug you fixed; and ideally, where it was.

* [`Improving code`](https://github.com/django/django/commit/058b38b43ea4726be2914ecc967b8fb1da47d995#diff-e3e2a9bfd88566b05001b02a3f51d286) − like other streams of engineering, there are often trade-offs to any decision. It's rarely clear-cut that one piece of code is *fundamentally better* than something else. Besides, aren't all commits meant to improve the codebase? If your commit vocabulary includes `making things better`, do you balance out the excitement with some commits `making things worse`?

* `Revising as per business requirements` / `Adjusting as per feedback` − commit messages are meant to explain what's happening. What are the business requirements? What was the feedback? If the explanation needs its own explanation, then where does it end?

  ![A stack of turtles](https://upload.wikimedia.org/wikipedia/commons/thumb/4/47/River_terrapin.jpg/170px-River_terrapin.jpg)

  In the same vein is commits like `Revised widget controller as per JIRA-1257`. Sure, there's probably more information in Jira, but even if [you've configured hyperlinks from your repository to Jira issues](https://stackoverflow.com/a/58383541/1495729), a few words reminding readers about the purpose of `JIRA-1247` will go a long way in helping them to stay in the flow; and it will protect against spelling mistakes in codes, that could easily go unnoticed.

  ![Commits in BitBucket with hyperlinks to Jira issues](./assets/images/bitbucket_hyperlinks.png)
