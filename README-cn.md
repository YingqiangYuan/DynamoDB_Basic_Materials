# DynamoDB + pynamodb 入门教程

**Amazon DynamoDB** 是 AWS 上最重要的 NoSQL 数据库，为高并发、低延迟、无限水平扩展而设计。**pynamodb** 是目前最优雅的 Python ORM——把表写成 Python 类，代码量只有原生 boto3 的三分之一。本教程通过 12 个循序渐进的 POC 脚本，带你从一个最小的 Model 定义，一路走到 Single-Table Design 的一对多、多对多建模。所有示例使用金融科技信用卡领域（AxiomCard），跑在真实 AWS 账号上。

---

## 这是什么——30 秒说清方法论

这是一个 **learn-this-project** 风格的仓库：一个刻意做小、做窄的代码库，把**一项垂直技能**端到端教透。目标不是让你"跑通这份代码"，而是让你**吃透**这项技能——能跑、能读、能为每一个设计决策辩护，最后在你自己的 GitHub 上有一份 portfolio 版本。

整个流程由 6 个交互式 skill 串起来：

- **`/learn-this-project-absorb`** —— 项目的 on-call mentor。多模式：Orient（给你地图 + `files to READ` / `files to RUN/DO` 分类）、Context-dive（你贴一个 `file:line`，它就地拆给你看）、Next-step（推荐下一步看什么）、Build（帮你在仓库里加东西）。是 mentor，不是教程——你卡住、迷路、想动手的时候去找它，而不是从头坐到尾。
- **`/learn-this-project-quiz`** —— 讨论式问答。每个回答按**3 段式标准**评分：**在哪查 + 是什么 + 为什么**。一句话事实正确不算过。两种模式：预生成题库（保底）+ 开放生成模式（你指定话题，它当场出题）。
- **`/learn-this-project-elevate`** —— 这个仓库再投入 3–6 个月会长成什么样。对每个升级方向走一遍：当前状态 → 资深工程师目标 → 备选方案 → 前置知识，并**收敛成一个具体的起手交付物**（例如"加一个 `tests/test_examples.py` 跑通每个脚本"），你可以直接把它扔回 Absorb Build 模式去真正实现。
- **`/learn-this-project-interview`** —— 整项目模拟面试，带 pushback。测的是你能不能在陌生人面前为自己的设计辩护。
- **`/learn-this-project-demo`** —— 把这个项目演给别人看的脚本和彩排。最关键的不是"展示哪里"，而是"**不要展示哪里**"那个 cardinal-rule 清单。
- **`/learn-this-project-publish`** —— 把这份教学仓库转成你自己的 portfolio 版本。删教学痕迹、生成 commit 备忘单（你手动 `git commit`，skill 不碰 git）、用你自己的语气共写 README、最后跑一遍 hostile-scan audit 兜底。

**推荐顺序**：absorb → quiz → elevate → interview → demo → publish。但这些 skill 是 mentor on call，需要的时候叫，不要当成线性教程来走。

---

## 这个仓库里有什么

```
.
├── examples/                          # 12 个循序渐进的模块（核心学习内容）
│   ├── 00-minimal-poc/                #   最小可运行示例（黄金骨架）
│   ├── 01-attributes/                 #   属性类型：标量 / 集合 / JSON
│   ├── 02-table-management/           #   建表 / 描述表 / 计费模式
│   ├── 03-crud-basic/                 #   save / get / update / delete / refresh
│   ├── 04-batch-operations/           #   batch_write / batch_get / UnprocessedItems
│   ├── 05-query-and-scan/             #   query vs scan、分页、排序
│   ├── 06-condition-expression/       #   条件写入、乐观锁
│   ├── 07-transactions/               #   TransactWrite / TransactGet
│   ├── 08-gsi-and-lsi/                #   二级索引、投影类型、GSI vs scan 成本
│   ├── 09-pipeline-metadata-demo/     #   综合示例：Pipeline Metadata 表 + GSI
│   ├── 10-single-table-one-to-many/   #   单表设计：一对多（Customer → Card → Tx）
│   ├── 11-single-table-many-to-many/  #   单表设计：多对多三种方案对比
│   ├── README.md                      #   12 个模块的索引 + 推荐学习顺序
│   └── cleanup_all_tables.py          #   一键清理本项目在 AWS 上创建的所有表
├── dynamodb_basic_opeartions/         # 极薄的 runtime 包：暴露一个 one.bsm 单例
│   └── one/                           #   BotoSesManager 的 cached_property 封装
├── docs/learn-this-project/           # 7 个分析文档（6 个 skill 共享的知识库）
├── .claude/skills/learn-this-project-*/   # 6 个交互式学习 skill
├── mise.toml / pyproject.toml         # 工具链 + 依赖（mise 管 Python+uv+claude，uv 管包）
└── .env.example                       # 只需要一个环境变量：AWS_PROFILE
```

