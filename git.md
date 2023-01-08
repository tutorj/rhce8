# [Git](https://git-scm.com/book/zh/v2)

[toc]

## Kick-off

- What is Git?

  - 一款免费、开源、分布式版本控制系统

  

- What is version control?

  - Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later、

    版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统

    

- 比较

  - CVS/Subversion/Perforce....

    - 差异比较 - delta-based
    - 以文件变更列表的方式存储信息
    - 将它们看作是一组基本文件和每个文件随时间逐步累积的差异

  - Git

    - 把数据看作是对小型文件系统的一系列快照
    - 在 Git中，每当你提交更新或保存项目状态时，它基本上就会对当时的全部文件创建一个快照并保存这个快照的索引
    - 为了效率，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件

    ![image-20230106151521687](C:\Users\ezjiaha\AppData\Roaming\Typora\typora-user-images\image-20230106151521687.png)



- 操作

  - 本地完成大部分操作，减少集中式版本控制系统带来的网络开销

  

- 数据完整性

  - 数据存储前计算checksum，以便后续校验/引用

  - SHA-1 Hash，由 40 个十六进制字符组成的String

  - Git中保存的信息都是以文件内容的Hash作为index而非文件名

    

- 只添加数据

  - 几乎所有的git操作几乎只往Git数据库中添加数据

  

- Git文件状态

  | State                | Description                                                  |
  | -------------------- | ------------------------------------------------------------ |
  | committed （已提交） | 已提交表示数据已经安全地保存在本地数据库中                   |
  | modified （已修改）  | 已修改表示修改了文件，但还没保存到数据库中                   |
  | staged（已暂存）     | 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中 |



- Git项目section

  | section                        | Description                                                  |
  | ------------------------------ | ------------------------------------------------------------ |
  | Working Directory （工作区）   | 对项目的某个版本独立提取出来的内容<br />这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改 |
  | Staging Area （暂存区）        | 保存了下次将要提交的文件列表信息，一般在 Git 仓库目录中      |
  | .git directory （Git仓库目录） | Git 用来保存项目的元数据和对象数据库的地方<br />这是 Git 中最重要的部分，从其它计算机克隆仓库时，复制的就是这里的数据 |

  ![image-20230106154035489](C:\Users\ezjiaha\AppData\Roaming\Typora\typora-user-images\image-20230106154035489.png)

  

- Git工作流程

  1. 在工作区中修改文件
  2. 将你想要下次提交的更改选择性地暂存 - staging，这样只会将更改的部分添加到暂存区
  3. 提交更新 - commit，找到暂存区的文件，将快照永久性存储到 Git 目录 - .git directory

  

- Git配置

  - git config可以设置控制 Git 外观和行为的配置变量，变量存储于

    - linux （自下而上依此覆盖）
      - /etc/gitconfig
        - 包含系统上每一个用户及他们仓库的通用配置
        - 如果在执行 git config 时带上--system 选项，那么它就会读写该文件中的配置变量
      - ~/.gitconfig or ~/.config/git/config
        - 包含当前用户的配置
        - 传递--global让git读写此文件，这会对系统上所有的仓库生效
      - 当前使用仓库的config - .git/config
        - 默认使用
        - 传递--local让Git读写此文件
    - Windows
      - C:\\Users\\{User}\\.gitconfig

    ```bash
    # 查看所有配置所在
    git config --list --show-origin
    ```

  

- 用户信息

  - 安装后的第一件事配置username & email，每次commit都会使用到这些信息

    ```bash
    # 该命令只需要执行一次
    # 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有--global选项的命令来配置
    git config --global user.name "tutorj"
    git config --global user.email hironwayj@gmail.com
    ```

    

- 文本编辑器

  - By default, Git uses whatever you’ve set as your default text editor via one of the shell environment variables `VISUAL` or `EDITOR`, or else falls back to the `vi` editor to create and edit your commit and tag messages

      ```bash
      git config --global core.editor emacs
      ```

  
  
- 检查配置

  ```bash
  git config --list
  # git config <config>
  git config user.name
  ```

  

- Help

  ```bash
  git help <verb>
  git <verb> --help
  man git-<verb>
  
  # git config manual page - open in browser
  git help config
  ```

  

## Basics

