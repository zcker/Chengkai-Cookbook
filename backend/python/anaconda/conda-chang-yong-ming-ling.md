---
cover: >-
  https://images.unsplash.com/photo-1650152965112-ab2a88d6e15f?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NTA4NjQzMTk&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 📡 conda常用命令

## 版本号

```shell
conda --version
conda -V
```

## 某一命令的帮助

```shell
conda update --help
conda remove --help
```

## 环境管理

帮助命令（全部）

```shell
conda env -h
```

创建环境(版本，初始包)

```shell
conda create --name your_env_name python=3.8 numpy scipy
```

列举当前所有环境

```shell
conda info --envs
conda env list
```

进入，退出某个环境

```shell
activate your_env_name
deactivate 
```

复制某个环境

```shell
conda create --name new_env_name --clone old_env_name 
```

删除某个环境

```shell
conda remove --name your_env_name --all
```

## 分享环境

通过`activate target_env`要分享的环境`target_env`，然后输入下面的命令会在当前工作目录下生成一个`environment.yml`文件，拿到`environment.yml`文件后，将该文件放在工作目录下，可以通过以下命令从该文件创建环境。

```shell
conda env export > environment.yml
conda env create -f environment.yml
```

## 包管理

列举当前活跃环境下的所有包

```shell
conda list
```

列举一个非当前活跃环境下的所有包

```shell
conda list -n your_env_name
```

为指定环境安装某个包

```shell
conda install -n env_name package_name
```

## 添加/删除 镜像

添加镜像

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/　　  #  添加清华镜像2

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/　　# 添加清华镜像2

conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/　　　　　　  # 添加中科大镜像
```

删除镜像

```shell
　　conda config --remove channels 'https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/'
```

设置搜索时显示channels地址

```shell
　　conda config --set show_channel_urls yes
```

查看channels是否添加/删除成功

```shell
conda config --show-sources　　# 方式一

conda config --get channels　　# 方式二
```

## 参考文献

* [Conda常用命令整理\_CharlotteMen的博客-CSDN博客\_conda命令](https://blog.csdn.net/menc15/article/details/71477949)
* [conda常用命令 - PRO\_Z - 博客园 (cnblogs.com)](https://www.cnblogs.com/nbk-zyc/p/13055904.html)
