### Change the commit author of all commits while keeping the original commit date
```sh
git rebase --root --exec 'git commit --amend --no-edit --reset-author --date="$(git log -n 1 --format=%aD)"'
```

### Remove the signed-off-by signature
```sh
git rebase --root --exec 'git commit --amend --no-edit --reset-author --date="$(git log -n 1 --format=%aD)" -m "$(git log -n 1 | tail -n +5 | head -n -1)"'
```

### Sign off the commits again
```sh
git rebase --root --exec 'git commit --amend --no-edit --reset-author --date="$(git log -n 1 --format=%aD)" -s'
```
