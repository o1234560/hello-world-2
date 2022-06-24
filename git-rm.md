# git rm 命令使用

git 本地数据管理，可以分为三个区：

- 工作区（Working Directory）：是可以直接编辑的地方。
- 暂存区（Stage/Index）：数据暂时存放的区域。
- 版本库（commit History）：存放已经提交的数据。

## 1.1 rm 命令

**作用：** 删除工作区的文件。

执行删除命令：

```
$ rm test.txt
```

查看状态：

```
$ git status
```

rm 命令只是删除了工作区的文件，并没有将这次删除记录到暂存区，也没有删除版本库的文件。

## 1.2 git rm 命令

**作用：** 删除工作区文件，并将这次删除记录到暂存区。

**作用对象：** 已经在版本库中，且没有修改过的文件。

执行删除命令：

```
$ git rm test.txt
```

## 1.3 git rm -f 命令

**作用：** 删除工作区文件和暂存区文件，并将这次删除记录到暂存区。

**作用对象：** 已经在版本库中，且修改过的文件。

执行删除命令：

```
$ git rm -f test.txt
```

## 1.4 git rm --cached 命令

**作用：** 删除暂存区文件，保留工作区文件，并将这次删除记录到暂存区。

**作用对象：** 已经在版本库中，且修改过的文件。