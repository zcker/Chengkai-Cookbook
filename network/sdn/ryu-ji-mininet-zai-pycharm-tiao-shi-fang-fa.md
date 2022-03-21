---
cover: >-
  https://images.unsplash.com/photo-1645740589710-296aecb29532?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDc4NDMzMDc&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🚔 Ryu及Mininet在pycharm调试方法

## Ryu调试方法

新建文件**debug\_ryu.py**，再直接在需要的地方打上断点运行即可

```python
# !/usr/bin/env python
# -*- coding: utf-8 -*-

import sys

from ryu.cmd import manager


def main():
    # 用要调试的脚本的完整路径取代/home/zck/桌面/mininet-wifi/ryu/ryu/app/delay.py就可以了
    sys.argv.append('/home/zck/桌面/mininet-wifi/ryu/ryu/app/delay.py')
    sys.argv.append('--verbose')
    sys.argv.append('--observe-links')
    sys.argv.append('--enable-debugger')
    manager.main()


if __name__ == '__main__':
    main()

```

## Mininet调试方法

### 方法1

由于mininet可以从python文件直接运行，输入以下字符即可：

```shell
sudo python 文件.py
```

1. 找到你python的位置，如 **/usr/bin/python3.9**
2. 在桌面或任意位置新建**python\_sudo.sh**

```shell
#!/bin/bash
sudo /usr/bin/python3.9 "$@"
```

3\. 赋予其可执行权限

```shell
chmod +x python_sudo.sh.sh
```

4\. 在pycharm的菜单设置（file->settings）项中,设置项目的intepreter为**python\_sudo.sh**文件

5\. debug即可

### 方法2

![](<../../.gitbook/assets/image (13).png>)

## 参考资料

* [https://its401.com/article/dongdao8186/102416423](https://its401.com/article/dongdao8186/102416423)
