# 鲤源池

鲤源池是面向华南理工大学的微信生态校园互助平台。本仓库作为项目总入口，通过 Git 子模块统一管理需求设计、Go 后端、Web 管理端和微信小程序，记录各模块配套的提交版本。

首发目标是文字许愿互助闭环，涵盖学生认证、心愿发布与审核、心愿广场、应愿、临时聊天、完成与还愿，以及必要的运营管理。悄悄鲤、独立搭子、鲤币等属于延期范围；具体需求与审批状态以 [需求与设计工作台](docs/README.md) 中的材料为准。

## 仓库结构

| 子模块 | 职责 | 主要技术 |
| --- | --- | --- |
| [docs](docs/README.md) | 需求、设计、审批、评审、实施计划与共享接口契约 | Markdown、Python 检查脚本 |
| [liyuanci-backend-go](liyuanci-backend-go/README.md) | API 与独立 worker，承载服务端业务 | Go、PostgreSQL、Redis、RocketMQ |
| [liyuanci-admin-vue](liyuanci-admin-vue/README.md) | Web 运营与管理入口 | Vue 3、TypeScript、Vite、Pinia |
| [liyuanci-miniprogram-uniapp](liyuanci-miniprogram-uniapp/README.md) | 微信小程序用户端 | uni-app、Vue 3、TypeScript、Pinia |

当前处于工程基础与会话接入准备阶段，完整业务闭环尚未交付。后端认证和领域业务仍待实现；两端已有部分会话逻辑与开发模拟能力，真实 API、微信登录及业务流程仍需联调验收。各模块的实际实现、验证结果与未完成依赖见各自 README；接口定义不代表接口已实现。

## 获取项目

需要 Git，并配置具有这些仓库访问权限的 GitHub SSH 密钥。

```sh
git clone --recurse-submodules git@github.com:calm195/liyuanci.git
cd liyuanci
```

如果已经克隆根仓库，补充初始化子模块：

```sh
git submodule update --init --recursive
```

更新根仓库及其记录的子模块版本前，先确认各工作区没有未提交修改：

```sh
git pull --ff-only
git submodule update --init --recursive
git submodule status
```

子模块按根仓库记录的提交检出，通常处于 detached HEAD 状态。开始开发前，在对应子模块中切换或创建开发分支。

## 本地开发

各子项目独立安装依赖、运行和检查，完整环境要求与配置说明见对应 README 和 AGENTS.md。

| 模块 | 开发入口 | 质量检查 |
| --- | --- | --- |
| Go 后端 | `make run` | `make fmt`、`make check`、`make build` |
| Web 管理端 | `pnpm install --frozen-lockfile`、`pnpm dev` | `pnpm format`、`pnpm check`、`pnpm build` |
| 微信小程序 | `npm ci`、`npm run dev:mp-weixin` | `npm run check`、`npm run build:mp-weixin` |

以上命令均在对应子模块目录执行。后端使用 Go 1.25 或更新版本；管理端使用 pnpm 10，小程序使用 Node 22.13 或更新版本，详细版本约束以子项目配置为准。小程序开发构建后，使用微信开发者工具导入 `dist/dev/mp-weixin`。

共享接口契约入口为 [docs/contracts/README.md](docs/contracts/README.md)。本地环境配置按子项目示例填写，密钥、真实令牌和本地凭据不得提交。

## 子模块协作

子项目的代码与历史保存在各自独立仓库中。修改子项目时，先在其仓库完成检查、提交并推送，再回到根仓库提交新的子模块指针，保证其他人可以获取对应版本。例如：

```sh
# 子项目的提交已推送后，在根目录记录配套版本
git add liyuanci-backend-go
git commit -m "chore: update backend submodule"
git push origin main
```

根仓库的 `git push` 不会自动推送子项目提交。日常同步使用 `git submodule update --init --recursive` 获取已记录版本；升级模块版本时，应先在子模块中明确选择并验证目标提交，再更新根仓库指针。
