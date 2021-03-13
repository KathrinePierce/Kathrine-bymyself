# 无人值守安装镜像实验

## 实现特性

   #### ·定制一个普通用户名和默认密码
   #### ·定制安装 OpenSSH Server

## 主要操作步骤

#### 1、下载纯净版 Ubuntu 安装镜像 iso 文件,并计算对照校验和

![下载iso文件](/chap0x01/img/download-iso.png)

#### 2、手动安装ubuntu并得到一个初始的「自动配置描述文件」

       （1）设置网卡

![设置网卡](/chap0x01/img/set-network.png)
       
       
       （2）选择下载好的安装镜像：

![选择安装镜像](/chap0x01/img/choose-iso.png)

       （3）手动安装ubuntu20.04，并查看ip地址
       
![安装成功](/chap0x01/img/install-success.png)

       （4）使用ssh远程访问虚拟机
       将/var/log/installer/autoinstall-user-data拷贝到本地

             ·修改属主并查看文件

![修改属主并查看文件](/chap0x01/img/chown+cat.png)

             ·下载文件到本地
scp cuc@192.168.56.108:/var/log/installer/autoinstall-user-data ./


![下载自动配置描述文件](/chap0x01/img/download.png)

#### 3、对照 Ubuntu 20.04 + Autoinstall + VirtualBox 中提供的示例配置文件 酌情修改

![修改user-data文件](/chap0x01/img/correct.png)

#### 4、使用sftp命令将user-data与自建的meta-data文件传输到手动安装的虚拟机里

     ·sftp> put C:\Users\14128\Downloads\user-data /home/cuc
![传输](/chap0x01/img/sftp-put.png)

#### 5、创建 cloud-init 镜像

    （1）安装genisoimage软件包
![安装软件包](/chap0x01/img/install-genisoimage.png)
    （2）创建cloud-init镜像
    
      ·genisoimage -input-charset utf-8 -output init.iso -volid cidata -joliet -rock user-data meta-data

![创建镜像](/chap0x01/img/create.png)

#### 6、把镜像下载回本机

       ·get /home/cuc/init.iso C:\Users\14128\Downloads
![下载iso](/chap0x01/img/download%20init.iso.png)

#### 7、新建focal-auto-Kathrine虚拟机并按顺序挂载光盘

![按顺序挂载光盘](/chap0x01/img/mount-cd.png)

#### 8、启动focal-auto-Kathrine虚拟机，完成无人值守安装

![手动输入yes](/chap0x01/img/yes.png)

## 实验中遇到的问题

#### 1、手动安装好ub20.04之后，ssh远程访问虚拟机错误（我远程访问了第一节课导入老师的ova的虚拟机ip地址）
      ·解决办法：重新做一遍

#### 2、github无法访问
      ·解决方法：使用vpn翻墙（免费软件安装网站：
https://www.baacloud34.com/shiyong.php）

#### 3、不知道如何实现虚拟机与本机间的文件传输
      ·解决方法：找到了一个sftp指令教程的神仙网站
https://www.cnblogs.com/afeige/p/12144296.html
#### 4、忘记下载genisoimage软件包
      ·解决方法：sudo apt install genisoimage

#### 5、忘记配置network-config.yaml文件
      ·解决方法：把自己手动安装的ubuntu里的-config.yaml下载到本机并稍作修改。修改后同meta-data、user-data一起上传至虚拟机，然后使用genisoimage命令将三个文件合成iso镜像。再将合成好的镜像下载回主机。即可得到366KB的镜像文件。
https://github.com/c4pr1c3/LinuxSysAdmin/tree/master/exp/cloud-init/docker-compose

#### 6、因为没有修改user-data文件名而导致无人值守镜像制作失败
      ·解决方法：下载修改autoinstall-user-data之后，将该文件名改为user-data



