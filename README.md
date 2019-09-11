# exprimental-methods
欢迎大家来到课程网站。本课程为课堂内容的补充，提供在线演示、发布作业等功能

## 注意
1. 考虑到服务器性能及ip访问限制，数据分析在学生个人电脑上完成。
2. 为确保环境一致性，应用docker技术发布数据环境镜像。
3. 原则上可以自行搭建数据分析环境，但不提供过多支持。
4. 严格按照规范进行操作，由于违规操作引起的数据丢失概不负责！

## 数据分析框架搭建

1. 前期准备及配置
  * windows 
    * 安装docker https://www.runoob.com/docker/windows-docker-install.html
    * 拉取镜像。进入cmd或者powshell执行`docker pull yfs2018/jupyroot:v1`
    * 安装chocolatey，并安装xsv。(https://chocolatey.org/install) 安装好chocolatey后，在cmd或powershell执行 `cinst xsv`或`choco install xsv` 【此步若出现问题先行跳过，执行后续步骤】
    * 安装Xming https://xming.en.softonic.com
    * 获取默认交换机的ip https://zhidao.baidu.com/question/1433778156685956859.html
  * linux
    * ubuntu安装docker https://www.runoob.com/docker/ubuntu-docker-install.html
    * centos安装docker https://www.runoob.com/docker/centos-docker-install.html
    * 拉取镜像。进入terminal执行`docker pull yfs2018/jupyroot:v1`
   
  * macOS https://www.runoob.com/docker/macos-docker-install.html
    * homebrew安装方法 https://brew.sh/index_zh-cn
    * homebrew 安装 xquartz和socat     ` brew cask install xquartz` 和 `brew install socat`
    * x11转发 `socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"`
    * 获取本机ip https://jingyan.baidu.com/article/b0b63dbf3fefd14a48307013.html

 2. 进入数据分析环境
  * windows
  ```bash
  docker run -it -p 8000:8888 -e DISPLAY=用默认交换机的ip地址替换:0.0 -v 用本机存放数据文件位置路径代替:/notebook  yfs2018/jupyroot:v1 
  ```
    * windows下本机路径要把反斜线改成斜线，如C:\Windows 更改为C:/Windows
  * linux
  ```bash
  xhost + && docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY -p 8000:8888 -v 用本机存放数据文件位置路径代替:/notebook yfs2018/jupyroot:v1
  ```
  * macOS
  ```bash
  xhost + && docker run -it -p 8000:8888 -e DISPLAY=(用本机ip替代):0.0 -v 用本机存放数据文件位置路径代替:/notebook yfs2018/jupyroot:v1
  ```
 
5. 分析环境包含内容：
   - root6.16.00 compiled with python3
   - python3
   - jupyter,ipython...
   

## Q&A
- 为什么使用docker技术？
```
docker 可以确保每个人搭建的数据环境是一致的。且能够极大地降低环境搭建的门槛和复杂性。
```
- 我能在docker中做什么？
```
do whatever you want. 镜像中给大家提供的是root权限，可以安装绝大部分linux系软件，并无太多限制。但是退出该环境后，对系统做出的改变会丢失。
```
- 我如何才能保留自己安装的软件和处理的数据不丢失，下次启动的时候依旧可以使用？
```
对于处理的数据文件：
可以通过挂载本地目录到docker中，这样在挂载目录做出的改变会直接影响本机目录内容。
对于自己安装的软件：
参阅docker commit命令，打包自己的镜像即可。
```
- 运行docker提示我权限不足怎么办？
```
此问题一般出现在linux上。由于使用sudo命令安装docker，当使用一般用户运行时会提示权限不足。需要在docker前加sudo命令。
若要避免此问题，可执行`sudo gpasswd -a ${USER} docker`，将当前用户加入到docker组，并修改相关文件权限。
随后重启docker服务即可。若提示docker组不存在，则`sudo groupadd docker`即可。
```
- xhost + 命令执行出错，类似于`xhost:  unable to open display "/private/tmp/com.apple.launchd.2Aoz4r2ZnI/org.macosforge.xquartz:0"`的提示怎么处理？
```
`xhost +` 是使所有用户都能访问`Xserver`. 
为了避免数据库软件安装过程中会出现0.0的错误，应该执行`xhost +`,当使用`xhost +`报错时，`export DISPLAY=:0.0`,再执行即可.
```
- jupyter-notebook 密码？
  `dataana`

- 配置docker的流程太过复杂，能提供虚拟机版本么？
```
访问 https://pkuenp.quickconnect.cn/d/s/509132606843953153/gnOF_Y4_Q13MvT-ajMjsqxGZf9TPOdjf-DLtAuHr2EAc_
下载镜像文件，密码为dataana。
下载后双击导入virtualbox虚拟机即可。
```

## contact me
- email：yuanfangsee@pku.edu.cn
- phone: 18511281625
- wechat: mission-young
