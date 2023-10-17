title: 配置一个新的 git 环境

date: 2023-10-17 18:01:58

categories:

- git的使用

tags:

- git

---

# Configuration for a new git environment

## User

```shell
git config --global user.name "John Doe"
```

## Email

```shell
git config --global user.email johndoe@example.com
```

## Editor

```shell
git config --global core.editor vim
```

<!--more-->

## Disable pager

```bash
git config --global pager.branch false
git config --global pager.log false
```

## Check the current configuration

```shell
git config --list
```

## Timezone

```shell
tzselect
```

## Clone the repository

## Add the remote upstream