- 获取Git仓库

  1. 将尚未进行版本控制的本地目录转换为Git仓库

     ```bash
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice
     $ ls -lhtra
     total 4.0K
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 ./
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 ../
     
     # git init
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice
     $ git init
     Initialized empty Git repository in C:/Users/ezjiaha/Desktop/Checkmate/git practice/.git/
     
     # ++.git directory
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ ls -lhtra
     total 8.0K
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 ../
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 ./
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 .git/
     ```

     之后便可该仓库下的文件进行追踪并提交

     ```bash
     # create some files
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ touch file1 file2 file3
     
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ touch LICENSE
     
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ ls -lhtra
     total 12K
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 ../
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:35 .git/
     -rw-r--r-- 1 ezjiaha 1049089 0 Jan  6 16:38 file1
     -rw-r--r-- 1 ezjiaha 1049089 0 Jan  6 16:38 file2
     -rw-r--r-- 1 ezjiaha 1049089 0 Jan  6 16:38 file3
     drwxr-xr-x 1 ezjiaha 1049089 0 Jan  6 16:39 ./
     -rw-r--r-- 1 ezjiaha 1049089 0 Jan  6 16:39 LICENSE
     
     # add to track
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ git add file*
     
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ git add LICENSE 
     
     # commit
     ezjiaha@CN-00012861 MINGW64 ~/Desktop/Checkmate/git practice (master)
     $ git commit -m "initial project version"
     [master (root-commit) c3f7f45] initial project version
      4 files changed, 0 insertions(+), 0 deletions(-)
      create mode 100644 LICENSE
      create mode 100644 file1
      create mode 100644 file2
      create mode 100644 file3
     ```

  2. 从其它服务器clone一个已存在的Git仓库

     - 这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 `.git` 文件夹， 从远程仓库拉取下所有数据放入 `.git` 文件夹，然后从中读取最新版本的文件的拷贝
     - Git 支持多种数据传输协议。 上面的例子使用的是 `https://` 协议，不过你也可以使用 `git://` 协议或者使用 SSH 传输协议

     ```bash
     hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop
     $ git clone https://github.com/libgit2/libgit2
     Cloning into 'libgit2'...
     remote: Enumerating objects: 119285, done.
     remote: Counting objects: 100% (119285/119285), done.
     remote: Compressing objects: 100% (32759/32759), done.
     remote: Total 119285 (delta 84626), reused 119169 (delta 84519), pack-reused 0
     Receiving objects: 100% (119285/119285), 61.23 MiB | 565.00 KiB/s, done.
     Resolving deltas: 100% (84626/84626), done.
     Updating files: 100% (11537/11537), done.
     
     hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop
     $ cd libgit2
     
     hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
     $ ll
     total 147
     -rw-r--r-- 1 hiron 197609   280  1月  7 13:19 api.docurium
     -rw-r--r-- 1 hiron 197609  1364  1月  7 13:19 AUTHORS
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 ci/
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 cmake/
     -rw-r--r-- 1 hiron 197609  5890  1月  7 13:19 CMakeLists.txt
     -rw-r--r-- 1 hiron 197609 63073  1月  7 13:19 COPYING
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 deps/
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 docs/
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 examples/
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 fuzzers/
     -rw-r--r-- 1 hiron 197609  3178  1月  7 13:19 git.git-authors
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 include/
     -rw-r--r-- 1 hiron 197609   294  1月  7 13:19 package.json
     -rw-r--r-- 1 hiron 197609 17503  1月  7 13:19 README.md
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 script/
     -rw-r--r-- 1 hiron 197609   615  1月  7 13:19 SECURITY.md
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:19 src/
     drwxr-xr-x 1 hiron 197609     0  1月  7 13:20 tests/
     
     # 通过额外的参数指定新的目录名
     git clone https://github.com/libgit2/libgit2 mylibgit
     ```

