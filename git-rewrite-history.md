## Change the commit author of all commits while keeping the original commit date
```sh
git rebase --root --exec 'git commit --amend --no-edit --reset-author --date="$(git log -n 1 --format=%aD)"'
```
