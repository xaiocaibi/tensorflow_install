Source:https://blog.csdn.net/makeyy123/article/details/81069392
This is the best one i have found,but i have a trouble on the pip.
sudo pip3 install --upgrade pip 
i use this order to update pip to latest.and then,pip don't find out the right path,
sudo pip3 install /home/nvidia/tensorflow-1.3.0-cp35-cp35m-linux_aarch64.whl
this order solves the problem.
when you finish ./installPrerequisites.sh
you need change the java version to 8u121.article has method.

JetsonTX2上安装tensorflow的心酸史
还是那句话，做事情得有耐心，有耐心…耐心….心……感觉像是给自己的一个心理暗示… -。-|||

tensorflow安装
常见问题总结
验证
tensorflow1.3.0安装
好的，进入正文，本文安装的是tensorflow1.3.0，使用的是源码编译安装，python2.7，cuda8.0，cudnn6.0.2.

本文介绍的tensorflow均是python2.7版本的安装，如果你需要python3.5+版本的tensorflow，同理得,你会看到python2.7的tensorflow中含有cp27字符，python3.5的tensorflow中含有cp35字符。 
TX2上安装tensorflow比较苦*，因为tx2上是arm开发板，不像x86的机子直接pip或者anaconda安装。

TX2安装方式有两种，这两种安装方式都需要安装java 、bazel等这些依赖 
如果这些流程你都走一遍了，基本上1.3.0以上的版本安装也就找到规律了： 
1. 使用别人已经编译好的.whl文件来安装 
2. 源码编译安装

注意： 
安装类似tensorflow的开源框架都是需要版本匹配的，下面会给大家提供一个简要的版本匹配

tensorflow不同版本的环境需求
目前支持tensorrt的tensorflow>=1.7 参考网址：https://github.com/NVIDIA-Jetson/tf_trt_models (you can get .sh file in this work)
截止目前为止，TX2支持的最高tensorflow1.9.0是最近才update出来的

—Tensorflow 1.8.0 is built on Jetson TX2 with the environment: 
L4T 28.23 (JetPack 3.2) 
CUDA 9.0 
cuDNN 7.0.5 
TensorRT 3.0

—Tensorflow 1.6.0 is built on Jetson TX2 with the environment: 
L4T 28.1 (JetPack 3.1) 
CUDA 8.0 
cuDNN 6.0.21

—Tensorflow 1.5.0 is built on Jetson TX2 with the environment: 
L4T 28.1 (JetPack 3.1) 
CUDA 8.0 
cuDNN 6.0.21

Tensorflow 1.4.1 is built on Jetson TX2 with the environment:

L4T 28.1 (JetPack 3.1) 
CUDA 8.0 
cuDNN 6.0.21

注意: 
方法1的安装确实比较方便，但是能用别人预编译好的whl文件装上的人很少，至少我是遇到了很多踩不平的坑，所以决定用的方式2，自己编译安装的，中间遇到的坑也都踩平了。 
如果你一定要用方式1来安装的话，那么坑来了，请做准备！你必须要把我说的下面的这些依赖安装了！！！一定要！！！

方式1.使用别人已经编译好的.whl文件来安装（不推荐）：
(1). java

注意！！！ 
这里用下面的方式安装的java版本是java官网更新的java8的jdk的最新版本，比如我现在安装的jdk就是jdk8的最新版本8U171，如果要按照版本来安装jdk的话，可以按照问题4中的解决方法来安装

sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer

(2). bazel

这里先附上bazel官方网址 ：https://docs.bazel.build/versions/master/install-ubuntu.html，官方网址提供了多种安装方式，按照你的喜好来装，随便是.shell脚本或者源码编译安装都可以。允许坐着装、躺着装、跪着装…….皮一下，哈哈哈 
bazel官方提供的下载地址：https://github.com/bazelbuild/bazel/releases?after=0.10.0 超级喜欢官方这个词，因为不用踩坑。。。~_~

下面提供的是官网二进制（源码编译）安装方式，给大家指个路先：

Step 1: Install required packages
First, install the prerequisites: pkg-config, zip, g++, zlib1g-dev, unzip, and python.

sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python
Step 2: Download Bazel
Next, download the Bazel binary installer named bazel-<version>-installer-linux-x86_64.sh from the Bazel releases page on GitHub.

Step 3: Run the installer
Run the Bazel installer as follows:

chmod +x bazel-<version>-installer-linux-x86_64.sh
./bazel-<version>-installer-linux-x86_64.sh --user
The --user flag installs Bazel to the $HOME/bin directory on your system and sets the .bazelrc path to $HOME/.bazelrc. Use the --help command to see additional installation options.

Step 4: Set up your environment
If you ran the Bazel installer with the --user flag as above, the Bazel executable is installed in your $HOME/bin directory. It's a good idea to add this directory to your default paths, as follows:

export PATH="$PATH:$HOME/bin"
You can also add this command to your ~/.bashrc file.

Note: Bazel includes an embedded JDK, which can be used even if a JDK is already installed. bazel-<version>-without-jdk-installer-linux-x86_64.sh is a version of the installer without an embedded JDK. Only use this installer if you already have JDK 8 installed. Later JDK versions are not supported.
(3). other stuff

sudo apt-get install zip unzip autoconf automake libtool curl zlib1g-dev maven -y
sudo apt-get install python-numpy swig python-dev python-pip python-wheel -y


(4). 安装tensorflow.whl文件

可以去这里下载tensorflow1.3.0 https://download.csdn.net/download/makeyy123/10544283，这个是我预编译好的版本，这里的java是8U121，bazel0.5.4，python2.7 
tensorflow1.8.0 https://download.csdn.net/download/makeyy123/10544295 
这里的pip安装可能会触发问题1/2，解决方法已经附上 
这些本来是想open source给大家的，但是发现csdn上传后至少需要1个积分。 
如果你没有积分又想下载的话，可以留下你的邮箱，我看到会发到你的邮箱里

