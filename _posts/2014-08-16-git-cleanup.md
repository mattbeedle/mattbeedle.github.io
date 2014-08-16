---
layout: post
title: "Git Cleanup"
category: git
description: "A Quick Git Alias to Cleanup Merged Branches"
keywords:
    - Git
    - Git Cleanup
    - Delete merged Git branches
---

My normal development workflow:

- Create a new feature branch on my local machine for whatever project I am
  working on
- Build the feature (testing first!)
- Push the working feature to GitHub
- Create a pull request
- Pull request is reviewed/signed off
- Click the "Merge" button on GitHub and then "Delete branch"
- Checkout master on my local machine and pull the latest master from GitHub
- Repeat

I should probably remember to always delete the feature branch on my local
machine after it's merged on GitHub. I never manage this. Sometimes I don't do
this for a very long time and end up with 30+ branches. Then I don't remember
which ones are merged so I don't know which ones I can delete. Things get messy.
So I wrote a quick [Git Alias](http://githowto.com/aliases) to help me out. To
create the alias run the following command from your terminal:

```bash
git config --global alias.cleanup '!git checkout master && git branch --merged | grep -v \"*\" | xargs -n 1 git branch -d'
```

This alias will automatically delete all branches which have already been merged
with master. Just run it from the root of your Git project.

```bash
git cleanup
```

I hope this helps simplify your life as it did mine.
