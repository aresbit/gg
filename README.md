# gg

[![GitHub CI](https://github.com/aresbit/gg/workflows/CI/badge.svg)](https://github.com/aresbit/gg/actions)
[![MPL-2.0 license](https://img.shields.io/badge/license-MPL--2.0-blue.svg)](LICENSE)

`gg` (原名为 **Z**ero **B**ullshit **G**it) 是一个高效使用 `git` 的 CLI 工具。

## Features

* ✨ Prettier `git status`, `git log` and other commands
* 🚀 Sane defaults to `git` commands that enforce good state of your local repository
* 🌌 Achieving more by typing less

| `gg status` | `gg log` |
| --- | --- |
| ![zbg status](./images/zbg-status-demo.png) | ![zbg log](./images/zbg-log-demo.png) |

> [!IMPORTANT]
> `gg` 由志愿者在空闲时间开发和维护
> by volunteers. The development may continue for decades or may stop
> tomorrow. You can use
> [GitHub Sponsorship](https://github.com/sponsors/aresbit) to support
> the development of this project.

## Install

> [!NOTE]
> 目前 `gg` 只能通过[从源码构建](#从源码安装)来安装。
> 其他安装方法[可能会在以后提供](https://github.com/aresbit/gg/issues/8)。

### 从源码安装

1. 克隆仓库
    ```shell
    git clone git@github.com:aresbit/gg.git
    cd gg
    ```
2. 构建可执行文件
    ```shell
    opam install . --deps-only --with-doc --with-test
    dune build
    ```
3. 将可执行文件复制到 `$PATH` 中的位置，例如：
    ```shell
    cp -f ./_build/default/bin/main.exe ~/.local/bin/gg
    ```
4. 运行 `gg --help`

## Configure

Set your GitHub username in the `user.login` field of your global `.gitconfig`:

```shell
git config --global user.login my-github-login
```

## Quick start guide

This section contains examples of various usage scenarios for `zbg`.

### Developing a new feature

A typical workflow for developing a new feature may look like this:

```shell
$ zbg switch           # Switch to the main branch and  sync it

$ zbg new Feature 2.0  # Create a new branch; 'zbg switch' made sure
                       # to start developing from the latest version

<coding>               # The fun part!

$ zbg status           # Check a quick summary of your changes

$ zbg commit           # Commit all local changes and write a description

$ zbg push             # Push the branch

$ zbg done             # Switch to the main branch and delete the feature branch locally
```

### Experimenting, saving and re-applying

While you were on vacation, a coworker of yours pushed some changes to your
branch, and now your want to sync their work locally and do some experimenting.

```shell
$ zbg sync     # Get the latest changes from your branch

$ zbg rebase   # Rebase on top of the main branch if anything changed

<start developing the experiment>

$ zbg clear    # Nah, scrap that, bad idea

<coding>

$ zbg stash    # Argh, too early, save and do the other thing

<coding>

$ zbg status   # Check what you've done

$ zbg commit   # Commit changes

$ zbg unstash  # Now apply your stashed changes

$ zbg commit   # Commit changes again

$ zbg push -f  # Push changes for the review
```

## All commands

The below table describes all `zbg` commands and the corresponding `git`
explanations.

> [!NOTE]
> The table below uses default or example arguments to all commands

| `zbg` | `git` |
| ----- | ----- |
| `zbg clear` | `git add .` <br /> `git reset --hard` |
| `zbg commit My description` | `git add .` <br /> `git commit --message="My description"` |
| `zbg done` | `git switch main` <br /> `git branch --delete --force <branch we're switching from>` |
| `zbg log` | `git log <long custom formatting string>` |
| `zbg new Branch name` | `git switch --create my-github-username/Branch-name` |
| `zbg push` | `git push --set-upstream origin <current-branch>` |
| `zbg rebase` | `git fetch origin main` <br /> `git rebase origin/main` |
| `zbg stash` | `git stash push --include-untracked` |
| `zbg status` | `git status` (but `zbg` is prettier) |
| `zbg switch` | `git switch main` <br /> `git pull --ff-only --prune` |
| `zbg sync` | `git pull --ff-only origin <current-branch>` |
| `zbg sync -f` | `git fetch origin <current-branch>` <br /> `git reset --hard origin/<current-branch>` |
| `zbg tag v0.1.0` | `git tag --annotate %s --message='Tag for the v0.1.0 release'` <br /> `git push origin --tags` |
| `zbg tag -d v0.1.0` | `git tag --delete v0.1.0` <br /> `git push --delete origin v0.1.0` |
| `zbg uncommit` | `git reset HEAD~1` |
| `zbg unstash` | `git stash pop` |

The following table provides the comparison of `zbg` and `git` outputs.

| `zbg` | `git` |
| ----- | ----- |
| `zbg status` <br /> ![zbg status](./images/zbg-status-comparison.png) | `git status` <br /> ![git status](./images/git-status-comparison.png) |
| `zbg log` <br /> ![zbg log](./images/zbg-log-comparison.png) | `git log` <br /> ![git log](./images/git-log-comparison.png) |

> [!WARNING]
> `zbg` is not a full `git` replacement! It provides a streamlined workflow for
the most common case but you may still need to use `git` occasionally.

## For contributors

Check [CONTRIBUTING.md](https://github.com/aresbit/zbg/blob/main/CONTRIBUTING.md)
for contributing guidelines.

## Development

**First time:** Install all dependencies with `opam`:

```
opam install . --deps-only --with-doc --with-test
```

To build the project locally, use `dune`:

```
dune build
```

To run tests:

```
dune runtest
```

> [!TIP]
> Empty output means that all tests are passing.

## Troubleshooting

If you see the following error when running `zbg switch`:

```shell
$ zbg switch
fatal: ambiguous argument 'origin/HEAD': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
```

run the following command to sync with the remote and set the `origin/HEAD` ref locally:

```shell
git remote set-head origin -a
```