pip install tensorflow.whl

方式2.源码编译安装（推荐）：
本文介绍的主要方法是使用源码编译安装，不过比较省心的一点是，所有的shell命令都写在shell脚本文件中了，所以一次执行脚本文件就可以了。

脚本文件中已经包含了java和bazel的安装 
脚本文件下载链接：https://download.csdn.net/download/makeyy123/10544320

需要环境：cuda8.0 cudnn6.0

一、Preparation
TX2共有8G运行内存和32G eMMc flash 看起来倒是挺多的，但是装上系统，ROS，OPENCV，Qt后基本所剩余无几了，现在我就教大家如何将硬盘分区并挂载到/home目录下。我使用的是samsung250G的固态硬盘 
如果插上之后没有自动弹出，参考：https://www.youtube.com/watch?v=KxZ-e6G7INg按照步骤把硬盘挂载，或者直接在大环境下搜索disks，下图右边的工具 
这里写图片描述 
会看到一个250G的硬盘，点设置进行format，这时在文件下就可以找到这个盘文件了

接下来挂载到/home下： 
1.查看硬盘所有分区。

 sudo fdisk -lu

会有一个/dev/sda 就是你所接入的硬盘

２、对硬盘进行分区。

sudo fdisk /dev/sda

在Command (m for help)提示符后面输入n，执行 add a new partition 指令给硬盘增加新分区。出现Partition number(1-4)时，输入１表示只分一个区。（所加硬盘比较大的话可以多分几个） 
后续指定起启柱面（First sector）,默认起始地址为 2048，结束地址为：1953525167，不输入数字按ENTER，将填入默认值。 
在Command (m for help)提示符后面输入w，保存分区表。 
输入quit退出 
再次输入

sudo fdisk /dev/sda

显示/dev/sda1 则表示分区完成

3、格式化分区为ext4

sudo mkfs -t ext4 /dev/sda1 

4、挂载硬盘分区 
先把新硬盘挂在一个临时目录下

cd /mnt/
sudo mkdir home
sudo mount /dev/sda1 /mnt/home  挂载到/mnt/home
df -h  查看
sudo cp -a /home/* /mnt/home/   把home下的东西拷到挂载的目录下，备份
sudo rm -rf /home/*             把home下的东西删干净
sudo umount /dev/sda1           卸载硬盘
df -h                           查看

5、设置开机挂载

sudo vi /etc/fstab
末尾增加一行:
/dev/sda1  /home  ext4  defaults  1  2

保存退出

df -h          查看 /home是否被挂载

mount -a       挂载/etc/fstab 中未挂载的分区
df -h  查看

二、 开辟一块8G的编译空间，否则会报内存error
进入到下载好的脚本文件夹中，一次执行：

 ./createSwapfile.sh -d ~/ -s 8
1
三、 安装依赖
for python2.7 
1.安装java、bazel

./installPrerequisites.sh

2.克隆aarch64的tensorflow1.3.0

./cloneTensorFlow.sh

3.导入tensorflow需要的python lib path

./setTensorFlowEV.sh

选择/usr/local/lib/python2.7/dist-packages

for python3.5 
1.安装java、bazel

./installPrerequisitesPy3.sh

2.克隆aarch64的tensorflow1.3.0

./cloneTensorFlow.sh

3.导入tensorflow需要的python lib path

./setTensorFlowEVPy3.sh

选择/usr/local/lib/python3.5/dist-packages

四、安装tensorflow1.3.0
build tensorflow
./buildTensorFlow.sh

这里写图片描述

2.生成whl文件,生成的文件在$HOME下面存放

./packageTensorFlow.sh

3 . 使用pip安装whl文件

for python2.7

pip install $HOME/wheel_file
或 python -m install $HOME/wheel_file

for python3.5

pip3 install $HOME/wheel_file
或 python3 -m install $HOME/wheel_file

这里pip安装会访问之前设置的python lib的path，所以需要管理员权限 
如果报一些permission error，将3中所以命令前加sudo

验证
Python2.7
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))

效果：

这里写图片描述

问题1：pip问题
如果出现pip==8.0.1，安装需要pip==10.0.0的问题， 
解决：这里需要更新一下pip的版本，不更新的话，是无法安装whl文件的

问题2：pip import main error
pip安装过程出现 import main error 
解决: 
修改/usr/bin/pip或/usr/bin/pip3,改过之后保存，就可以用pip/pip3了

from pip import main
if __name__ == '__main__':
    sys.exit(main())

改为：

from pip import __main__
if __name__ == '__main__':
    sys.exit(__main__._main())

问题3：./buildTensorflow.sh出现下图问题
解决：搜索了一圈，大家都说是bazel版本10.0.0过高，把bazel版本降为0.5.4，不过我提供的脚本下载的是bazel0.5.4，因此应该不会出现下图问题 
这里写图片描述

问题4：把bazel换成0.5.4后，./installPrerequisites.sh出现下图问题
这里写图片描述 
原因 ：javajdk版本过高，查看自己的java版本是jdk_8U171，改成 jdk_8U121 
参考：https://blog.csdn.net/yebhweb/article/details/55098189

java -version

修改.bashrc文件后记得source一下，不然还会是之前的java版本

jdk是java开发工具的简称，下载官方网址： 
http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html 
TX2 64位选择 
这里写图片描述 
这里可以安装多个java版本，可以进行切换：

sudo update-alternatives –config java

无数次刷机后，安装这个的过程大概就是这些个常见的坑了，反正是被我全遇到了，一点都不遗憾，hhhhhh…..囧…..
