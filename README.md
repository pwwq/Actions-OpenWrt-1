**English** | [中文](https://p3terx.com/archives/build-openwrt-with-github-actions.html)


## SSH 连接到 Actions

* 在`Run Workflow`时把`SSH connection to Actions`的值改为`true`
* 在触发工作流程后，在 Actions 日志页面等待执行到SSH connection to Actions步骤
* 复制 SSH 连接命令粘贴到终端内执行，或者复制链接在浏览器中打开使用网页终端。（网页终端可能会遇到黑屏的情况，按 Ctrl+C 即可）

```
cd openwrt && make menuconfig
```

* 完成后按Ctrl+D组合键或执行exit命令退出，后续编译工作将自动进行。

## 添加额外的软件包

* 在 DIY 脚本中加入对指定软件包源码的远程仓库的克隆指令。就像下面这样：

```
git clone https://github.com/P3TERX/xxx package/xxx
```

* 本地make menuconfig生成.config文件时添加相应的软件包，如果你知道包名可以直接写到.config文件中。

> TIPS: 如果额外添加的软件包与 Open-Wrt 源码中已有的软件包同名的情况，则需要把 Open-Wrt 源码中的同名软件包删除，否则会优先编译 Open-Wrt 中的软件包。这同样可以利用到的 DIY 脚本，相关指令应写在diy-part2.sh。

原理是把软件包源码放到 package 目录下，编译时会自动遍历，与本地编译是一样的。当然方法不止一种，其它方式请自行探索。

## 源码更新自动编译

在检测到源码更新后自动进行编译。

* [创建Personal access token](https://github.com/settings/tokens/new) 名字为`ACTIONS_TRIGGER`并且勾选`repo`权限，这将用于自动触发编译工作流程。
* 然后点击自己仓库的`Settings`选项卡，再点击`Secrets`。添加名为`ACTIONS_TRIGGER_PAT`的加密环境变量，保存刚刚创建的 PAT 。
* 在 `Actions` 页面选择`Update Checker`，点击`Run workflow`手动进行一次测试运行。如果没有报错且 OpenWrt 编译工作流程被触发，则代表测试通过。
* 最后编辑`Update Checker`的 `workflow` 文件`.github/workflows/update-checker.yml`，取消注释（删除#）定时触发相关的部分。这里可以根据 cron 格式来设定检测的时间，时区为 UTC 。

```
#  schedule:
#    - cron: 0 */18 * * *
```

此外还可以根据实际情况对监视的源码仓库进行更改，如果有多个源码仓库需要监视则多复制几份相应的 work-flow 文件。

# Actions-OpenWrt

[![LICENSE](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square&label=LICENSE)](https://github.com/P3TERX/Actions-OpenWrt/blob/master/LICENSE)
![GitHub Stars](https://img.shields.io/github/stars/P3TERX/Actions-OpenWrt.svg?style=flat-square&label=Stars&logo=github)
![GitHub Forks](https://img.shields.io/github/forks/P3TERX/Actions-OpenWrt.svg?style=flat-square&label=Forks&logo=github)

A template for building OpenWrt with GitHub Actions

## Usage

- Click the [Use this template](https://github.com/P3TERX/Actions-OpenWrt/generate) button to create a new repository.
- Generate `.config` files using [Lean's OpenWrt](https://github.com/coolsnowwolf/lede) source code. ( You can change it through environment variables in the workflow file. )
- Push `.config` file to the GitHub repository.
- Select `Build OpenWrt` on the Actions page.
- Click the `Run workflow` button.
- When the build is complete, click the `Artifacts` button in the upper right corner of the Actions page to download the binaries.

## Tips

- It may take a long time to create a `.config` file and build the OpenWrt firmware. Thus, before create repository to build your own firmware, you may check out if others have already built it which meet your needs by simply [search `Actions-Openwrt` in GitHub](https://github.com/search?q=Actions-openwrt).
- Add some meta info of your built firmware (such as firmware architecture and installed packages) to your repository introduction, this will save others' time.

## Credits

- [Microsoft Azure](https://azure.microsoft.com)
- [GitHub Actions](https://github.com/features/actions)
- [OpenWrt](https://github.com/openwrt/openwrt)
- [Lean's OpenWrt](https://github.com/coolsnowwolf/lede)
- [tmate](https://github.com/tmate-io/tmate)
- [mxschmitt/action-tmate](https://github.com/mxschmitt/action-tmate)
- [csexton/debugger-action](https://github.com/csexton/debugger-action)
- [Cowtransfer](https://cowtransfer.com)
- [WeTransfer](https://wetransfer.com/)
- [Mikubill/transfer](https://github.com/Mikubill/transfer)
- [softprops/action-gh-release](https://github.com/softprops/action-gh-release)
- [ActionsRML/delete-workflow-runs](https://github.com/ActionsRML/delete-workflow-runs)
- [dev-drprasad/delete-older-releases](https://github.com/dev-drprasad/delete-older-releases)
- [peter-evans/repository-dispatch](https://github.com/peter-evans/repository-dispatch)

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © [**P3TERX**](https://p3terx.com)
