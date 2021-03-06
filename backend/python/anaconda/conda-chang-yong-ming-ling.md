---
cover: >-
  https://images.unsplash.com/photo-1650152965112-ab2a88d6e15f?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NTA4NjQzMTk&ixlib=rb-1.2.1&q=85
coverY: 0
---

# ð¡ condaå¸¸ç¨å½ä»¤

## çæ¬å·

```shell
conda --version
conda -V
```

## æä¸å½ä»¤çå¸®å©

```shell
conda update --help
conda remove --help
```

## ç¯å¢ç®¡ç

å¸®å©å½ä»¤ï¼å¨é¨ï¼

```shell
conda env -h
```

åå»ºç¯å¢(çæ¬ï¼åå§å)

```shell
conda create --name your_env_name python=3.8 numpy scipy
```

åä¸¾å½åææç¯å¢

```shell
conda info --envs
conda env list
```

è¿å¥ï¼éåºæä¸ªç¯å¢

```shell
activate your_env_name
deactivate 
```

å¤å¶æä¸ªç¯å¢

```shell
conda create --name new_env_name --clone old_env_name 
```

å é¤æä¸ªç¯å¢

```shell
conda remove --name your_env_name --all
```

## åäº«ç¯å¢

éè¿`activate target_env`è¦åäº«çç¯å¢`target_env`ï¼ç¶åè¾å¥ä¸é¢çå½ä»¤ä¼å¨å½åå·¥ä½ç®å½ä¸çæä¸ä¸ª`environment.yml`æä»¶ï¼æ¿å°`environment.yml`æä»¶åï¼å°è¯¥æä»¶æ¾å¨å·¥ä½ç®å½ä¸ï¼å¯ä»¥éè¿ä»¥ä¸å½ä»¤ä»è¯¥æä»¶åå»ºç¯å¢ã

```shell
conda env export > environment.yml
conda env create -f environment.yml
```

## åç®¡ç

åä¸¾å½åæ´»è·ç¯å¢ä¸çææå

```shell
conda list
```

åä¸¾ä¸ä¸ªéå½åæ´»è·ç¯å¢ä¸çææå

```shell
conda list -n your_env_name
```

ä¸ºæå®ç¯å¢å®è£æä¸ªå

```shell
conda install -n env_name package_name
```

## æ·»å /å é¤ éå

æ·»å éå

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ãã  #  æ·»å æ¸åéå2

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ãã# æ·»å æ¸åéå2

conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ãããããã  # æ·»å ä¸­ç§å¤§éå
```

å é¤éå

```shell
ããconda config --remove channels 'https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/'
```

è®¾ç½®æç´¢æ¶æ¾ç¤ºchannelså°å

```shell
ããconda config --set show_channel_urls yes
```

æ¥çchannelsæ¯å¦æ·»å /å é¤æå

```shell
conda config --show-sourcesãã# æ¹å¼ä¸

conda config --get channelsãã# æ¹å¼äº
```

## åèæç®

* [Condaå¸¸ç¨å½ä»¤æ´ç\_CharlotteMençåå®¢-CSDNåå®¢\_condaå½ä»¤](https://blog.csdn.net/menc15/article/details/71477949)
* [condaå¸¸ç¨å½ä»¤ - PRO\_Z - åå®¢å­ (cnblogs.com)](https://www.cnblogs.com/nbk-zyc/p/13055904.html)
