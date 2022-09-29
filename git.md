## git Git

### 分支命名规范

规范：`MMDD-type-do_what`

即：`分支创建的日期（月日）-创建该分支的修改类型（feat/fix 等）-简述主要改动`

如：`1207-fix-img_bug`

> 如果是多人同时开发一个需求，可以在后面加上`-your_name`以作区分
>
> 如：`1207-fix-img_bug-alex`

## 常用命令

### 分支

#### 新建分支

```bash
git checkout -b branch_name # 本地新建分支
git push -u origin branch_name # 提交到远程
```

#### 删除分支

```bash
git branch -D branch_name # 本地分支删除
git push origin :branch_name # 远程分支删除
```

### 标签

```bash
git tag # 列出（当前分支）标签
git tag -l 'v0.1.*' # 列出符合模式的标签


git tag -a tag_name -m "tag_comment" # 创建附注标签，默认在head上，可以在最后加上指定commit-ish
git tag -a tag_name -m "tag_comment" -f # 如果标签已存在，强制更新标签（尽量针对特殊名称的tag）


git show tag_name # 查看相应标签的版本信息，并显示打标签时的提交对象


git push origin tag_name # 提交标签
git push origin tag_name -f # 如果服务器端标签已存在，强制提交并更新标签（尽量针对特殊名称的tag）
git push origin --tags # 提交所有标签


git tag -d tag_name # 删除标签（尽量避免）
git push origin :refs/tags/tag_name # 删除服务器端标签（尽量避免）


git checkout tag_name # 切换到标签
```



### Case1

首次创建并切换分支提交

```bash
npm i git-cz -g
git reset HEAD-1# 回退
git stash #缓存到工作区
git checkout -b 0928-proFilterTable_uiChanges--xufei
git branch #查看分支
git branch -a #查看远端分支
git stash pop #从工作区提取出来
git add .
git cz #选fix, 合并代码选chore
git push

#上线gitlab => create a merge request => 把master换成dev2 => 合并
```

### Case2

代码冲突: 原因是没有及时pull下来

```bash
git checkout #检查分支
git merge origin/feature-dev-2 #合并分支到dev2会自动pull
git add .
git cz #合并选chore
git push
#上线gitlab => create a merge request => 把master换成dev2 => 合并
```





所有tab下的表格只能选择一个，切换tab要保存选中的状态
