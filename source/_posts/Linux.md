---
layout: linux
title: linux base use
date: 2020-04-14 10:25:58
tags: linux
categories:
- linux
---
# linux
- 快捷键
  -  Ctrl+d 退出终端
  -  Ctrl+s 暂停 任意键恢复
  -  Ctrl+z 后台运行 fg恢复
  -  Ctrl+a 光标移至 头
  -  Ctrl+e 光标移至 尾
  -  Ctrl+k 删除光标所在到尾
  -  Alt+Backspace 向前删除一个单词
  -  Shift+PgUp 终端显示向上
  -  Shift+PgDn 终端显示向下

- Shell通配符
  - \* 匹配0或多个字符
  - ? 匹配任意一个字符
  - [list] 匹配list中任意单一字符
  - [^list] 匹配除list中任意单一字符以外字符
  - [c1-c2] 匹配c1-c2中任意单一字符 :[0-9][a-z]
  - {string1,string2,..} 匹配string1或string2..其一字符串
  - {c1..c2} 匹配c1-c2中全部字符 :{1..10}
 
- man
  - 1 一般命令
  - 2 系统调用
  - 3 库函数(c标准函数库)
  - 4 特殊文件(/dev 设备)和驱动程序
  - 5 文件格式和约定
  - 6 游戏和屏保
  - 7 杂项
  - 8 系统管理命令和守护进程
  
- pic char
  - banner  sysvbanner printerbanner
  - toilet
  - figlet
  
- who
  - -a 打印能打印
  - -d 打印死掉进程
  - -m 同am i,mom likes
  - -q 打印当前登录用户数及用户名
  - -u 打印当前登录用户登录信息
  - -r 打印运行等级
  
- add user
  - sudo adduser kanseer 创建kanseer(默认组与用户名相同)
  - su -l kanseer 切换kanseer用户
  - useradd 只创建用户 手动passwd设置密码

- add groups
  - groups kanseer 显示用户:用户组
  
- add root p
  - sudo usermod -G sudo kanseer
  
- del user
  - sudo deluser kanseer --remove-home
  
- del group
  - groupdel
 
- ls -l
![image](https://doc.shiyanlou.com/linux_base/3-9.png)
![image](https://doc.shiyanlou.com/linux_base/3-10.png)
  - -a 显示隐藏文件
  - -d 查看目录
  - -s 显示文件大小
  - -S 按文件大小
  
- change file author
  - sudo chown user file
  
- change file p
  ![image](https://doc.shiyanlou.com/linux_base/3-14.png)
  - chmod 600 file
  - chmod go+rw file g group o others u user -+权限
  
- FHS标准
![image](https://doc.shiyanlou.com/linux_base/4-1.png)

- 文件四种交互作用形态
![image](https://doc.shiyanlou.com/document-uid18510labid59timestamp1482919171956.png)

- mkdir
  - p 创建多级目录文件
  
- cp
  - cp file dir 复制文件到指定目录
  - -r or -R 复制目录
  
- rm
  - rm file 
  - -f 强制 -r 递归
  
- mv
  - mv source dir 移动文件到指定目录
  - mv old new 重命名文件
  
- rename 
  - rename 's/\.txt/\.c/' *.txt 批量将后缀为.txt改为.c
  - rename 'y/a-z/A-Z' *.c 批量将文件名和后缀大写
  
- check file
  - cat 正序
  - tac 倒序
  - -b 指定添加行号的方式
    - b a : 表示无论是否为空行,同样列出行号("cat -n"就是这种方式)
    - b t : 只列出非空行的编号并列出(默认为这种方式)
  - -n : 设置行号的样式，主要有三种：
    - n ln : 在行号字段最左端显示
    - n rn : 在行号字段最右边显示，且不加 0
    - n rz : 在行号字段最右边显示，且加 0
  - -w : 行号字段占用的位数(默认为 6 位)
  - nl file 添加行号并打印
  
- more less

- head tail
  - -n 1
  - -f 实现不停地读取某个文件的内容并显示
  
- file
  - file file 查看文件类型
  
- edit
  - vimtutor
  
- xeyes
  - nohup xeyes & 后台运行

- kill
  - sudo kill -9 PID 杀死进程

