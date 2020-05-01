---
title:      Django 使用 Github Actions 
date:       2020-04-29
tags:
    - Django
    - Github Actions
---
#### 一、添加 Github Actions

在项目里创建 django.yml 文件
```bash
django_demo
|__.github
|   └── workflows
|       └── django.yml
├── demo
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   ├── urls.py
│   └── views.py
├── django_demo
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
├── requirements.txt
```

编辑 django.yml 文件
```yml
name: Django CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest
    strategy:
      max-parallel: 4
      matrix:
        python-version: [3.6, 3.7, 3.8]

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v1
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install Dependencies
      run: |
        make install_test
    - name: Run Tests
      run: |
        python manage.py test

```

给项目添加 Github Actions 运行情况图标

* 添加 README.md 文件
```bash
django_demo
|__.github
|   └── workflows
|       └── django.yml
├── README.md
...
```

* 编辑 README.md 文件添加图标
```
# django demo
![](https://github.com/xxx/django_demo/workflows/Django%20CI/badge.svg)
```
![](https://raw.githubusercontent.com/terrluo/terrluo.github.io/master/img/2020-04-29-Github-Actions.png)

#### 二、添加代码覆盖率测试
关联 coveralls

>在 github 的 Marketplace 里搜索 coveralls 选择 Open Source 选项关联 coveralls

安装 coverage
```bash
pip install coverage
pip freeze > requirements.txt
```

添加 coverage 配置文件
```bash
django_demo
|__.github
|   └── workflows
|       └── django.yml
├── README.md
├── .coveragerc
...
```

编辑 .coveragerc
```bash
[run]
source = .
relative_files = True

[report]
# Regexes for lines to exclude from consideration
exclude_lines =
    # Have to re-enable the standard pragma
    pragma: no cover

    # Don't complain about missing debug-only code:
    def __repr__
    if self\.debug

    # Don't complain if tests don't hit defensive assertion code:
    raise AssertionError
    raise NotImplementedError

    # Don't complain if non-runnable code isn't run:
    if 0:
    if __name__ == .__main__.:

omit =
    manage.py
    venv/*
    */migrations/*
    django_demo/urls.py
    django_demo/wsgi.py
```

运行 coverage 看下是否正常
```bash
coverage run manage.py test
# 输出的内容
System check identified no issues (0 silenced).

----------------------------------------------------------------------
Ran 0 tests in 0.000s

OK
```

```bash
coverage report
# 输出的内容
Name                       Stmts   Miss  Cover
----------------------------------------------
demo/__init__.py               0      0   100%
demo/admin.py                  1      0   100%
demo/apps.py                   3      0   100%
demo/models.py                 6      0   100%
demo/tests.py                  0      0   100%
demo/urls.py                   4      0   100%
demo/views.py                 24     14    42%
django_demo/__init__.py        0      0   100%
django_demo/settings.py       22      0   100%
----------------------------------------------
TOTAL                         56     14    80%
```

修改 django.yml 文件
```yml
name: Django CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest
    strategy:
      max-parallel: 4
      matrix:
        python-version: [3.6, 3.7, 3.8]

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v1
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install Dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run Tests
      run: |
        coverage run manage.py test
        coverage report
    - name: Coveralls Python
      uses: AndreMiras/coveralls-python-action@v20200413
```

编辑 README.md 文件添加覆盖率情况图标
```
# django demo
![](https://github.com/xxx/django_demo/workflows/Django%20CI/badge.svg)
[![Coverage Status](https://coveralls.io/repos/github/xxx/django_demo/badge.svg?branch=master)](https://coveralls.io/github/xxx/django_demo?branch=master)
```
![](https://raw.githubusercontent.com/terrluo/terrluo.github.io/master/img/2020-04-29-覆盖率.png)