# test_public_repo

**DO NOT USE - TESTING PURPOSES ONLY**

Test Git strategies for developing in a public repository, based on this 
[StackOverflow answer](https://stackoverflow.com/a/30352360).

## General reasoning

We need to determine if there is a feasible way of developing new code in feature branches that may be subject to 
history rewriting **without** impacting users who fork our public repositories. History rewriting actions include 
rebasing, squashing, and fixing up Git commits.

## Testing methodology

Several methods have been considered, and this repository will test development in a private fork of a public Git 
repository. The steps are:

1. Create a test public Git repository (this repository);
2. Create a private fork of Step 1 using the steps outlined [here](https://stackoverflow.com/a/30352360);
3. Add some commits to Step 1;
4. Add some commits to Step 2;
5. Use the steps outlined [here](https://stackoverflow.com/a/30352360) to rebase the private fork; and
6. Merge in changes from Step 5 into Step 3, using ths steps outlined [here](https://stackoverflow.com/a/30352360).

## Example files

[Step 3](#testing-methodology) involves adding two dummy Python files, [`foo.py`](foo.py), and [`bar.py`](bar.py), as 
well as adding this section in the [`README.md`](#example-files).

This is used to simulate changes made by other developers that have been accepted and merged into the `master` branch 
of this public Git repository.
