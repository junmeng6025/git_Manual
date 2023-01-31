# Git Manual

## An overview of git workflow
<div align=center>
    <img src="git-workflow-1.png"/><br>
    Fig. overview git workflow.  
</div> 

An official interactivable animation about git commands: [learn git branching](https://learngitbranching.js.org/?locale=zh_CN)  
<br>

***
## 0) Git concept
<details>
    <summary> Click for details </summary>

<div align=center>
    <img src = "git-concept.png" width=300><br>
    Fig. Git concept
</div> 
<br>
Git is a distributed version control tool.  

- All the versions are cloned to user's local PC.
- User can commit the changes offline to local, push them to remote repository when connect to the Internet.
- Causes extra local storage usage, but provides stable development pileline.
</details>
<br>

## 1) Install and configure your Git
<details>
  <summary> Click for details</summary>

```bash
git config --global user.name "user name"
git config --global user.email 123@mail.com
```
Check the config info
```bash
git config -l               # all the config info  
git config --global --list  # user (global) config info
git config --system --list  # system (local) config info
```
The config files are saved locally, wich can be deleted.
### use SSH key
Add SSH key to your git account.  
First check if you already have SSH key on your PC (Ubuntu):
```bash
cat ~/.ssh/id_rsa.pub
```
If not, generate an SSH key
```bash
ssh-keygen
```
Copy and paste to git account.

</details>
<br>  

***
## 2) Play with Git!

<br>
<div align=center>
    <p float="left">
    <img src="layout.png" height="300" />
    <img src="dir.png" height="300" /> 
  </p>
    Fig. Layout of Git repository.  

</div> 
<br>

- **Working Directory:** Your local folder containing the code
- **Stage (Index):** A changelog file, containing the information about to-be-committed changes
- **History:** A temporary storage to save the committed changes
- **Remote Dirtectory:** The git server, the online repository

<br>
<div align=center>
    <img src="workflow.png" width="600" /><br>
    Fig. Workflow of Git.  

</div> 
<br>

- Edit the code/ Create or remove files in local Workspace
- `Add` the changes 
- `Commit` the changes to LOCAL temporary storage
- `Push` the changes to remote repo

### **2.1) Update your changes**
```bash
git add .   # add the changes to temp storage; "." means add all the changes
git commit -m "commit message"  # commit the changes added in temp storage to LOCAL repo
git push    # push the changes to remote repo
```
<details>
    <summary>Commit syntax (CHN):</summary>

referd from [zhihu (CHN)](https://zhuanlan.zhihu.com/p/182553920)  

```text
<type>(<scope>): <subject>
```

**type(必须)**

用于说明git commit的类别，只允许使用下面的标识。

`feat`：新功能（feature）。

`fix/to`：修复bug，可以是QA发现的BUG，也可以是研发自己发现的BUG。

- `fix`：产生diff并自动修复此问题。适合于一次提交直接修复问题
- `to`：只产生diff不自动修复此问题。适合于多次提交。最终修复问题提交时使用fix  
  
`docs`：文档（documentation）。

`style`：格式（不影响代码运行的变动）。

`refactor`：重构（即不是新增功能，也不是修改bug的代码变动）。

`perf`：优化相关，比如提升性能、体验。

`test`：增加测试。

`chore`：构建过程或辅助工具的变动。

`revert`：回滚到上一个版本。

`merge`：代码合并。

`sync`：同步主线或分支的Bug。

**scope(可选)**

scope用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

例如在Angular，可以是location，browser，compile，compile，rootScope， ngHref，ngClick，ngView等。如果你的修改影响了不止一个scope，你可以使用*代替。

**subject(必须)**

subject是commit目的的简短描述，不超过50个字符。
</details>
<br>

To check file status:  
```bash
git status  # OPTIONAL: check the file status
```

<details>
    <summary>There are 4 stauts (CHN):</summary>

- `Untracked`: **未跟踪**
    > 此文件在文件夹中，但并没有加入到git库，不参与版本控制。通过 `git add` 命令将状态变为 `Staged`
- `Unmodify`: **文件已入库但未修改**
    > 被修改则变为 `Modified`；  
    > 被删除变为 `Untracked`
- `Modified`: **文件已修改**，但尚未进行其他操作
    > 通过`git add` 命令将状态变为 `Staged`；  
    > 通过 `git checkout` 从库中取出原来版本覆盖，放弃更改，回到 `Unmodify` 状态
- `Staged`: **暂存状态**
    > 执行 `git commit` 将修改同步到库；  
    > 执行 `git reset HEAD <filename>` 撤销已执行的暂存，回到 `Modified` 状态
</details>
<br>

***
### 2.2) Create your own git project
**1a) Initialize a folder as local repo**
```bash
git init
...
git add .
git commit -m "first commit"
git branch -M main
git remote add origin [http url]
git push -u origin main
```
> With this you'll generate "git things" in your current local folder. This folder is now a git workspace.  

**1b) Clone a remote repo to local**
```bash
git clone [repo-url]
...
git add .
git commit -m "commit message"
git push
```
> With this you can first create a blank git repo, clone to local and then start coding.  

