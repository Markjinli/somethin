# somethin

把源码级的对比分析做成网页,放这里。

**线上地址:https://markjinli.github.io/somethin/**

---

## 页面清单

目前 **2 个页面**。新增页面时请同步更新这张表。

| 页面 | 在线 | 源码 | 讲什么 |
| --- | --- | --- | --- |
| **FlClash 与 Verge 网络层对比** | [打开](https://markjinli.github.io/somethin/) | [`index.html`](index.html) | 两个开源代理客户端的 VPN 基本功能链路怎么搭的:核心引擎、控制面、TUN 虚拟网卡、提权、系统代理、DNS、路由。含一张控制面拓扑图 |
| **FlClash 与 Verge 许可证对比** | [打开](https://markjinli.github.io/somethin/licenses.html) | [`licenses.html`](licenses.html) | 两家的许可证、GPL-3.0 义务边界、上游 mihomo 的命名限制、血统来源与风险面。为"要不要在它们基础上做二次开发"服务。含一张"默认分支陷阱"机制图 |

两个页面顶部都有站内导航可以互相跳转。

### 第一个页面具体有什么

`index.html` 分六节:

| 节 | 内容 |
| --- | --- |
| 结论 | 一句话概括两家路线:**FlClash 自包含**(核心自 fork、协议自造、提权服务在仓库内)vs **Verge 薄编排**(上游原版核心、核心自带 API、提权服务在外部仓库) |
| 逐项对比 | 8 个维度的并排表:核心引擎、版本策略、控制面、配置下发、进程模型、提权产物、macOS 路线、系统代理 |
| 控制面拓扑 | 一张 inline SVG 图。两行方框坐标完全对齐,**只有箭头方向相反** —— 这就是两家在控制面上的全部区别 |
| 四个分歧点 | 核心自有还是上游 / 控制面谁说了算 / 提权产物在不在仓库里 / macOS 两条完全不同的路 |
| 两边都没写的 | TUN 网卡、路由表、DNS 解析、代理协议 —— 全部委托给 mihomo。附两边共同的失败模式 |
| 读取范围 | 三条**没有验证到**的东西,避免当成结论 |

### 第二个页面具体有什么

`licenses.html` 分八节:

| 节 | 内容 |
| --- | --- |
| 先更正一处 | **整页的头号发现。** 早前版本写过"核心是 MIT",是错的 —— `MetaCubeX/mihomo` 的默认分支 `main` 里是一个**同名的 Python 包**(MIT),真正的 Go 核心在 `Meta` 分支,**是 GPL-3.0**。GitHub 的徽章和 `/license` 接口只读默认分支,所以两边都报 MIT。附一张机制图 |
| 许可证全景 | 6 个组件从上到下的许可证表,每一项标注**核实状态**。结论:整条链路都是 GPL-3.0,许可证**不是**这两家的选型依据 |
| 可以 / 不可以 | GPL-3.0-only 下的义务边界。最容易踩的坑是"闭源分发二进制";另附一条边界情况:SaaS 不分发不触发 GPL |
| 命名限制 | mihomo 在 GPL 之外附加的条款原文 —— **下游项目名字里不许出现 `mihomo`**。含 Mihomo Party → Clash Party 的改名先例 |
| 血统时间线 | 2018 原版 Clash 到 2026 的时间线,重点是 2023-11 那次删库潮。结论:两家都接得干净,没有 DMCA、没有授权纠纷 |
| 两家风险面 | 许可证既然对等,差异全在这里:**FlClash 出货并执行一个 All rights reserved 的专有 exe**,**Verge 提交了一个 23MB 的苹果字体但零引用不出货**;外加贡献者集中度 3 vs 166 |
| 怎么选 | 按技术栈和长期打算的条件式建议,附动手前要清掉的东西 |
| 核实状态 | 把"我直接读到的"和"二手研究"分开列,并明确这不是法律意见 |

无构建步骤、无依赖。直接改 `index.html` / `licenses.html` 即可,推上来 Pages 会自动重建。

---

## 新增页面

1. 在仓库根目录建 `<页面名>.html`(多页面也可以建子目录放 `index.html`)
2. 在仓库根目录建 `<页面名>.md`,或在本文档的清单表里加一行
3. 页面清单表里补上**在线链接**和**这个页面讲什么** —— 一行说清,别只写标题
4. 推到 `main`,Pages 会自动发布

约定:

- 单文件 HTML,样式内联,不引外部依赖(字体除外)
- **必须是完整 HTML 文档** —— `<!doctype html>`、`<meta charset>`、`<meta name="viewport">` 一个都不能少。少 doctype 会进怪异模式,少 viewport 手机上会整体缩放
- 页面标题写具体名词短语,不要写"某某分析报告"这种通用名
- 涉及源码的结论都带上 `文件:行号`,让读者能自己核
- 两个主题都要能看:token 定义在裸 `:root`,暗色在 `@media (prefers-color-scheme: dark)` 里用 `:root:not([data-theme="light"])` 覆盖,再在 `:root[data-theme="dark"]` 覆盖一次

---

## 数据来源

| 项目 | commit | 日期 |
| --- | --- | --- |
| [FlClash](https://github.com/chen08209/FlClash) | `c7be702` | 2026-09-17 |
| [Clash Verge Rev](https://github.com/clash-verge-rev/clash-verge-rev) | `22e3f1a` | 2026-09-22 |

两个仓库都是**浅克隆**,只取了最新一版。文中所有 `文件:行号` 引用对应上面这两个 commit。

`licenses.html` 除了这两个 commit 的 `LICENSE` / manifest 文件,还用了 GitHub API 查到的仓库元数据(创建日期、archived 状态、贡献者统计),以及上游 `MetaCubeX/mihomo` **`Meta` 分支**(注意不是默认分支)的 LICENSE 和 README。

---

## 已知的读取限制

这几条在页面的"读取范围"一节里也写了,这里再列一次,免得有人把推断当结论:

- **FlClash 的 mihomo fork 没读到。** `core/Clash.Meta` 是 git 子模块,浅克隆是空目录。凡涉及核心内部(Windows TUN 网卡怎么建、路由怎么装)的说法,都是"客户端侧没有这段代码"的**推断**。
- **Verge 的服务端不在这个仓库里。** 提权服务在外部仓库 `clash-verge-service-ipc`,以预编译二进制引入,IPC 线格式细节没有源码可读。
- **两边的核心本身都没读源码。** FlClash 是子模块没拉;Verge 用的是上游 stock 二进制。涉及 mihomo 内部行为的部分只标注为"由核心负责",没有验证。

### 已修正的两处

- 网络层页面早期版本写过"两家都在 macOS 钉国内公共 DNS",后续复核发现 **Verge 那边只在 fake-ip 分支里触发**,配置显式写 `redir-host` 就不会钉。所以这是**行为不同**,不是相同做法。
- 许可证页面早期版本写过"核心是 MIT,所以 GPL 义务只来自客户端外壳",**这是错的**。`MetaCubeX/mihomo` 的默认分支 `main` 里是一个同名的 Python 包(MIT),真正的 Go 核心在 `Meta` 分支上,是 **GPL-3.0**。GitHub 的徽章和 `/repos/.../license` 接口**只读默认分支**,所以两边都报 MIT。`chen08209/Clash.Meta` 有同样的坑(实际用的是 `FlClash` 分支,也是 GPL-3.0)。
