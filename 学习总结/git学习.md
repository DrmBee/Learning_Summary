# git学习
git入门：
				1.多个分支，分支上传之后需要合并到主分支上才会在main上有效。
				2.从main分支创建分支时，创建的是main当时的副本。
				3.打开拉取请求！！！拉取请求是 GitHub 上协作的核心！
				4.合并拉取请求！！


- 提交更改：
  `gh repo create`：为项目创建存储库。
  `git status`：用于查看在你上次提交之后是否对文件进行再次修改。
  `git add README.md && git commit -m "Add README"`：暂存并提交文件
  `git push --set-upstream origin HEAD`：将更改推送到您的分支。
  **注意：origin是远程仓库在本地的一个指针！！！HEAD是指向你正在工作的本地分支的指针！！！**

- 创建存储的分支

  **注意：gh命令主要是对远端仓库的处理！！！！**

  `gh repo fork REPOSITORY`：格式`gh repo fork OWNER/REPO`
  **注意：私有项目无法fork!!!同时也不能fork自己已有的项目！！！**
  `gh repo fork REPOSITORY --org "octo-org"`：在组织中创建分支，用`--org`标记。
  **注意：fork相当于在你的远端有了一个和你复刻的仓库一样的仓库，这和clone不同，clone是在本地拥有一个你想要的仓库。**
  `gh repo fork REPOSITORY --clone=true`：若要创建分支的克隆，使用 `--clone` 标记。

​		**`gh repo fork REPOSITORY --remote=true`：若要为分支存储库配置远程存储库，使用 `--remote` 标志。
​				`gh repo fork REPOSITORY --remote-name "main-remote-repo"`：若要指定远程存储库的名称，请使用 `--remote-name` 标志。**

- 创建分支以处理
`git branch BRANCH-NAME`
`git checkout BRANCH-NAME`：表示切换分支。
- 创建和推送更改
`git add .`：告诉 Git 你希望在下一次提交中包含所有更改。
`git commit -m "a short description of the change"`：会拍摄这些更改的快照。
`git push`：将更改推送到远程。
- 创建远程仓库
`git remote add origin <REMOTE_URL>`：使用 git remote add 命令将远程 URL 与名称匹配。
**不太明白！！！** https://docs.github.com/zh/get-started/getting-started-with-git/about-remote-repositories
- 基本git命令：
`git init`:初始化一个全新的 Git 存储库并开始跟踪现有目录。 它在现有目录中添加一个隐藏的子文件夹，该子文件夹包含版本控制所需的内部数据结构。
`git clone`:创建远程已存在的项目的本地副本。 克隆包括项目的所有文件、历史记录和分支。
`git add`:暂存更改。 Git 跟踪对开发人员代码库的更改，但有必要暂存更改并拍摄更改的快照，以将其包含在项目的历史记录中。 此命令执行暂存，即该两步过程的第一部分。 暂存的任何更改都将成为下一个快照的一部分，并成为项目历史记录的一部分。 通过单独暂存和提交，开发人员可以完全控制其项目的历史记录，而无需更改其编码和工作方式。
`git commit`:将快照保存到项目历史记录中并完成更改跟踪过程。 简言之，提交就像拍照一样。 任何使用 git add 暂存的内容都将成为使用 git commit 的快照的一部分。
`git status`:将更改的状态显示为未跟踪、已修改或已暂存。
`git branch`:显示正在本地处理的分支。
`git merge`:将开发线合并在一起。 此命令通常用于合并在两个不同分支上所做的更改。 例如，当开发人员想要将功能分支中的更改合并到主分支以进行部署时，他们会合并。
`git pull`:使用远程对应项的更新来更新本地开发线。 如果队友已向远程上的分支进行了提交，并且他们希望将这些更改反映到其本地环境中，则开发人员将使用此命令。
**注意：如果本地有未提交的修改，git pull会覆盖本地代码**
`git push`:使用本地对分支所做的任何提交来更新远程存储库。
- 推送提交到远程
git push两个参数：(参数都是描述的是远端)
远程名称：（例如origin）
分支名称：（例如main）
`git push REMOTE-NAME :BRANCH-NAME`
命令`git push origin main`将本地更改到联机存储库。

​		重命名分支：
​				`git push REMOTE-NAME LOCAL-BRANCH-NAME:REMOTE-BRANCH-NAME`
​				这会将 LOCAL-BRANCH-NAME 推送到 REMOTE-NAME，但它已重命名为 REMOTE-BRANCH-NAME。

​		删除远端分支或标记
​				`git push remote_name -d remote_branch_name`

​		远程与复刻
​				**不太明白！！！**
​				https://docs.github.com/zh/get-started/using-git/pushing-commits-to-a-remote-repository#remotes-and-forks


- 从远程仓库获取更改
使用 git fetch 检索其他人完成的新工作。 从存储库中提取会获取所有新的远程跟踪分支和标记，而无需将这些更改合并到自己的分支中。
如果已经有本地存储库包含为所需项目设置的远程 URL，则可以通过在终端使用 git fetch remotename 获取所有新信息
**有点不太明白**
https://docs.github.com/zh/get-started/using-git/getting-changes-from-a-remote-repository#fetching-changes-from-a-remote-repository

​		合并更改到本地分支
​				合并可将您的本地更改与其他人所做的更改组合起来。
​				通常将远程跟踪分支（即从远程仓库获取的分支）与您的本地分支进行合并：
​				`git merge REMOTE-NAME/BRANCH-NAME`

​		从远程仓库拉取更改
​				git pull 是在同一命令中完成 git fetch 和 git merge  的便捷方式:
​				`git pull REMOTE-NAME BRANCH-NAME`

​		**注意：master和main,可以通过命令`git branch -m master main`将master重命名为main**
​				**git fetch和git pull的区别：
​				https://www.cnblogs.com/runnerjack/p/9342362.html**

- **git remote** 命令用于用于管理 Git 仓库中的远程仓库。

  https://www.runoob.com/git/git-remote.html

error: remote origin already exists.表示远程仓库已存在。-》`git remote rm origin`删除关联的origin的远程库。

token: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