**2) Modify the files**  
Make some changes, then commit them as shown in 2.1)

**3) Git ignore**  
<details>
  <summary> syntax of .gitignore (CHN)</summary>
有些运行必需的文件并不纳入版本控制，例如数据集。在主目录下建立 `.gitignore` 文件：  
- 空行或#开始的行被忽略
- 支持Linux通配符：（*）表示任意字符，（？）表示任意字符，（[abc])表示可选字符范围，（{string1, string2, sring3, ...}）表示可选字符串
- 名称前加 ! 表示例外规则，将不会被忽略
- 名称前加 / 表示要忽略的文件在此目录下，而子目录中的文件不被忽略
- 名称后加 / 表示要忽略的是此目录下该名称的子目录，而非文件

e.g.
```yaml
# 注释
*.txt       # 忽略所有 .txt 类型的文件
!lib.txt    # lib.txt除外
/temp       # 仅忽略项目根目录下的TODO文件，不包括其他目录 temp
build/      # 忽略 build/ 目录下的所有文件
doc/*.txt   # 会忽略 doc 目录中的 .txt文件，但不会忽略 doc/server/ 下一级目录中的 .txt 文件
```
</details>
<br>

***  
## Summary of common-used git cmd
<div align=center>
    <img src="git_cmd.png" width="600" /><br>
    Fig. common-used git cmd.  
</div> 

## 3) Git in IDE

## **4) Git branch (for coorpation!)**
<div align=center>
    <img src="git-branch.png" width="600" /><br>
    Fig. git branch.  
</div> 
<br>

The default branch is the `master` branch, which already exists when the git repo was initialized.   
### Check the git branches:
```bash
git branch      # chenck the local branches
git branch -r   # chenck the remote branches
```
### Modify the branches:
```bash
# Create a new branch, but stay in current branch
git branch [branch_name]

# Create a new branch and switch to it
git checkout -b [branch_name]

# Switch to a branch:
git checkout [branch_name]

# Merge a branch to the current
git merge [another_branch]

# Delete a branch
git branch -d [branch_name]

# Delete a REMOTE branch
git push origin --delete [branch_name]
git branch -dr [remote/branch]
```

- 如果多个分支并行执行，代码互不冲突，相当于同时存在多个版本；  
- 如果同一个文件在合并分支时都被修改过，则会引起冲突。此时需要选择要保留哪个版本，修改冲突，然后重新提交。  
- `master` 主分支应该非常稳定，用来发布新的版本。一般不允许直接在上面进行开发工作。开发工作应该在其他新建的分支上进行，经过测试，确定可以稳定运行之后，再合并到主分支 (`merge`)。

<br>

***
### **Git branch cheatsheet**
<div align=center>
    <img src="git-branch-cheatsheet.png"/><br>
    Fig. common-used git cmd.  
</div> 

## An overview of git workflow
<div align=center>
    <img src="git-workflow-1.png"/><br>
    Fig. overview git workflow.  
</div> 