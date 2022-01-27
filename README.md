# practical-method
收集一些实用小方法，后续整理到个人网站中～


## 如何在项目中生成项目目录结构树
1. 安装插件
```shell
# 因为在任何项目中都可能用到，所以就应该安装为全局
npm install mddir -g
```
2. 使用
> 在想要生成结构目录的项目的根目录运行命令
```shell
mddir
```
3. 结果会生成一个`directoryList.md`名字的文件
内部包含当前项目的整个项目结构

tips: 生成的项目结构最好用`代码块`包裹起来，防止预览格式错乱

## 如何处理服务启动时端口被占用情况

1. 打开服务终端，进入项目所在目录

2. 输入命令
```shell
netstat -anp
```

3. 找到自己的对应端口，记住后面 PID对应的数字

如：0.0.0.0:8080 端口后面对应的PID为 12345

4. 输入命令 kill -9 PID
例：kill -9 12345

5. 这样就将被占用的8080端口程序退出了。然后重新运行服务启动命令即可

## 设置npm镜像源
### 查看当前设置的镜像源
```shell
npm config get registry
```
### 设置npmjs源
```shell
npm config set registry=http://registry.npmjs.org
```
### 设置淘宝镜像源
```shell
npm config set registry http://registry.npm.taobao.org
```
### 如果在运行某些命令的时候，需要使用别的npm源，但是又不想更改全局可以使用下面的方法
```shell
# 添加 --registry 参数
# 举个例子 发布npm模块
yarn publish --registry https://registry.yarnpkg.com
# 这样可以为当前这条命令指定对应的npm源，又不会修改全局的
```

## 关于git命令
### 本地远程关联
```shell
# 查看关联信息
git remote -v
# 删除关联
git remote rm 远程别名
# 新增关联
git remote add 别名 远程仓库地址
```
### git用户名和邮箱地址
```shell
# 查看用户名和邮箱
git config user.name
git config user.email
# 修改用户名和邮箱地址
git config user.name "yourname"
git config user.email "youremail"
```

### 版本查看回退
```shell
# 查看版本号
git log
# 回退到上一个版本
git reset --hard HEAD^
# 回退到前3次提交之前，以此类推，回退到n次提交之前
git reset --hard HEAD~3
# 回退到指定版本
git reset --hard 目标版本号
# 强制推送
git push -f
```
