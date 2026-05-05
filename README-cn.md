# DynamoDB + pynamodb 入门教程

## 这门课学什么？

**Amazon DynamoDB** 是 AWS 上最重要的 NoSQL 数据库服务，专为高并发、低延迟、无限水平扩展而设计。**pynamodb** 是目前最优雅的 Python ORM，让你用 Python 类来操作 DynamoDB 表，代码量只有原生 boto3 的三分之一。

这是**云计算时代后端开发和数据工程的基础技能**。无论你做 Web 后端、数据管道还是 Serverless 应用，DynamoDB 都是绑定 AWS 生态后绕不开的核心服务。

本教程面向入门人群，通过一系列小而完整的 POC 脚本，带你从零掌握 DynamoDB 的核心操作和数据建模思路。

### 学完这门课你会掌握

- **pynamodb 基本骨架** — 用 Model + Attribute + Meta 三件套定义表、读写数据
- **单条 CRUD 操作** — save / get / update / delete / refresh，理解 PutItem vs UpdateItem 的区别
- **批量与查询** — batch_write / batch_get / query / scan，理解 query 和 scan 的性能差异
- **条件写入与乐观锁** — 服务端前置条件、版本号冲突检测，保证并发安全
- **事务（TransactWrite / TransactGet）** — 跨行原子操作，理解适用场景和 2x 成本
- **二级索引（GSI / LSI）** — 为非主键查询场景建索引，理解投影类型和一致性差异
- **单表设计（Single-Table Design）** — 一对多、多对多关系在同一张表里建模，理解 access pattern 驱动的 schema 设计


## 前置课程

我们默认你已经完成以下三门前置课程：

1. **AWS 基础** — 你已经知道如何创建 AWS 账号、配置 AWS CLI credential（`aws configure`）
2. **Claude Code 系列教程** — 你已经知道如何使用 Claude Code 的 slash command（`/` 命令）来与 AI 协作学习
3. **mise-en-place** — 你已经知道这是在电脑上最快准备好开发环境的工具

如果以上任何一项你还不熟悉，请先去完成对应的前置课程再回来。


## 环境准备

### 第一步：安装依赖

只需两条命令：

```bash
mise install
mise run inst
```

如果跑不起来，进入 Claude Code，输入 `/learn-this-project` 命令，让 AI 帮助你在 MacBook 或 GitHub Codespace 上把环境跑起来。

### 第二步：配置 AWS

因为我们直接操作 AWS 上的 DynamoDB 表（不是本地模拟），你需要配置 AWS profile：

1. 复制 `.env.example` 为 `.env`：
   ```bash
   cp .env.example .env
   ```
2. 编辑 `.env`，填入你自己的 AWS profile 名称：
   ```
   AWS_PROFILE="your-profile-name"
   ```

这个 profile 需要有 DynamoDB 的读写权限（`us-east-1` region）。

准备工作到此完成。


## 如何学习

### 课程内容结构

`examples/` 目录下有 12 个模块，从最简单的 hello-world 到完整的单表设计，循序渐进。**每个子文件夹下都有一个 `README.md`**，是该模块的小教程：

```
examples/
├── 00-minimal-poc/                    # 最小可运行示例（Model + Attribute + Meta 骨架）
├── 01-attributes/                     # 属性类型：标量、集合、JSON
├── 02-table-management/               # 建表 / 描述表 / 计费模式
├── 03-crud-basic/                     # save / get / update / delete / refresh
├── 04-batch-operations/               # batch_write / batch_get / UnprocessedItems
├── 05-query-and-scan/                 # query vs scan、分页、排序 + limit
├── 06-condition-expression/           # 条件写入、乐观锁
├── 07-transactions/                   # TransactWrite / TransactGet 跨行原子操作
├── 08-gsi-and-lsi/                    # 二级索引、投影类型、GSI vs scan 成本对比
├── 09-pipeline-metadata-demo/         # 综合演示：复刻 AxiomCard Pipeline Metadata 表
├── 10-single-table-one-to-many/       # 单表设计：一对多（Customer → Card → Transaction）
└── 11-single-table-many-to-many/      # 单表设计：多对多三种方案
```

其中 00-08 各模块相互独立，可以任意跳着学；09-11 是顺序演示，需要按 s01 → s02 → ... 顺序运行。

你需要一个一个地用 `python` 运行这些脚本。比如：

```bash
python examples/00-minimal-poc/s01_minimal_poc.py
```

### 用 AI 辅助学习

**核心命令：** 进入 Claude Code，输入：

