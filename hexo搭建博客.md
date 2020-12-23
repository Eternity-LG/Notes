### 最好是提前下载好git

当hexo init运行时报错WARN git clone failed. Copying data instead

WARN Failed to install dependencies. Please run 'npm install' manually!

下载安装Git 然后在git bash命令行里面进行hexo init就可以了

### git安装步骤：

###### 1.git安装好去GitHub上注册一个账号，注册好后，点击桌面上的Git Bash快捷图标，我们要用账号进行环境配置

###### 2.

```
# 配置用户名
git config --global user.name "username"    //（ "username"是自己的账户名，）
# 配置邮箱
git config --global user.email "username@email.com"     //("username@email.com"注册账号时用的邮箱)

```

###### 3.生成SSH

```
ssh-keygen -t rsa
```

然后连敲三次回车键，结束后去系统盘目录下（一般在 C:\Users\你的用户名.ssh）(mac: /Users/用户/.ssh）查看是否有。ssh文件夹生成，此文件夹中以下两个文件

![image-20200203121500292](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20200203121500292.png)

###### 4. 将ssh文件夹中的公钥（ id_rsa.pub）添加到GitHub管理平台中，在GitHub的个人账户的设置中找到如下界面

![image-20200203121658471](C:\Users\LIU\AppData\Roaming\Typora\typora-user-images\image-20200203121658471.png)

将公钥（ id_rsa.pub）文件中内容复制粘贴到key中，然后点击Ass SSH key就好啦

###### 5.查看ssh

```
ssh -T git@github.com
```



### 安装说明

###### node安装

###### 新建空文件夹进行cnpm安装

###### 同一文件夹下用cnpm install -g hexo-cli进行hexo-cli安装

###### 新建文件夹用于存放blog文件(基础操作的文件夹)

###### hexo init初始化blog

###### hexo s 部署成功返回localhost:4000链接地址

###### hexo n 新建blog

###### hexo clean

###### hexo g 生成

##### cnpm install --save hexo-deployer-git下载部署插件

##### 修改文件内的配置

```
 vi _config.yml
 type: git
 repo: https://github.com/eternity-lg/eternity-lg.github.io.git
 branch: master
```

##### （更换主题）

```
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia

```



###### hexo d部署到远程服务github.io

