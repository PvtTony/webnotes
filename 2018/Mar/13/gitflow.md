### Git flow

- 使用**分支**确定职责

  1. **master** 主分支 最为稳定功能最为完整的随时可发布的代码
  2. hotfix 线上bug修正
  3. **develop** 功能开发主分支 永远是功能最新最全的分支
  4. feature 功能点开发分支
  5. release 发布定期上线的功能

  图示：

  ![Git-Flow-Demo](./res/git-workflow-release-cycle-4maintenance.png)

### 团队协作规范

- ####代码提交

  - **使每次提交都有质量**

  1. 提交时的粒度是一个小功能点或者一个 bug fix，这样进行恢复等的操作时能够将「误伤」减到最低；
  2. 用一句简练的话写在第一行，然后空一行稍微详细阐述该提交所增加或修改的地方；
  3. **不要每提交一次就推送一次**，多积攒几个提交后一次性推送，这样可以避免在进行一次提交后发现代码中还有小错误。

- #### 代码推送

  - **一个人进行开发时，在功能完成之前不要急着创建远程分支。**
  - commit 规范：

  ```
  <type>(<scope>): <subject>
  <空行>
  <body>
  <空行>
  <footer>
  ```

  - Type

    - feat：新功能（feature）
    - fix：修补bug
    - docs：文档（documentation）
    - style： 格式（不影响代码运行的变动）
    - refactor：重构（即不是新增功能，也不是修改bug的代码变动）
    - test：增加测试
    - chore：构建过程或辅助工具的变动

  - Scope

    用来说明本次Commit影响的范围，即简要说明修改会涉及的部分。

  - Subject

    简要描述本次改动

    - 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
    - 首字母不要大写
    - 结尾不用句号(.)

  - Body

    <body>里的内容是对上面subject里内容的展开，在此做更加详尽的描述，内容里应该包含修改动机和修改前后的对比。

  - Footer

    footer里的主要放置**不兼容变更**和**Issue关闭**的信息

  - Revert

    如果需要撤销之前的Commit，那么本次Commit Message中必须以`revert：`开头，后面紧跟前面描述的Header部分，格式不变。并且，Body部分的格式也是固定的，必须要记录撤销前Commit的SHA值。

- #### 代码拉取

  - ```
    git pull --rebase
    ```

- #### 代码合并

  在将其他分支的代码合并到当前分支时，如果那个分支是当前分支的父分支，为了保持图表的可读性和可追踪性，可以考虑用 `git rebase` 来代替 `git merge`；反过来或者不是父子关系的两个分支以及互相已经 `git merge` 过的分支，就不要采用 `git rebase` 了，避免出现重复的冲突和提交节点。

### 分支管理

####操作流程

**master** 和 **develop** 为保留分支，除了各项目的负责人外不允许进行推送和删除等操作。

- 开发功能

  在确定发布日期之后，将需要完成的内容细分一下分配出去。

  负责某个功能的开发人员利用 SourceTree 所提供的 Git Flow 工具创建一个对应的 feature 分支。如果是多人配合的话，创建分支并做一些初始化工作之后就推送创建远程分支；否则，直到功能开发完毕要合并进 develop 前，不要创建远程分支。

  功能开发并自测之后，先切换到 develop 分支将最新的代码拉取下来；再切换回自己负责的 feature 分支把 develop 分支的代码合并进来，合并方式参照上文中的「代码合并」；如果有冲突则自己和配合的人一起解决；最后，到 GitLab 上创建合并请求（merge request）给项目负责人。

  项目负责人在收到合并请求时，应该先做下代码审核看看有没有明显的严重的错误；有问题就找开发人员去修改，没有就接受请求并删除对应的 feature 分支。

- 测试功能

  在将某次发布的所需功能全部开发完成时，负责测试的人创建一个 release 分支部署到测试环境进行测试；

  若发现了 bug，相应的开发人员就在 release 分支上或者基于 release 分支创建一个分支进行修复。

- 发布功能

  当确保某次发布的功能可以发布时，负责发布的人将 release 分支合并进 master 和 develop 并打上 tag，然后打包发布到线上环境。

  建议打 tag 时在信息中详细描述这次发布的内容，如：添加了哪些功能，修复了什么问题。

- 问题修正

  当发现线上环境的代码有小问题或者做些文案修改时，相关开发人员就在本地创建 hotfix 分支进行修改，具体操作参考「开发功能」。

  如果是相当严重的问题，可能就得回滚到上一个 tag 的版本了。

#### 命名规则

- feature 分支：按照功能点（而不是需求）命名；
- release 分支：用发布时间命名，可以加上适当的前缀；
- hotfix 分支：issue 编号或 bug 性质等。

另外还有 tag，用[语义化的版本号](http://semver.org/lang/zh-CN/)命名。