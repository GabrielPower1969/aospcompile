# aospcompile

#1、安装 Git
sudo apt-get install git
#2、设置git身份，添加自己的邮箱和姓名
git config --global user.email "xxxx@qq.com"
git config --global user.name "xxxx"
#3、创建bin，并加入到PATH中
mkdir ~/bin
PATH=~/bin:$PATH
#4、安装curl库
sudo apt-get install curl
#5、下载repo并设置权限
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo > ~/bin/repo
chmod a+x ~/bin/repo
# 6、安装python，repo初始化时会用到
sudo apt-get install python
# 7、安装 jdk11
sudo apt-get update
sudo apt-get install openjdk-11-jdk
# 8、安装编译需要的依赖包
sudo apt install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip
# 9、编译需要有libncurses.so.5
sudo vi /etc/apt/sources.list 
#在行尾添加下面这个源保存
deb-src http://us.archive.ubuntu.com/ubuntu/ xenial main universe
sudo apt install apt-file
sudo apt-file update
sudo apt-file find libncurses.so.5  #没有这个文件再执行下面的。
sudo apt install libncurses5
# 10、创建工作目录
mkdir aosp_12
cd aosp_12
#repo的运行过程中会尝试访问官方的git源更新repo自己,如果想使用tuna的镜像源进行更新,可以将如下内容添加到你的~/.bashrc里
echo 'export REPO_URL="https://mirrors.tuna.tsinghua.edu.cn/git/git-repo/"'>> ~/.bashrc
source ~/.bashrc
# 11、初始化并指定版本
repo init -u https://aosp.tuna.tsinghua.edu.cn/platform/manifest -b android-12.0.0_r1
# 12、同步源码 -j后面的数字一般为cpu核心数的2倍，我的cpu为8核，这里我这设置的12。
repo sync -j12
# 13、源码下载成功后 cd进入AOSP的目录依次执行如下2个命令：
source build/envsetup.sh
make clobber #编译前删除build文件夹
# 14、选择编译目标
lunch
# 或者
lunch sdk_phone_x86_64
# 15、开始编译
make -j12
# 16、编译 启动模拟器
source build/envsetup.sh
lunch sdk_phone_x86_64  
emulator -gpu off -partition-size 3048