- 記錄每次更新到倉庫

  - 通常我們會對倉庫内的文件進行修改，每當完成了一個階段的目標，就記錄下它並提交到倉庫

  - 倉庫下的每一個文件無外乎兩種狀態

    - 已跟蹤：被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后， 它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是 Git 已经知道的文件
    - 未跟蹤：既不存在于上次快照的记录中，也没有被放入暂存区； 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态，因为 Git 刚刚检出了它们， 而你尚未编辑过它们

  - 編輯過某些文件之後，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 在工作时，你可以选择性地将这些修改过的文件放入暂存区，然后提交所有已暂存的修改，如此反复

    ![Git 下文件生命周期图。](https://git-scm.com/book/en/v2/images/lifecycle.png)

    ```bash
    # 檢查當前倉庫下哪些哪些文件處於什麽狀態
    # 1. 所有已跟踪文件在上次提交后都未被更改过
    # 2. 当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来
    # 3. 顯示當前分支為main
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    nothing to commit, working tree clean
    
    
    # 創建一個README文件並再次檢查狀態，提示未跟蹤的狀態
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ echo 'My Project' > README
    
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
            README
    
    nothing added to commit but untracked files present (use "git add" to track)
    
    
    # 追蹤剛才的README，提示README已被追蹤，並處於暫存狀態 = Changes to be commited
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git add README
    warning: LF will be replaced by CRLF in README.
    The file will have its original line endings in your working directory
    
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   README
            
            
    # 修改一個已經被跟蹤的文件並再次檢查狀態
    # Changes not staged for commit - 説明已追蹤的文件内容發生了變化，但未放到暫存區
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ vi README.md
    
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   README
    
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   README.md
    
    
    # 因此，這裏需要將其加入到暫存區中
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git add README.md
    
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   README
            modified:   README.md
            
            
    # 如果此時再進行修改，則README.md會同時出現在暫存區和非暫存區
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   README
            modified:   README.md
    
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   README.md
    
    
    # 因此，每次修改后最好都加入到暫存區
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git add README.md
    
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   README
            modified:   README.md
    ```

  - 狀態簡覽

    ```bash
    # ?? - 新添加未跟蹤的文件
    # A  - 新添加到暫存區
    # M  - 修改過的文件
    # MM - 暫存后又做了修改
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status -s
    A  README
    M  README.md
    
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status --short
    A  README
    M  README.md
    ```

  - 忽略文件

    - .gitignore記錄哪些文件不需要被Git進行管理追蹤
    - https://github.com/github/gitignore

    ```bash
    # regex
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ cat .gitignore
    /build/
    .DS_Store
    *~
    .*.swp
    /tags
    CMakeSettings.json
    .vs
    .idea
    ```

  - 查看已暫存和未暫存的修改

    ```bash
    # 修改README並暫存
    # 修改README.md不暫存
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git status
    On branch main
    Your branch is up to date with 'origin/main'.
    
    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            new file:   README
            modified:   README.md
    
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   README.md

    
    # 查看未暫存文件的修改
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git diff README.md
    diff --git a/README.md b/README.md
    index ce528101f..d860faa61 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,5 +1,6 @@
     simply add one
     add another
    +add one more
     libgit2 - the Git linkable library
     ==================================
    
    
    # 查看已暫存文件的修改
    hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
    $ git diff --staged README
    diff --git a/README b/README
    new file mode 100644
    index 000000000..63d4ea066
    --- /dev/null
    +++ b/README
    @@ -0,0 +1,2 @@
    +My Project
    +add
    
    
    # 也可以使用图形化的工具或外部 diff 工具来比较差异
    git difftool <unstaged file>
    git difftool --staged <staged file>
    ```
    

    - 提交更新

      ```bash
      # 添加注釋
      # 提交后它会告诉你
      # 1. 当前是在哪个分支（master）提交的
      # 2. 本次提交的完整 SHA-1 校验和是什么（1293af857）
      # 3. 本次提交中，有多少文件修订过，多少行添加和删改过
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git commit -m "Story 182: Fix benchmarks for speed"
      [main 1293af857] Story 182: Fix benchmarks for speed
       2 files changed, 5 insertions(+)
       create mode 100644 README
      ```

    - 跳過使用暫存

      ```bash
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git status
      On branch main
      Your branch is ahead of 'origin/main' by 1 commit.
        (use "git push" to publish your local commits)

      Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
        (use "git restore <file>..." to discard changes in working directory)
              modified:   README.md

      no changes added to commit (use "git add" and/or "git commit -a")

      # 加上-a選項Git就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git commit -a -m 'added new benchmarks'
      [main 07c885940] added new benchmarks
       1 file changed, 1 insertion(+)
      ```

    - 移除文件

- 必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交
      
    ```bash
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ rm SECURITY.md
      
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git status
      On branch main
      Your branch is ahead of 'origin/main' by 2 commits.
        (use "git push" to publish your local commits)
      
      Changes not staged for commit:
        (use "git add/rm <file>..." to update what will be committed)
        (use "git restore <file>..." to discard changes in working directory)
              deleted:    SECURITY.md
      
      no changes added to commit (use "git add" and/or "git commit -a")
      
      # 下次提交時該文件就不再被git所管理
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git rm SECURITY.md
      rm 'SECURITY.md'
    ```
    
    - 移動文件
    
      ```bash
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git mv README README_rename
      
      hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/libgit2 (main)
      $ git status
      On branch main
      Your branch is ahead of 'origin/main' by 2 commits.
        (use "git push" to publish your local commits)
      
      Changes to be committed:
        (use "git restore --staged <file>..." to unstage)
              renamed:    README -> README_rename
              deleted:    SECURITY.md
      ```

- 查看提交歷史

  ```bash
  # 不传入任何参数的默认情况下，git log 会按时间先后顺序列出所有的提交
  # 每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log
  commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Mon Mar 17 21:52:11 2008 -0700
  
      changed the verison number
  
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Sat Mar 15 16:40:33 2008 -0700
  
      removed unnecessary test code
  
  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Sat Mar 15 10:31:28 2008 -0700
  
      first commit
  
  
  # 显示最近2次提交所引入的差异
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log -p -2
  commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Mon Mar 17 21:52:11 2008 -0700
  
      changed the verison number
  
  diff --git a/Rakefile b/Rakefile
  index a874b73..8f94139 100644
  --- a/Rakefile
  +++ b/Rakefile
  @@ -5,7 +5,7 @@ require 'rake/gempackagetask'
   spec = Gem::Specification.new do |s|
       s.platform  =   Gem::Platform::RUBY
       s.name      =   "simplegit"
  -    s.version   =   "0.1.0"
  +    s.version   =   "0.1.1"
       s.author    =   "Scott Chacon"
       s.email     =   "schacon@gmail.com"
       s.summary   =   "A simple gem for using Git in Ruby code."
  
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Sat Mar 15 16:40:33 2008 -0700
  
      removed unnecessary test code
  
  diff --git a/lib/simplegit.rb b/lib/simplegit.rb
  index a0a60ae..47c6340 100644
  --- a/lib/simplegit.rb
  +++ b/lib/simplegit.rb
  @@ -18,8 +18,3 @@ class SimpleGit
       end
  
   end
  -
  -if $0 == __FILE__
  -  git = SimpleGit.new
  -  puts git.show
  -end
  \ No newline at end of file
  
  
  # 簡略顯示統計信息
  # 列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了，在每次提交的最后还有一个总结
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log --stat
  commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Mon Mar 17 21:52:11 2008 -0700
  
      changed the verison number
  
   Rakefile | 2 +-
   1 file changed, 1 insertion(+), 1 deletion(-)
  
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Sat Mar 15 16:40:33 2008 -0700
  
      removed unnecessary test code
  
   lib/simplegit.rb | 5 -----
   1 file changed, 5 deletions(-)
  
  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author: Scott Chacon <schacon@gmail.com>
  Date:   Sat Mar 15 10:31:28 2008 -0700
  
      first commit
  
   README           |  6 ++++++
   Rakefile         | 23 +++++++++++++++++++++++
   lib/simplegit.rb | 25 +++++++++++++++++++++++++
   3 files changed, 54 insertions(+)
   
   
   # 使用不同默認格式輸出歷史
   # oneline/short/full/fuller
   hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log --pretty=oneline
  ca82a6dff817ec66f44342007202690a93763949 changed the verison number
  085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
  a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
  
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log --pretty=short
  commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
  Author: Scott Chacon <schacon@gmail.com>
  
      changed the verison number
  
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gmail.com>
  
      removed unnecessary test code
  
  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author: Scott Chacon <schacon@gmail.com>
  
      first commit
  
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log --pretty=full
  commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
  Author: Scott Chacon <schacon@gmail.com>
  Commit: Scott Chacon <schacon@gmail.com>
  
      changed the verison number
  
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gmail.com>
  Commit: Scott Chacon <schacon@gmail.com>
  
      removed unnecessary test code
  
  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author: Scott Chacon <schacon@gmail.com>
  Commit: Scott Chacon <schacon@gmail.com>
  
      first commit
  
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log --pretty=fuller
  commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
  Author:     Scott Chacon <schacon@gmail.com>
  AuthorDate: Mon Mar 17 21:52:11 2008 -0700
  Commit:     Scott Chacon <schacon@gmail.com>
  CommitDate: Fri Apr 17 21:56:31 2009 -0700
  
      changed the verison number
  
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author:     Scott Chacon <schacon@gmail.com>
  AuthorDate: Sat Mar 15 16:40:33 2008 -0700
  Commit:     Scott Chacon <schacon@gmail.com>
  CommitDate: Fri Apr 17 21:55:53 2009 -0700
  
      removed unnecessary test code
  
  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author:     Scott Chacon <schacon@gmail.com>
  AuthorDate: Sat Mar 15 10:31:28 2008 -0700
  Commit:     Scott Chacon <schacon@gmail.com>
  CommitDate: Sat Mar 15 10:31:28 2008 -0700
  
      first commit
  
  
  # 定制記錄的顯示格式 - https://git-scm.com/book/zh/v2/ch00/pretty_format
  hiron@DESKTOP-MRRKMSM MINGW64 ~/Desktop/simplegit-progit (master)
  $ git log --pretty=format:"%h - %an, %ar : %s"
  ca82a6d - Scott Chacon, 15 years ago : changed the verison number
  085bb3b - Scott Chacon, 15 years ago : removed unnecessary test code
  a11bef0 - Scott Chacon, 15 years ago : first commit
  ```

  Table 1. git log --pretty=format 常用的选项

  | 选项  | 说明                                          |
  | :---- | :-------------------------------------------- |
  | `%H`  | 提交的完整哈希值                              |
  | `%h`  | 提交的简写哈希值                              |
  | `%T`  | 树的完整哈希值                                |
  | `%t`  | 树的简写哈希值                                |
  | `%P`  | 父提交的完整哈希值                            |
  | `%p`  | 父提交的简写哈希值                            |
  | `%an` | 作者名字                                      |
  | `%ae` | 作者的电子邮件地址                            |
  | `%ad` | 作者修订日期（可以用 --date=选项 来定制格式） |
  | `%ar` | 作者修订日期，按多久以前的方式显示            |
  | `%cn` | 提交者的名字                                  |
  | `%ce` | 提交者的电子邮件地址                          |
  | `%cd` | 提交日期                                      |
  | `%cr` | 提交日期（距今多长时间）                      |
  | `%s`  | 提交说明                                      |

  Table 2. git log 的常用选项

  | 选项              | 说明                                                         |
  | :---------------- | :----------------------------------------------------------- |
  | `-p`              | 按补丁格式显示每个提交引入的差异。                           |
  | `--stat`          | 显示每次提交的文件修改统计信息。                             |
  | `--shortstat`     | 只显示 --stat 中最后的行数修改添加移除统计。                 |
  | `--name-only`     | 仅在提交信息后显示已修改的文件清单。                         |
  | `--name-status`   | 显示新增、修改、删除的文件清单。                             |
  | `--abbrev-commit` | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。            |
  | `--relative-date` | 使用较短的相对时间而不是完整格式显示日期（比如“2 weeks ago”）。 |
  | `--graph`         | 在日志旁以 ASCII 图形显示分支与合并历史。                    |
  | `--pretty`        | 使用其他格式显示历史提交信息。可用的选项包括 oneline、short、full、fuller 和 format（用来定义自己的格式）。 |
  | `--oneline`       | `--pretty=oneline --abbrev-commit` 合用的简写。              |
  
  Table 3. 限制 git log 输出的选项
  
  | 选项                  | 说明                                       |
  | :-------------------- | :----------------------------------------- |
  | `-<n>`                | 仅显示最近的 n 条提交。                    |
  | `--since`, `--after`  | 仅显示指定时间之后的提交。                 |
  | `--until`, `--before` | 仅显示指定时间之前的提交。                 |
  | `--author`            | 仅显示作者匹配指定字符串的提交。           |
  | `--committer`         | 仅显示提交者匹配指定字符串的提交。         |
  | `--grep`              | 仅显示提交说明中包含指定字符串的提交。     |
  | `-S`                  | 仅显示添加或删除内容匹配指定字符串的提交。 |

- 撤銷操作

  ```bash
  # 提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了，可以重新提交
  # 最終只有一個提交，第2次提交將代替第1次提交的結果
  $ git commit -m 'initial commit'
  $ git add forgotten_file
  $ git commit --amend
  ```

  - 取消暫存的文件

    - 可以使用git reset HEAD <file>來取消暫存

    ```bash
    $ git add *
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
        renamed:    README.md -> README
        modified:   CONTRIBUTING.md
    
    
    $ git reset HEAD CONTRIBUTING.md
    Unstaged changes after reset:
    M	CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
        renamed:    README.md -> README
    
    # CONTRIBUTING.md從暫存區移除
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
        modified:   CONTRIBUTING.md
    ```

  - 撤銷對文件的修改

    - git checkout -- <file>來撤銷修改

    ```bash
    $ git checkout -- CONTRIBUTING.md
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
        renamed:    README.md -> README
    ```

- 遠程倉庫的使用

- 打標簽

- Git別名

- 總結

## Branch

