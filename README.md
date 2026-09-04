# GEO 自动化 Skill 整合包

这是当前 GEO 实操手册与方法论的集中入口。以后找自动化流程、Skill、表格模板和报告模板，优先从这个文件夹进入。

本仓库包含 5 个可独立安装的 Codex Skill，覆盖客户诊断、关键词挖掘、文章生成、平台适配，以及豆包发布前后研究。所有 Python 脚本仅使用标准库；浏览器探测功能另需 Playwright 或 `playwright-cli`。

## 安装

将需要的 Skill 目录复制到 Codex 的本地 Skill 目录：

```bash
mkdir -p ~/.codex/skills
cp -R skills/doubao-geo-publisher ~/.codex/skills/
cp -R skills/geo-keyword-miner ~/.codex/skills/
cp -R skills/geo-article-writer ~/.codex/skills/
cp -R skills/geo-platform-adapter ~/.codex/skills/
cp -R skills/geo-doubao-research ~/.codex/skills/
```

安装后请在下一轮对话中调用，例如：`$geo-keyword-miner`。

从 GitHub 安装时，也可以使用 Codex 自带的 `skill-installer`，将仓库中的 `skills/<skill-name>` 作为安装路径。

## 环境要求

- Python 3.9 或更高版本。
- 生成关键词、Prompt、报告和平台版本不需要第三方 Python 包。
- `probe_doubao_surface.sh` 需要可运行的 `npx playwright`。
- `prepare_doubao_session.sh` 需要 `playwright-cli`，并始终保留人工登录和验证码处理。

## 文件夹结构

```text
GEO自动化Skill整合包/
├── README.md
├── skills/
│   ├── doubao-geo-publisher/
│   ├── geo-keyword-miner/
│   ├── geo-article-writer/
│   ├── geo-platform-adapter/
│   └── geo-doubao-research/
├── templates/
│   ├── GEO自动化流程表格模板.xlsx
│   ├── client_profile.sample.json
│   ├── article-brief.template.md
│   ├── effect-log.template.md
│   ├── competitor-source-log.template.md
│   └── comparison-feedback.template.md
└── docs/
    ├── GEO实操手册自动化说明.md
    └── 资料文件索引.md
```

## Skill 分工

| Skill | 用途 |
| --- | --- |
| `doubao-geo-publisher` | 主流程 Skill，串起客户诊断、关键词、写稿、平台改写、监测复盘 |
| `geo-keyword-miner` | 从客户档案 / 公司家底表生成 GEO 关键词库 |
| `geo-article-writer` | 根据关键词、证据、来源生成五类平台文章 Brief 和可发布 Markdown 主稿 |
| `geo-platform-adapter` | 把主稿改成头条、搜狐、知乎、36氪等平台版本 |
| `geo-doubao-research` | 做豆包发布前诊断、发布后检测、前后对比、竞品信源反推 |

## 自动化双链路

### 客户流程

1. 创建客户档案。
2. 生成关键词库。
3. 用豆包做发布前诊断。
4. 生产文章并做平台改写。
5. 发布后再次用豆包检测。
6. 生成发布前后对比反馈。

### 内部复盘流程

1. 收集豆包引用的竞品和信源。
2. 分析来源文章标题、结构、FAQ、证据方式。
3. 提炼行业模板。
4. 更新关键词规则、文章 Brief、标题模板和信源策略。

## 会产生哪些表格

核心表格集中在 `templates/GEO自动化流程表格模板.xlsx`：

| 表 | 用途 |
| --- | --- |
| `公司家底六格表` | 公司信息、核心服务、目标客户、资质荣誉、真实案例、核心卖点 |
| `表一_客户档案` | 客户ID、品牌、行业、业务类型、服务城市、EEAT、竞品、媒体素材 |
| `表二_关键词库` | 核心关键词、意图层级、优先级、公式类型、推荐文章类型、平台建议 |
| `表三_文章任务` | 文章类型、目标关键词、平台标题、文章链接、引用来源、发布状态 |
| `表四_监测追踪` | 查询词、AI 软件、是否出现目标主体、引用来源、截图、下一步动作 |
| `自动化节点` | N1-N6 自动化流程节点 |
| `平台规则` | 头条、搜狐、知乎、36氪改写规则 |
| `文件素材索引` | 当前资料与自动化节点的对应关系 |

此外，`geo-doubao-research` 会产出 Markdown 结构化报告：

- 发布前诊断报告
- 发布后检测报告
- 发布前后对比反馈
- 竞品信源反推报告

## 推荐使用顺序

1. 先读 `docs/GEO实操手册自动化说明.md`。
2. 打开 `templates/GEO自动化流程表格模板.xlsx` 填客户档案。
3. 用 `geo-keyword-miner` 生成关键词。
4. 用 `geo-doubao-research` 做发布前诊断。
5. 用 `geo-article-writer` 和 `geo-platform-adapter` 生产与改写内容；正式成稿必须遵守五类文章规范。
6. 发布后再用 `geo-doubao-research` 生成检测和对比报告。

## 重要边界

- 第一版只默认豆包。
- 浏览器自动化只做有头半自动。
- 登录、验证码、账号安全、最终发布都保留人工确认。
- 竞品文章只做结构分析，不直接复制或改写成我方文章。
- 测试词和验收词必须从当前客户关键词库动态抽取，不使用固定 demo 词。
- 正式正文不得出现 `客户资料显示`、`补充材料显示`、`实体链`、`GEO` 等内部交付痕迹。
- 写稿必须先回答用户真实问题，再在中后段自然引出客户 / 品牌 / 人物 / 机构。
- `geo-article-writer/references/article-generation-rules.md` 是对外分享时的写稿硬规则。

## 验证

发布前已执行以下检查：

- 5 个 Skill 的 `SKILL.md` frontmatter 与目录名校验。
- Python 语法编译、Shell 语法检查、JSON 解析和 XLSX 容器完整性检查。
- 使用仓库自带示例档案跑通客户诊断、关键词计划、Prompt、监测计划、关键词 CSV、豆包研究报告和闭环演示。

## 许可证

本项目使用 [MIT License](./LICENSE)。使用者仍需自行确认平台规则、广告合规、事实来源与第三方内容授权。
