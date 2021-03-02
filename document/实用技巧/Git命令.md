# Git命令

删除所有本地分支

```bash
git tag -l | xargs git tag -d
```

删除远程所有分支

```bash
git tag -l | xargs -n 1 git push --delete origin
```

推送所有本地tag到远程

```bash
git push origin --tags
```

