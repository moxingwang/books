# git tag
    Git 中的tag指向一次commit的id，通常用来给开发分支做一个标记，如标记一个版本号。
    打标签
    git tag -a v1.01 -m "Relase version 1.01"
    注解：git tag 是打标签的命令，-a 是添加标签，其后要跟新标签号，-m 及后面的字符串是对该标签的注释。
    提交标签到远程仓库
    git push origin --tags
    注解：就像git push origin master 把本地修改提交到远程仓库一样，-tags可以把本地的打的标签全部提交到远程仓库。
    删除标签
    git tag -d v1.01
    注解：-d 表示删除，后面跟要删除的tag名字
    删除远程标签
    git push origin :refs/tags/v1.01
    注解：就像git push origin :branch_1 可以删除远程仓库的分支branch_1一样， 冒号前为空表示删除远程仓库的tag。
    查看标签
    git tag
    或者
    git tag -l