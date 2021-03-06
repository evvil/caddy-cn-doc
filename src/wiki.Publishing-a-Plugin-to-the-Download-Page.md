---
date: 2018-10-01 15:59:15 +0800
title: "发布插件到下载页面"
sitename: "Caddy开发者wiki"
template: "wiki"
---

# 发布插件到下载页面

_<small>英文原文：<https://github.com/mholt/caddy/wiki/Publishing-a-Plugin-to-the-Download-Page></small>_

----------------------------

加入[Caddy交流论坛](https://caddy.community/)和其他开发者一起分享。

这个页面描述了如何将插件发布到Caddy网站上的下载页面。

## 发布之前

使用这个清单来确保你的插件已经准备好发布：

* __我的插件许可证与[Caddy许可证](https://github.com/mholt/caddy/blob/master/LICENSE.txt)兼容。__目前不允许GPL和商业许可。
* __如果我的插件向Caddyfile添加了任何指令，我已经向服务器类型的repo提交了一个pull请求，以将其按正确的顺序排列；而且这种变化已经被发布。__新指令需要被服务器类型知道，这样它们就可以按正确的顺序排列。例如，HTTP服务器的指令列表在[httpserver/plugin.go](https://github.com/mholt/caddy/blob/729e4f0239528e1c2ac2f64207aef769564fc697/caddyhttp/httpserver/plugin.go#L434)中。一旦您的PR被合并，你就应该等到这个变更发布之后，因为下载页面只会从发布版本中生成二进制文件。如果你在变更发布之前发布你的插件，即使你的插件被插入，你的用户还是不能使用你的指令。
* __文档可以在插件项目的README或网站上找到。__你的插件文档不在Caddy网站上；确保它易于查找、跟踪，而且是准确和完整的。你的插件在Caddy网站上的唯一文档是描述和一些例子。
* __这个插件在它自己的代码仓库中。__多个插件不应该共享一个仓库。这是构建插件的特定版本所必需的；如果两个插件共享一个仓库，但是构建需要两个不同的版本，那么构建将是不可能的。从技术上讲，两个插件可能共享一个仓库，前提是它们永远不会有互斥的版本，不管如何我们都不建议这样做。
* __我的插件通常很有用。__只对一到两个人有用的插件并不适合Caddy网站，可能会被删除。
* __我的插件和它的依赖不需要cgo。__Caddy和插件是通过禁用cgo来构建的，以确保能够跨平台编译。
* __我的插件的功能是独一无二的。__Caddy下载页面不是应用程序商店或市场；我们遵循“改进或删除”的理念。
* __我已经测试我的插件！__通过测试是非常重要的；如果插件测试失败，就不能部署它，而且它必须有测试。测试必须在非特权环境中通过，并且在合理的时间内通过。
* __我的插件是安全的。__它不会对它所运行的系统造成损害，且其代码非常严谨。(例如，它不会删除[所有文件](https://github.com/valvesoftware/steam-for-linux/issues/3671)或[销毁根目录](https://github.com/systemd/systemd/issues/5644)。)
* __我的插件不违反或干扰Caddy的目标、价值观、或愿景。__换句话说，它不监视用户、插入广告、破坏HTTPS、与DRM技术相关联等。插件必须符合Caddy及其用户的最大利益。
* __我同意积极维护我的插件。__这意味着项目所有者或协作者在合理的时间内对任何问题或pull请求做出响应，尽快应用安全修复，偶尔部署修复/更新，且程序包能有效地保持稳定的工作状态。

在Caddy网站上发布并不意味着得到Caddy项目或其作者、维护人员或贡献者的认可。用户承担所有风险，而你，开发人员，负责更新、安全和维护您自己的代码。如果插件不符合此标准或出于任何其他原因，可由Caddy维护人员删除插件。

## 展示你的插件

1. 如果你还没有帐号，需要先创建一个[Developer Portal帐户](https://caddyserver.com/account/register)。验证你的电子邮件地址，然后登录。
2. 点击“Register Plugin”。

    ![注册插件](https://c.dengxiaolong.com/caddy/register-account.png)
3. 按照每个字段的说明操作。注意：将每个插件放在自己的代码仓库中；如果不同的插件在同一个存储库中，版本控制就变得困难/不可能。
4. 单击“Next”（下一步）。你的仓库将被下载，试图找到你发布的插件。在列表中选择它。
5. 填写其余的字段；一个好的描述和例子对你的插件的成功是至关重要的！让描述的第一句话真正有意义。
6. 当你提交表单时，你的插件将被部署。部署由`go vet`检查、跨平台构建检查和`go test -race`组成。你的插件会立即出现在[文档](https://caddyserver.com/docs)页面上，但是一切都必须通过才能正常下载。

部署需要一段时间，甚至10-15分钟。你将通过电子邮件(或在你的帐户中禁用电子邮件通知)收到部署结果的通知。在当前部署完成之前不要再次进行部署。

## 更新你的插件

在任何时候，你都可以登录到Developer Portal并更新插件的描述、示例和链接。你还可以随意部署新版本。请保持你的插件是最新的！用户可以看到上一次更新是多久以前。同时请确保这些例子在任何时候都是简明、有用、翔实和准确的。

__一旦你的代码被发布，不要重置它的提交历史记录。__如果没有找到提交，构建服务器将无法通过`git pull`命令提取更改。如果发生这种情况，目前没有办法删除构建服务器的repo缓存。如果必须重置代码仓库，请联系Caddy维护者，让他们删除构建服务器上的文件夹。

感谢你发布插件！希望你享受成为社区的一员。
