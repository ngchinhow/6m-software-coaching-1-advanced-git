# Advanced Git
## Setup
- Clone from this repository [link](https://github.com/ngchinhow/6m-software-coaching-1-advanced-git.git).
- Use Git Bash instead of PowerShell/Command Prompt if using Windows.
- Use `git status` and `git log` if confused about where you are in the commit history.

## Scenario 1 - Regular GitHub Flow
1. Create a new feature branch in local.
```
    git checkout -b <feature branch>
```
2. Make some edits to EDITME.md.
3. Save, add and commit your changes.
```
    git add . & git commit -m "<your message>"
```
4. Push your changes.
```
    git push origin <feature branch>
```
5. Raise a pull request.

## Scenario 2 - Working on main
Imagine if a new feature branch (step 1) wasn’t created in a regular GitHub Flow.

***

1. Move back to main branch
```
    git checkout main
```
2. Make some edits to EDITME.md. These changes **must** be different from the ones in the feature branch.
3. Save, add and commit your changes.
```
    git add . & git commit -m "<your message>"
```
4. Push your changes.
```
    git push origin main
```

## Scenario 3 - Divergent branches
The changes in main are important; they need to be moved to the feature branch. There are a few ways that can be done, each with their pros and cons.

***

### A. Resolution by merging
1. Move back to your feature branch.
```
    git checkout <feature branch>
```
2. Merge main into the feature branch. Resolve the conflicts.
```
    git merge main
```
3. Save, add and commit the resolution.
```
    git add . & git commit -m “<your message>”
```

While this method is relatively easy, it breaks the linear history of the feature branch, which makes it harder for both the developer himself and other developers on the team to track the history of the commits. Because of the 3-way nature of merges, they make commit histories more complicated and harder to follow. As much as possible, developers try to have linear commit histories.

***

### B. Resolution by stashing
To avoid going too deep into Git, `git stash` will not be covered in this lesson. However, on a conceptual level, what one would do is:

1. Soft reset commits on main.
2. Stash changes on main.
3. Switch over to feature branch.
4. Unstash changes on feature branch.
5. Commit changes as usual on feature branch.

As can be seen, the steps are quite involved, however, the commit history is now linear.

***

### C. Resolution by rebasing
In terms of maintaining a linear commit history, `git rebase` is the most powerful tool to achieve it. In order to get back the divergent branches to see that in action, the merge performed earlier has to be undone (this is performed on feature branch).
```
    git reset --hard ORIG_HEAD
```

***

1. (Still at feature branch) Rebase feature branch onto main branch.
```
    git rebase main
```
2. Resolve the conflicts. Stage the changes and continue the rebase.
```
    git add . & git rebase --continue
```
3. Confirm the commit message.
4. The commit history for your feature branch is now different between your local and remote. To sync them back together, force push your local to remote.
```
    git push -f origin <feature branch>
```

***

Care must always be taken when using `git rebase`. It is very easy to get confused during the process, especially if multiple conflicts need to be resolved. When in doubt, remember that aborting the rebase is an option, rather than going through with it and resetting after.

## Git in IDEs
Commit histories, when not represented as a graph, is actually quite difficult to understand. Luckily, most IDEs have either in-built functionality or plugins to graph-ify a project's Git history.

For Visual Studio Code, I recommend the [Git Graph Extension](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph).

For the JetBrains suite (IntelliJ IDEA, PyCharm, etc), they come natively with Git graphing support.