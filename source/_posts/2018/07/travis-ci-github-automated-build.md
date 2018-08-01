---
title: Travis CI & Github 自动化构建
date: 2018-07-27
updated: 2018-07-27
categories: 
    - 运维
---

## 起因

个人文档（ http://docs.flc.io/ ）： 源文件托管至 Github （ https://github.com/flc1125/docs ）平台。但每次更新文档后都需要手动执行 `mkdocs gh-deploy` 发布至 `gh-pages` 分支才能访问。

为节省此操作和对环境软件的依赖，于是用起 Travis CI 持续化集成。

## Github Token

> 用于 `Travis` 加入 `Github` 的授权使用

Token 创建地址: https://github.com/settings/tokens；勾选 `repo` 下所有选项。该 Token 不可暴露

{% asset_img slug 1.png %}

## Travis CI

{% asset_img slug 2.png %}

如图勾选状态；点击 `settings` 进行配置

{% asset_img slug 3.png %}

如图：

- A 输入变量名（名字随意）: `GITHUB_TOKEN`
- B 输入上文创建的 `Token`
- **C 禁用，否则会在构建过程中暴露 Token 信息**

## .travis.yml

在托管平台创建 `.travis.yml` 文件。写入下文（仅适用本文 docs 项目，具体自行研究）

```yaml
language: python
cache: pip

python:
  - "3.6"

install:
  - pip install mkdocs
  - pip install mkdocs-material
  - pip install pygments
  - pip install pymdown-extensions
  - echo -e "machine github.com\n  login ${GITHUB_TOKEN}" > ~/.netrc # 这个研究了好久

script:
  - mkdocs gh-deploy --force --clean

branches:
  only:
    - master
```

> 如此操作后，即可实现自动化构建；内容略粗糙，仅为了记录用。

## 相关文档

- Travis加入Github认证操作：https://docs.travis-ci.com/user/customizing-the-build/#authentication
- 官方文档：https://docs.travis-ci.com/
- GitHub Pageshttps://pages.github.com/