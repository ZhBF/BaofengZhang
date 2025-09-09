---
title: "Python常见报错记录"
date: "2025-09-08T21:21:06+08:00"
categories: "Python"
tags: ["Python"]
author: "Baofeng Zhang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "记录在Python使用中经常遇到的问题，便于日后再次遇到相同问题时快速查阅。"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>"
    alt: "<alt text>" 
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## Setuptools

在2024到2025年期间，安装一些老版本的包时遇到报错可尝试将setuptools回退到62.0.0，能解决相当一部分问题。

记得pip和wheel也要进行相应的回退。

## Sacred 

### MongoObserver

运行遇到报错

TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'

```python
INFO - my_experiment - Running command 'my_main_function'
ERROR - my_experiment - Failed after 0:00:00!
Traceback (most recent call last):
  File "c:\Users\zhang\.conda\envs\temp\lib\runpy.py", line 194, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "c:\Users\zhang\.conda\envs\temp\lib\runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\launcher/../..\debugpy\__main__.py", line 71, in <module>
    cli.main()
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\launcher/../..\debugpy/..\debugpy\server\cli.py", line 501, in main
    run()
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\launcher/../..\debugpy/..\debugpy\server\cli.py", line 351, in run_file
    runpy.run_path(target, run_name="__main__")
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\_vendored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 310, in run_path
    return _run_module_code(code, init_globals, run_name, pkg_name=pkg_name, script_name=fname)
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\_vendored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 127, in _run_module_code
    _run_code(code, mod_globals, init_globals, mod_name, mod_spec, pkg_name, script_name)
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\_vendored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
    _run_code(code, mod_globals, init_globals, mod_name, mod_spec, pkg_name, script_name)
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\_vendored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
  File "c:\Users\zhang\.cursor\extensions\ms-python.debugpy-2025.8.0-win32-x64\bundled\libs\debugpy\_vendored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
dored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
dored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
  File "C:\MyData\Projects\Nagoya\pymarl2\baofeng\ex.py", line 28, in <module>
dored\pydevd\_pydevd_bundle\pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
  File "C:\MyData\Projects\Nagoya\pymarl2\baofeng\ex.py", line 28, in <module>
    ex.run()
    exec(code, run_globals)
  File "C:\MyData\Projects\Nagoya\pymarl2\baofeng\ex.py", line 28, in <module>
    ex.run()
  File "c:\Users\zhang\.conda\envs\temp\lib\site-packages\sacred\experiment.py", l  File "C:\MyData\Projects\Nagoya\pymarl2\baofeng\ex.py", line 28, in <module>
    ex.run()
  File "c:\Users\zhang\.conda\envs\temp\lib\site-packages\sacred\experiment.py", line 277, in run
    run()
  File "c:\Users\zhang\.conda\envs\temp\lib\site-packages\sacred\run.py", line 235, in __call__
    self._emit_started()
  File "c:\Users\zhang\.conda\envs\temp\lib\site-packages\sacred\run.py", line 326, in _emit_started
    _id = observer.started_event(
  File "c:\Users\zhang\.conda\envs\temp\lib\site-packages\sacred\observers\mongo.py", line 259, in started_event
    self.insert()
  File "c:\Users\zhang\.conda\envs\temp\lib\site-packages\sacred\observers\mongo.py", line 368, in insert
    c.next()["_id"] + 1 if self.runs.count_documents({}, limit=1) else 1
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

可能是因为有脏数据，需确认 MongoDB 连接正常，且 sacred.runs 集合没有脏数据。

有些AI说可能与pymongo的版本有关，经过尝试，不能解决问题。

尝试在Mongo Shell中运行以下命令能解决问题。

```powershell
use sacred
db.runs.deleteMany({})
db.runs.insertOne({"_id": 1, "dummy": true})
```

### MongoObserver Omniboard

sacred实验使用mongodb记录并可视化查看。

```python
ex.observers.append(MongoObserver.create(
	url="mongodb://localhost:27017",  # MongoDB连接URL
	db_name="pymarl",  # 数据库名称
))
omniboard -m localhost:27017:database
http://localhost:9000/
```

## Gfootball

### Gfootball运行测试报错

安装gfootball后运行测试时遇到报错

```python
RuntimeError: Dynamic linking causes SDL downgrade! (compiled with version 2.28.4, linked to 2.0.16)
```

利用Everything找到位于python环境包中的SDL2.dll，会发现pygame中的版本高于gfootball中的版本，删除或重命名更老的版本，即可解决。

### Gfootball安装编译失败

Gfootball在python3.11需要重新编译，容易失败。

在3.8下可以直接安装，不需编译。（应该是3.6到3.10都可以。）

## Gym

### Gym版本问题

gym<=0.17.3和gym>=0.22.0一般可以直接安装

gym==0.18.0~gym==0.21.0 可能需要对安装工具降级

- pip==24.0

- setuptools==65
- wheel==0.37.1

gym==0.21.0的返回值符合老版接口。

gym==0.22.0的返回值符合新版接口。

gym==0.24.0尽量不用，有官方警告。

## Tensorboard

### Tensorboard查看实验记录报错

```python
TypeError: MessageToJson() got an unexpected keyword argument
```

tensorboard版本太高导致，安装tensorboard==2.12.0可以解决。