00–08 各模块**独立**，可任意顺序学习；09–11 是**序列化**示例，需按 `s01 → s02 → ...` 顺序运行。

---

## 技术栈 + 启动

| 组件 | 版本 | 作用 |
|---|---|---|
| Python | 3.12（`mise.toml` 锁定） | 运行时 |
| uv | latest | 包管理（取代 pip + virtualenv） |
| pynamodb | ≥6.1.0 | DynamoDB ORM |
| boto3 + boto-session-manager | ≥1.42.95 | AWS SDK + profile/region/cached client 封装 |
| pynamodb-session-manager | ≥0.1.2 | `use_boto_session(Model, bsm)` 上下文管理器 |

启动只需要 4 步：

```bash
mise install                # 装 Python 3.12 / uv / claude / pandoc
cp .env.example .env        # 编辑后填入 AWS_PROFILE=<你的 profile>
mise run inst               # 自动建 venv + uv sync --all-extras
python examples/00-minimal-poc/s01_minimal_poc.py   # smoke test
```

第 4 步首次运行会卡 5–30 秒——这是 `create_table(wait=True)` 在等 DynamoDB 把表激活，正常。之后再跑同一个脚本只需 ~2 秒。AWS IAM 需要在 `us-east-1` 对 `dynamodb_basic_opeartions_*` 表有读写权限。

---

## 学完之后

学完之后跑一下清理：

```bash
python examples/cleanup_all_tables.py
```

会列出所有 `dynamodb_basic_opeartions_` 开头的表，确认后一次性删掉。

然后 —— 跑 `/learn-this-project-publish` 把这份仓库变成 portfolio。

---

## 发布成你自己的项目

教学仓库永远是教学仓库——直接发到 GitHub 上面试官一眼就能看出"这是跟着教程做的"。`/learn-this-project-publish` 这个 skill 把转换全流程自动化：删除教学痕迹、生成一份依赖顺序的 commit 备忘单、用你自己的话共写一份新的 README、最后跑一轮 hostile-scan audit 确认没有残留。

- skill 操作本地文件，**不会替你 `git commit`** —— 提交动作还是你自己来，从生成的 `tmp/publish-commit-plan.md` 里复制粘贴。
- README 共写是 D-mode：它问你 2–3 个问题，听你的回答，按你的语气写一段，你 review & 编辑——不是它替你 invent 内容。
- 最终 Audit 必须 0 个 🔴 HIGH RISK 才算"可以发布"。

**cardinal rule**：发布后的仓库**不能被识别出是教学项目**——这是发布是否成功的唯一硬指标，整个 publish 流程都围绕这一条展开。

---

## 掌握的标志

学完之后，你应该能：

- 用 pynamodb 定义 Model、做 DynamoDB 读写——并能向同事解释**为什么** `save()` 是 PutItem 而 `update()` 是 UpdateItem，以及搞混了会发生什么。
- 在 query 和 scan 之间凭直觉就能选对，并能用真实的 ConsumedCapacity 数字（参见 `examples/09/s04_compare_capacity.py`）说服别人为什么要加 GSI。
- 用条件写入和乐观锁处理并发；知道什么时候必须升级到 `TransactWrite`，并能算出 2× 的代价是否值得。
- 在白板上画出一个 Single-Table Design：**先列访问模式、再设计 PK/SK**，能解释 `TX#<card_id>#<ts>` 这种 composite SK 顺序为什么不能换。这是从 SQL 思维切到 DynamoDB 思维的根本一步。
