---
layout:     post
title:      "解决pycharm问题：module 'pip' has no attribute 'main'"
categories: "编程"
date:       2019-04-08 21:00:00
author:     "lem0n"
tags:
    - Python
typora-root-url: ..
---

在pycharm上面遇到了坑记下来，帮大家填坑

<!-- more -->

找到安装目录下 helpers/packaging_tool.py文件，找到如下代码：

```python
def do_install(pkgs):
    try:
        import pip
    except ImportError:
        error_no_pip()
    return pip.main(['install'] + pkgs)


def do_uninstall(pkgs):
    try:
        import pip
    except ImportError:
        error_no_pip()
    return pip.main(['uninstall', '-y'] + pkgs)
```

改为

```python

def do_install(pkgs):
    try:
        # import pip
        try:
            from pip._internal import main
        except Exception:
            from pip import main
    except ImportError:
        error_no_pip()
    return main(['install'] + pkgs)


def do_uninstall(pkgs):
    try:
        # import pip
        try:
            from pip._internal import main
        except Exception:
            from pip import main
    except ImportError:
        error_no_pip()
    return main(['uninstall', '-y'] + pkgs)
```

转载自：<http://www.cnblogs.com/Fordestiny/p/8901100.html>
