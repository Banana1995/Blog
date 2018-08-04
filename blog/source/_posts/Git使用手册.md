---
title: Git以及TortoiseGit安装使用
date: 2018-06-10 20:02:12
categories: "工具"
tags: "GIT"
---

#### 1. 下载安装Git-2.8.3-64-bit.exe程序

- 默认选项一直next即可。


- 本地初始化git的配置项，设置username和email，使用如下命令：

 ```java
git config --global user.name "abc"
git config --global user.email "123abc@163.com"
 ```

--global 表示全局属性，所有的git项目都会公用这个属性。因为Git是分布式版本控制系统，需要一个用户名和email作为一个标识。

<!--more-->

#### 2. 配置ssh key

- **生成秘钥对** 

  在Git Bash中输入以下命令 ：

  ``` 
  ssh-keygen -t rsa -C "123abc@163.com"
  ```
  之后可以不用设置密码，按下3个回车键即可。接下来可以去默认路径 `C:\Users\banana\.ssh`下查看生成的两个文件，分别为 私钥：`id_rsa` 公钥：`id_rsa.pub` 

  ![](Git使用手册\微信截图_20180324002602.png)

- ​ **添加公钥到远程仓库** 
  打开GitHub主页，在`Settings`-->`SSH and GPG keys`中，点击`New SSH key` 按钮；

  再打开公钥文件，将其中的字符串完整复制，粘贴到`key` 中

  ![](Git使用手册\微信截图_20180324002258.png)

  执行`ssh -T git@github.com` 命令，查看公钥是否配置成功了，如下图所示则表示成功：

  ![](Git使用手册\微信截图_20180324213757.png)

- **将私钥添加到自己的系统中** 

  使用命令： `ssh-add ~/.ssh/id_rsa`  添加私钥至系统中，若无效的话，建议采取以下两种方法：
  1. 先执行 `eval 'ssh-agent-s' ` 再执行 `ssh-add ~/.ssh/id_rsa` ；
  2. 先执行`ssh-agent bash --login -i` 启动bash，或者说把bash挂到ssh-agent下面，再执行 `ssh-add`

  当看到下图所示结果时，则表示成功了

![](Git使用手册\微信截图_20180324003452.png)

#### 3. 配置远程仓库

- 登录github账号，新建一个远程仓库。

- 在本地建一个与仓库同名的文件夹，在文件夹中打开Git Bash，执行如下命令：

  ``` 
  echo "# commang" >> README.md
  新建一个README.md文件，写入“# commang”
  git init
  初始化git文件夹，创建master分支和.git文件夹
  git add README.md
  将工作区中的README.md文件添加进暂存区
  git commit -m "first commit"
  将暂存区的文件提交到master分支上
  git remote add origin git@github.com:abc/gitTest.git
  配置远程仓库地址命名为origin
  git push -u origin master
  将本地master分支的数据push到远程仓库的master分支上
  ```

- 若没能成功push去服务器的话，可以去检查下本地`.git` 文件夹下的`config` 文件，其中的url必须与上述命令中的远程地址相同。

  ![](Git使用手册\微信截图_20180324141838.png)
#### 4.安装TortoiseGit-2.6.0.0-64bit.msi文件 

[下载TortoiseGit和中文语言包](https://tortoisegit.org/download/)

- 默认选项一直next即可。

- 创建本地仓库，在文件夹中`右键-->Git在这里创建版本库` （我使用的是中文版本），如下图：
  ![](Git使用手册\微信截图_20180324214536.png)

  不用勾选，直接确定即可。

- **设置网络和远端** 

  **1. `右键-->设置` 将本地安装Git的ssh.exe路径地址配置到网络上，如下图：**

  ![](Git使用手册\微信截图_20180324215326.png)

  我的Git是安装在`C:\software\Git\`路径下 。

  **2. 将远程仓库的地址粘贴到`URL`和`推送URL` 中，如下图：**

  ![](Git使用手册\微信截图_20180324214754.png)

- 至此，你已经可以愉快的使用右键进行push和update了，但是会时不时遇到需要输入密码，但是你怎么输都不对的情况。

#### 5. 使用本地`Pageant`记住你的私钥密码

1. 开始菜单找到TortoiseGit菜单下的puttygen，打开puttygen

   ![](Git使用手册\1.png)


2. 选择导入秘钥

   ![](Git使用手册\2.png)

3. 选择C:\Users\Administrator\.ssh目录下的私钥，并输入秘钥密码

   ![](Git使用手册\3.png)

   ![](Git使用手册\3.1.png)

4. 选择save private key，将私钥另存为ppk格式的秘钥

   ![](Git使用手册\4.png)

   ![](Git使用手册\4.1.png)

5. 开始菜单找到TortoiseGit菜单下的pageant，打开

   ![](Git使用手册\5.png)

6. 点击add key，选择ppk秘钥，输入密码

   ![](Git使用手册\6.png)

   ![](Git使用手册\6.1.png)

   每次开机启动pagent并添加ppk秘钥

7. 点击torise git – settings 

   ![](Git使用手册\7.png)

8. 设置network—ssh的路径，设置为“C:\Program Files\TortoiseGit\bin\TortoisePlink.exe” 

   ![](Git使用手册\8.png)

#### 6. 使用过程遇到的问题

1. **在新文件下拉取远程仓库报错：You asked to pull from the remote 'origin', but did not specify:a branch. Because this is not the default configured remotefor your current branch, you must specify a branch on the command line.**

   找到：`.git/config`文件 添加如下

   ```
   [branch "master"]
       remote = origin
       merge = refs/heads/master
   ```

2. **在设置git远端地址时，尽量修改远端名称** 

   ![](C:\Users\banana\Pictures\微信截图_20180327222554.png)

   如果不设置的话，在同一个文件夹下，新建两个文件夹，在这两个文件夹中分别拉取不同仓库的内容时就会报错

3.  **当新建了一个空的远程仓库，本地创建了一个关联到远端地址的仓库后，不要尝试拉取，否则会报出`Couldn't find remote ref master`错误信息**

4. **idea使用Git时，需要配置ssh**

   - **设置私钥地址**

   ![](C:\Users\banana\Pictures\微信截图_20180327233928.png)

   ​

   - **设置Git地址和SSH executable 为Native**

   ![](C:\Users\banana\Pictures\微信截图_20180327234038.png)

   - **设置github地址**

     ![](C:\Users\banana\Pictures\微信截图_20180327234303.png)

5. **对于重命名改名字等操作都可以在本地，修改完后添加进版本控制再push到远端仓库即可**

6. 对于在第2步中修改了本地远端的名称的操作，如果在idea中拉取代码的话，回报出这个错误：

   ```
   Can't Update
   			No tracked branch configured for branch master or the branch doesn't exist.
   			To make your branch track a remote branch call, for example,
   			git branch --set-upstream-to origin/master master (show balloon)
   ```

   此时，可以执行命令`git branch --set-upstream-to origin-repo/master master` 意思是使我们在git设置的本地远程名称`origin-repo/master`追踪远程仓库的`master`分支

   ​

   ​