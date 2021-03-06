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

### 常用命令
```shell
# 将本地代码提交到暂存库
git add .
# 添加提交信息
git commit -m '提交的信息'
# 拉取代码 origin 为远程仓库别名    master 为分支名
git pull origin master
# 提交代码 origin 为远程仓库别名    self 为分支名
git push origin self
# 切换到 主分支 一般都是 master 具体根据项目来
git checkout master
# 将自己的分支合并到主分支---注意这一步，需要保证自己的代码没有问题或者冲突解决完以后再合并
git merge self
# 提交代码 origin 为远程仓库别名  master 为分支名
git push origin master
# 切换回自己的分支
git checkout self
# 切换并创建分支
git checkout -b xxx
# 查看本地所有分支
git branch
# 查看所有分支，包括远程
git branch -a
# 查看远程所有分支
git branch -r
# 删除本地分支---注意，需要先切换到别的分支，才能删除这个分支
git branch -D xxx
# 删除远程仓库分支
git push origin --delete xxx
```

### 拓展
```shell
# 查看关联信息 即上面说的 origin 别名
git remote -v
# 删除关联
git remote rm 远程别名
# 新增关联
git remote add 别名 远程仓库地址
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
# 查看用户名和邮箱
git config user.name
git config user.email
# 修改用户名和邮箱地址
git config user.name "yourname"
git config user.email "youremail"
```

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
### 修改提交时间
偷摸摸的，不要告诉别人哦～
```shell
# 时间随便改～
git commit --amend --date="Thu Dec 10 00:00:01 2022 -0700"
```

## 关于vue路由中的history模式刷新404问题
### node服务器解决方法
在node配置文件中添加以下代码
```js
// 先安装这个插件 connect-history-api-fallback
// 导入处理history模块的模块
const history = require('connect-history-api-fallback')
// 注册处理 history 模式的中间件
app.use(history())
```
### nginx服务解决方法
找到`nginx.conf`文件,修改配置
```conf
server {
    ...
    location / {
        root html;
        index index.html index.htm
        # 添加下面一段代码,就可以解决history模式刷新404问题
        try_files $uri $uri/ /index.html
    }
    ...
}
```

## 去除数据中所有字符串的前后空格
const trimData = data => {
  const isNumber = num => typeof num === 'number'
  const isString = str => typeof str === 'string'
  const isFn = fn => typeof fn === 'function'
  const isArray = arr => arr instanceof Array
  const isObject = obj => typeof obj === 'object' && obj instanceof Object
  let changeV = obj => {
    let a = Object.entries(obj).map(item => [item[0], trimD(item[1])])
    return Object.fromEntries(a)
  }
  const trimD = data => {
    if (isNumber(data) || isFn(data)) return data // 数字或函数类型
    if (isString(data)) return data.trim() // 字符串类型
    if (isArray(data)) return data.map(item => trimD(item)) // 数组类型
    if (isObject(data)) return changeV(data)// 对象类型
  }
  return trimD(data)
}

