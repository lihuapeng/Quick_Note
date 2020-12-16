# Git_Questions

### Q1：分支合并错误

```
fatal: refusing to merge unrelated histories
```

 可能会在`git pull`或者`git push`中都有可能会遇到，这是因为两个分支没有取得关系 

解决方案是在操作命令后面加 `--allow-unrelated-histories `