```
/learn-this-project
```

这个命令会加载本教程的所有信息，AI 将清楚地知道如何引导你学习。它有两种模式：

- **Guided Tour（引导学习）** — AI 会从头到尾带你走一遍项目，逐步讲解每个脚本
- **Quiz（练习测验）** — AI 会针对你学过的内容进行概念提问（50 道题，覆盖 10 个知识领域），帮你巩固理解

### 推荐学习方式

我建议你**同时打开两个窗口**：

1. **窗口 1** — 学习模式：打 `/learn-this-project`，跟着 AI 学
2. **窗口 2** — 练习模式：打 `/learn-this-project`，选 Quiz，一边学一边练

学习过程中，请务必：

- **自己运行代码** — 不要只看不跑，亲手执行每个脚本
- **去 AWS Console 看** — 登录 AWS Console → DynamoDB，看看运行脚本后表里到底有什么数据、索引长什么样
- **看到不懂的东西，截图给 AI 提问** — 这就是你学会一个东西的过程

### 如何检验自己学懂了

学习就是一个**学 → 检验 → 再学**不断提升的过程。检验标准：

1. **代码都能跑** — 每个脚本你都亲手运行过，理解输出结果
2. **Quiz 80% 以上能答** — 如果面试中问你这些问题，你能直接用自然语言描述清楚（不需要背代码，但要能讲明白原理和为什么这样做）

如果 Quiz 中有答不上来的，回到对应的脚本重新学习，搞懂了再继续。

### 学习节奏

这个虽然只有一节课，但内容很多（12 个模块、50 道 Quiz），你可以慢慢做，不要急。

多多提问题。遇到不懂的：

- **事实性问题**（"query 和 scan 有什么区别？""GSI 的投影类型有几种？"）→ 直接问 AI，AI 回答了就行
- **开放性问题**（"什么时候该用单表设计？""DynamoDB 和 Postgres 怎么选？""这个 access pattern 合理吗？"）→ 记下来问导师

学会鉴别这两类问题，是一项重要的职业能力。

### 复习

学完之后，你可以随时再用 `/learn-this-project` 的 Quiz 模式来复习巩固。


## 清理资源

所有示例脚本会在你的 AWS 账号里创建以 `dynamodb_basic_opeartions_` 开头的表。学完之后，运行：

```bash
python examples/cleanup_all_tables.py
```

输入 `yes` 确认，即可删除所有教程创建的表（不会影响你账号里的其他资源）。


## 展示你的学习成果

🚨 **重要：请认真阅读本节，这是你的 actionable item！** 🚨

学完之后，我们希望你把学习过程展示在 GitHub 上，让别人知道你是一个循序渐进、持续学习的人。

### 创建你自己的仓库

本教程代码库是 private 的。你需要：

1. 把本项目下载到本地
2. 创建一个**新的 public 仓库**

仓库命名建议：不要照搬 `learn_dynamodb_basic_opeartions`。加上你自己的名字，去掉 `learn`，加上 `POC`。例如：

```
firstname-lastname-dynamodb-basic-operations-poc
```

当然你也可以自己随便起一个个性化的名字，不要跟大家撞车。

### 分步提交（非常重要）

🚨 **不要把所有文件一股脑一个 commit 全部塞进去！** 🚨

你应该把它作为一个**渐进式学习过程**来展示。建议：

1. 先提交根目录下的配置文件（`mise.toml`、`pyproject.toml` 等）
2. 再提交 `.claude/` 目录
3. 再提交源代码 `dynamodb_basic_opeartions/`
4. `examples/` 里面的子文件夹和文件，**一个一个地提交**

总共至少要有 **15-20 个 commit**，展现出你不断学习、不断进步的过程。

### 🚨 必须删除的文件 🚨

在你的新仓库中，**请务必删除以下文件**：

- `README.md` — 删掉
- `README-cn.md` — 删掉
- `TICKET.md`（如果有）— 删掉

这些是教程文件，不应出现在你的展示仓库中。

**只保留 `README.rst`** — 这才是一个正常的项目 README，看起来像是你自己探索学习后写的，而不是跟着教程抄的。你可以看看 `README.rst` 的内容，那就是一个很合格的项目说明。

### 最终效果

你的 public 仓库应该看起来像：
- 一个有十几到二十个 commit 的渐进式学习记录
- 只有 `README.rst` 作为项目说明
- 没有任何教程痕迹（没有 README.md、README-cn.md、TICKET.md）
- 向别人展现你是一个认真学习、持续进步的人
