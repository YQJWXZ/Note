# Git

## 1. 版本控制

## 2. Git
1. 三个区域
* 工作区
* 暂存区
* Git仓库

2. 工作区中文件的四种状态
* 未跟踪(Untracked)  
* 未修改(Unmodified)
* 已修改(Modified)
* 已暂存(Staged)
### 2.1 基础
1. 配置用户信息
```git
git config --global user.name "xiaoyi0814"

git config --global user.email "@sdfhjdis.com"
```
2. 检查配置信息
```git
    # 查看所有的全局配置项
    git config --list --global
    
    # 查看指定的全局配置项
    git config user.name
    git config user.email
```

3. 检查文件状态
> git status
> 
> git status -s 以精简的方式显示文件状态

6. 跟踪新文件
> git add index.html

7. 提交更新
> git commit -m '新增文件描述信息'

8. 对已提交的文件进行修改
9. 暂存已修改的文件

10. 提交已暂存的文件
> git commit -m

11. 撤销对文件的修改
> 本质：用Git仓库中保存的文件，覆盖工作区中指定的文件
> 
> 所有修改都会丢失，且无法恢复
> 
> git checkout -- indeex.html

12. 向暂存区中一次性添加多个文件
> git add .

13. 取消暂存的文件(暂存区移除文件)
> gir reset HEAD index.html

14. 跳过使用暂存区
> git commit -a -m '描述消息'

15. 移除文件
> git rm -f index.html  从Git仓库和工作区中同时移除对应的文件
> 
> git rm --cached index.html  只从Git仓库中移除指定的文件，但保留工作区中对应的文件

16. 忽略文件
* 以#开头的是注释
* 以/结尾的是目录
* 以/开头防止递归
* 以!开头表示取反
* 可以使用*glob模式*进行文件和文件夹的匹配(glob模式是指简化了的正则表达式)

17. 查看提交历史
> git log
> 
> git log -2 只显示最新的两条提交历史
> 
> git log -2 --pretty=oneline 在一行上展示最近两条提交历史的信息
> 

> git log -2 --pretty=format:"%h | %an | %ar | %s "
> 
> %h：提交的简写哈希值
> 
> %an: 作者名字
> 
> %ar：作者修订日期
> 
> %s: 提交说明

## 3. GitHub

1. 常见的五种开源许可协议
* BSD
* Apache Licence 2.0
* GPL
    * 具有传染性的一种开源协议，不允许修改后和衍生的代码作为闭源的商业软件发布和销售
    * eg: Linux
* LGPL
* MIT
    * 目前限制最少的协议，唯一的条件：在修改后的代码或者发行包中，必须包含原作者的许可信息
    * eg: jQuery、Node.js

2. 基于HTTPS将本地仓库上传到GitHub
```git
    // 本地没有现成的Git仓库
    echo "# project_02" >> README.md  使用终端命令创建README.md文档，并写入初始内容为# project_02
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin https://github.com/xxx/project_02.git  将本地仓库和远程仓库进行关联，并把远程仓库命名为origin
    git push -u origin master  将本地仓库中的内容推送到远程的origin仓库中
    
    // 本地有现成的Git仓库
    git remote add origin https://github.com/xxx/project_02.git
    git push -u origin master
```
3. 配置SSH传输远程仓库

1. 生成SSH key
> ssh-ketgen -t rsa -b 4096 -C "xxxx@xxx.com"

2. 检测Github的SSH key是否配置成功
> ssh -T git@github.com

## 4. 分支

1. 查看分支
> git branch

2. 创建新分支
> git branch 分支名称

3. 切换分支
> git checkout 分支名称

4. 分支的快速创建和切换
> git checkout -b 分支名称

5. 合并分支
> git merge 要合并的分支
```git
  # 先切换到master分支
  git checkout master
  
  # 在master分支上运行 git merge命令，将login分支的代码合并到master分支
  git merge login
```

6. 删除分支
> git branch -d 分支名称

7. 遇到冲突时的分支合并
> 手动解决冲突

8. 将本地分支推送到GitHub仓库
```git
  # -u 表示把本地分支和远程分支进行关联，只在第一次推送的时候需要带 -u 参数
  git push -u 远程仓库的别名 本地分支名称：远程仓库名称
  eg: git push -u origin payment:pay
  
  # 如果希望远程分支的名称和本地分支名称保持一致，则可以对命令进行简化
  git push -u origin payment
```

9. 跟踪分支
> git checkout 远程分支的名称
> 
> git checkout -b 本地分支名称 远程仓库名称/远程分支名称

10. 拉取当前远程分支的最新的代码
> git pull

11. 删除远程分支
> git push 远程仓库名称 --delete 远程分支名称