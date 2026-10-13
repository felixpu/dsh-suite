# dsh-suite

DSH（DeepSeek Harness）插件合集 —— **聚合包**，一次安装即获得以下五件套。

## 包含的插件

| 插件 | 作用 |
|---|---|
| [`dsh-lock`](https://github.com/felixpu/dsh-lock) | 密码门禁：打开 DSH 需输入密码解锁，支持手动/自动锁定 |
| [`dsh-settings-params`](https://github.com/felixpu/dsh-settings-params) | 设置分页：集中管理注入宿主的环境变量 |
| [`dsh-env-inspector-plus`](https://github.com/felixpu/dsh-env-inspector-plus) | 设置分页：宿主进程身份与环境变量分组概览（值不出宿主） |
| [`dsh-restart-autostart`](https://github.com/felixpu/dsh-restart-autostart) | 进程控制：重启按钮 +「跟随系统启动」 |
| [`dsh-archive-session-manager`](https://github.com/felixpu/dsh-archive-session-manager) | 归档会话管理 |

## 安装

```sh
dsh plugin --profile web add github:felixpu/dsh-suite
```

装完后在「设置 → 插件」里会看到 **dsh-suite** 一个分组，展开即五个插件。
若想单独安装某个插件，直接装它自己的仓库即可（本包不强制）。

## 这是什么形态：聚合包，不是「合并代码」

本包 **不含任何功能代码**（`lib/index.js` 只有一行 `export {}`），它的全部内容是一份
`cordis.patch.yml`，用一个 `cordis:group` 行把上面五个独立插件装配成一组。

这样做的理由：

| | 聚合包（本仓库） | 把代码合并进一个包 |
|---|---|---|
| 功能代码 | 仍在各自仓库，独立发版 | 全部挤在一处 |
| **故障隔离** | ✅ 单个插件出错只影响自己那一行 | ❌ 任一处出错**全部功能一起失效** |
| 独立升级 | ✅ 各包单独发版，本包无需改动 | ❌ 不可能 |
| 排障 | ✅ 定位到具体插件 | ❌ 混杂在一起 |

故障隔离这一点有实测依据：平台的行是独立加载的，而**一个插件的 `apply` 抛错会导致它自己激活失败**。若五份代码在同一包里，同一类错误会一次拖垮全部功能——包括锁屏。

> 另有一条平台硬约束：客户端 bundle 的 `id` 必须等于包名。各插件的客户端半身各有自己的 id，物理上无法合并进同一个 bundle。

## 自定义

想增删组合里的插件，直接改 `cordis.patch.yml` 的嵌套列表；每个条目的：

- `id`：本组合内的短名，仅用于定位与在其他 patch 层里覆盖它；
- `name`：**必须是真实包名**，且要在 `package.json` 的 `dependencies` 里声明。

## 许可证

MIT
