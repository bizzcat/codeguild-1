# Git Branching and Merging
Git lets you keep track of parallel histories from a common source.
Each separate history is called a **branch**.

```
Time --->
               coolfeature
               |
       C11 - C12
      /
C1 - C2 - C3 - C4
                |
                master
```

The main branch of every repository is called `master`.
It should contain all finalized work.

## Making Branches
A branch should be made _before_ you start some speculative work.
This is called a **feature branch**.
```bash
git checkout -b BRANCH
```
`git status` will then show you what branch you're working on.
You can add commits to any current branch with `git commit`.

## Listing Branches
You can swap between branches so separate projects don't interfere with each other.
To list all known branches:
```bash
git branch
```

## Checking Out Branches
Then to swap your _working directory_ to a specific branch:
```bash
git checkout BRANCH
```

This will give you an error if you have un-committed changes in your working directory, since it would overwrite them.

## Merging Branches
Then, once some work is finalized, you can **merge** it back into the `master` branch.
```
Time --->
               coolfeature
               |
       C11 - C12 --\
      /             \
C1 - C2 - C3 - C4 - M1
                     |
                     master
```

To do this:
```bash
git checkout master
git merge BRANCH -m "Merging BRANCH."
```
Merging _creates a commit_, so it needs a message.

## Deleting Branches
After you've successfully merged, delete the speculative branch, if it's fully merged into `master`:
```bash
git branch -d BRANCH
```

## Diff-ing Versions
Your `git diff` [commands](basicgit.md) also work with branch names, in addition to commit hashes.

## Merge Conflicts
If your branch modifies some lines of code that were modified in parallel, when you merge the histories together, you will get a **merge conflict**.

This _pauses_ the merge and lets you manually go in and fix the conflicts.

If you get a conflict, first run `git status`.
This will show you what files are "both modified".

Inside of those files you will see something like:
```python
<<<<<<< HEAD
print('Hello, ' + name + '!')
=======
print('Hello, {}'.format(name))
>>>>>>> branch
```

Those `<<<<<<<` and `>>>>>>>` are called **conflict markers**.
Search for them.
Between the top `<<<<<<<` and the `=======` is the version currently in `master`.
Between the bottom `=======` and `>>>>>>>` is the version in the branch you're merging.

Replace all that with a version of the code that combines the functionality:
```python
print('Hello, {}!'.format(name))
```

**Remove all conflict markers when you're done!**
They're not Python code and won't run.

Now you need to finish making the merge commit.
Run `git add FILE` to mark that you've resolved the conflict.

Once `git status` shows only "changes to be committed" / all green, run `git commit -m"Merge BRANCH"`.

Then remember to delete your finalized branch.

## Basic Branching Workflow
1. Before you start some speculative work, make a feature branch using `git checkout -b BRANCH`.

Now repeat until the speculative work is done:

1. Make changes using your editor to the working directory.
1. Stage those changes in the index using `git add FILES...`.
1. Commit those staged changes to your history of the feature branch using `git commit -m"MESSAGE"`.

Now that the feature branch is done:

1. Checkout `master` with `git checkout master`.
1. Merge in your changes from your feature branch `git merge BRANCH -m "Merging BRANCH."`.
1. Delete the finalized feature branch with `git branch -d BRANCH`.
1. Push those new commits in `master` to remote history using `git push`.

Make a new branch for each practice problem you work on from now on.
