> BY 张建成([prettyEcho@github](https://github.com/prettyEcho))
> 
>  除非另行注明，页面上所有内容采用知识共享-署名（[CC BY 2.5 AU](https://creativecommons.org/licenses/by/2.5/au/deed.zh)）协议共享

<p style="color: rgb(253,201,11);" align="center">🐬🐬 欢迎评论和star 🐳🐳</p>

### git命令行

1. git clone
    * 用于将远端仓库拷贝到本地
    * ssh: git clone username@host:/path/to/repository
    * https: git clone https:/path/to/repository.git

2. git config 
    * 这个命令定义了所有配置，从用户信息到仓库行为等等
    * git config --global --edit : 编辑器打开配置文件
    * git config --global user.name : 配置提交的用户名
    * git config --global user.email : 配置提交的邮箱

3. git add
    * 将本地工作区(Working dir)改变提交到缓存区(Index)
    * git add <file> : 提交确定文件
    * git add * : 提交所有更改
    * git add -A : 提交tracted和untracted中的文件提交到缓存区
    * git add -u : 提交tracted中的文件提交到缓存区
    * git add -p : 交互式提交

4. git commit
    * 将缓存的快照提交到项目历史
    * git commit -m "<message>" ： 提交已经缓存的快照。它会运行文本编辑器，等待你输入提交信息。当你输入信息之后，保存文件，关闭编辑器，创建实际的提交。

5. git status
    * 列出已缓存、未缓存、未追踪的文件（缓存区和工作区文件状态）
    * Changes to be committed: 文件在缓存区
    * Changes not staged for commit: 在工作区已经追踪的文件
    * Untracked files: 在工作区未追踪的文件

6. git pull 
    * 拉取并合并远端项目(默认拉取marter项目)

7. git push
    * 推送本地git到远端
    * git push 
    * git push origin <branch>: 推送本地git到远端某个分支
    * git push --set-upstream origin <branch>: 创建远端分支并推送代码

8. git log 
    * 命令显示已提交的快照
    * git log
    * git log -p -2: -p 选项展开显示每次提交的内容差异，用 -2 则仅显示最近的两次更新
    * git log --stat: 显示增减行提示
    * git --pretty: 可以指定使用完全不同于默认格式的方式展示提交历史,可选择的选项：oneline, short, full, fuller, format
    format格式清单：
    ```
    选项	 说明
    %H	提交对象（commit）的完整哈希字串
    %h	提交对象的简短哈希字串
    %T	树对象（tree）的完整哈希字串
    %t	树对象的简短哈希字串
    %P	父对象（parent）的完整哈希字串
    %p	父对象的简短哈希字串
    %an	作者（author）的名字
    %ae	作者的电子邮件地址
    %ad	作者修订日期（可以用 -date= 选项定制格式）
    %ar	作者修订日期，按多久以前的方式显示
    %cn	提交者(committer)的名字
    %ce	提交者的电子邮件地址
    %cd	提交日期
    %cr	提交日期，按多久以前的方式显示
    %s	提交说明
    ```
    * git --graph: ASCll图形
    * 美观：git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative

9. git checkout
    * 这个命令有三个不同的作用：检出文件、检出提交和检出分支
    * 提交层面
        + 移动HEAD指针到固定的提交
        + git checkout HEAD~2
        + git checkout <commit>

    * 文件层面
        + 用提交版本中的文件覆盖本地工作区的文件
        + git checkout HEAD~2 test.txt
        + git checkout HEAD <file> (常用)
     <p style="text-align: center">
        <img src="https://user-images.githubusercontent.com/22290721/38131555-2380bf1c-343a-11e8-84e6-8474d2d8e81a.png" alt="checkout-file" style="width: 40%">
    </p>

    * 分支层面
        + 切换分支
        + git checkout <branch>

10. git reset
    * 修改提交版本，会删除提交历史（一定要谨慎），切记当把提交推送到远端后，禁止使用git reset
    * 提交层面

        + git reset --soft HEAD~2
        + git reset --soft <commit>

    <p style="text-align: center">
        <img src="https://user-images.githubusercontent.com/22290721/38131865-3f9d476e-343b-11e8-8b36-dbfce778ccce.png" alt="reset" style="width: 40%">
    </p>


    除了在当前分支上操作，你还可以通过传入这些标记来修改你的缓存区或工作目录：

        * --soft – 缓存区和工作目录都不会被改变
        * --mixed – 默认选项。缓存区和你指定的提交同步，但工作目录不受影响
        * --hard – 缓存区和工作目录都同步到你指定的提交
         
    <p style="text-align: center">
        <img src="https://user-images.githubusercontent.com/22290721/38131884-59efb520-343b-11e8-85fe-bfc739fe84ae.png" alt="reset-scope" style="width: 40%">
    </p>
        
    常用操作：

        * git reset --mixed HEAD / git reset HEAD
        * git reset --hard HEAD

    * 文件层面
        + 用提交版本中的文件覆盖缓存区的文件
        + git reset HEAD~2 text.txt
        + git reset HEAD <file>

      <p style="text-align: center">
        <img src="https://user-images.githubusercontent.com/22290721/38132348-4fd7245e-343d-11e8-959f-8caf4287a4e3.png" alt="reset-file" style="width: 40%">
    </p>

11. git revert
    * Revert撤销一个提交的同时会创建一个新的提交。这是一个安全的方法，因为它不会重写提交历史。
    * git revert HEAD~2
        - 会找出倒数第二个提交，然后创建一个新的提交来撤销这些更改，然后把这个提交加入项目中。

    <p style="text-align: center">
        <img src="https://user-images.githubusercontent.com/22290721/38132384-6fe2c564-343d-11e8-8d7e-8ac0734dab74.png" alt="revert" style="width: 40%">
    </p>

12. git stash
    * git stash : 暂存当前正在进行的工作
    * git stash pop : 恢复暂存的文件
    * git stash list: 显示暂存栈中所有暂存的历史
    * git stash apply stash@{1} : 将指定暂存纪录恢复
    * git stash clear : 清空暂存栈

13. git branch
    * 创建、列出、重命名和删除分支
    * git branch : 列出所有分支
    * git branch <branch> : 创建一个名为<branch>的分支
    * git branch -d <branch> : 删除指定分支。这是一个安全的操作，Git 会阻止你删除包含未合并更改的分支。
    * git branch -D <branch> : 强制删除指定分支，即使包含未合并更改。如果你希望永远删除某条开发线的所有提交，你应该用这个命令。
    * git branch -m <branch> : 将当前分支命名为 <branch>。

14. 简记图

<p style="text-align: center">
    <img src="https://user-images.githubusercontent.com/22290721/38132460-baa96bfc-343d-11e8-848f-869435d8f5c9.jpg" alt="main" style="width: 30%">
</p>

15. 参考
[https://www.cnblogs.com/houpeiyong/p/5890748.html](https://www.cnblogs.com/houpeiyong/p/5890748.html)
[https://github.com/geeeeeeeeek/git-recipes](https://github.com/geeeeeeeeek/git-recipes)