# DynamoDB + pynamodb 入门教程

## 这门课学什么？

**Amazon DynamoDB** 是 AWS 上最重要的 NoSQL 数据库服务，专为高并发、低延迟、无限水平扩展而设计。**pynamodb** 是目前最优雅的 Python ORM，让你用 Python 类来操作 DynamoDB 表，代码量只有原生 boto3 的三分之一。

本教程通过 12 个循序渐进的 POC 脚本，带你从零掌握 DynamoDB 的核心操作和数据建模思路——从最简单的 CRUD 到单表设计的一对多、多对多关系建模。所有示例使用金融科技信用卡领域（AxiomCard），每个脚本都可以直接在真实 AWS 账号上运行。

### 学完你能做什么

- 用 pynamodb 定义 Model、读写 DynamoDB 数据
- 理解 query vs scan 的性能差异，知道什么时候该建 GSI/LSI
- 用条件写入和乐观锁处理并发，用 Transaction 保证跨行原子性
- **用 Single-Table Design 建模一对多和多对多关系** — 这是 DynamoDB 和 SQL 思维的根本分水岭


## 前置课程

我们默认你已经完成以下前置课程：

1. **AWS 基础** — 你已经知道如何创建 AWS 账号、配置 AWS CLI credential
2. **Claude Code 系列教程** — 你已经知道如何使用 `/learn-this-project-*` 系列命令来学习
3. **mise-en-place** — 你已经知道这是在电脑上最快准备好开发环境的工具


## 环境准备

```bash
mise install
mise run inst
```

然后配置 AWS profile：

```bash
cp .env.example .env
# 编辑 .env，填入你的 AWS profile 名称（需要 us-east-1 的 DynamoDB 读写权限）
```


## 课程内容

`examples/` 目录下有 12 个模块，循序渐进：

```
examples/
├── 00-minimal-poc/                    # 最小可运行示例（黄金骨架）
├── 01-attributes/                     # 属性类型：标量、集合、JSON
├── 02-table-management/               # 建表 / 描述表 / 计费模式
├── 03-crud-basic/                     # save / get / update / delete / refresh
├── 04-batch-operations/               # batch_write / batch_get / UnprocessedItems
├── 05-query-and-scan/                 # query vs scan、分页、排序
├── 06-condition-expression/           # 条件写入、乐观锁
├── 07-transactions/                   # TransactWrite / TransactGet
├── 08-gsi-and-lsi/                    # 二级索引、投影类型、GSI vs scan 成本
├── 09-pipeline-metadata-demo/         # 综合演示：Pipeline Metadata 表
├── 10-single-table-one-to-many/       # 单表设计：一对多
└── 11-single-table-many-to-many/      # 单表设计：多对多三种方案
```

00–08 各模块独立，可以任意跳着学；09–11 需要按 s01 → s02 → ... 顺序运行。


## 如何学习

进入 Claude Code，按顺序使用以下命令：

| 顺序 | 命令 | 做什么 |
|---|---|---|
| 1 | `/learn-this-project-absorb` | 从头到尾走一遍项目，理解每个模块的 what 和 why |
| 2 | `/learn-this-project-quiz` | 60 道小题检验你真的学进去了没有 |
| 3 | `/learn-this-project-elevate` | 看看高级工程师会怎么改进这个项目，学习进阶方向 |
| 4 | `/learn-this-project-interview` | 模拟面试，压力测试你对项目的理解深度 |
| 5 | `/learn-this-project-demo` | 学会怎么把这个项目讲给别人听 |

学习过程中，务必自己运行每个脚本，并去 AWS Console 看表里的实际数据。


## 清理资源

学完之后，运行：

```bash
python examples/cleanup_all_tables.py
```

删除所有 `dynamodb_basic_opeartions_*` 开头的表。


## 展示你的学习成果

学完之后，把学习过程展示在 GitHub 上：

1. **创建新的 public 仓库**，命名建议：`firstname-lastname-dynamodb-basic-operations-poc`
2. **分步提交**（至少 15-20 个 commit）：先提交配置文件，再逐个提交 examples 子文件夹
3. **删除教学文件**：删掉 `README.md`、`README-cn.md`、`TICKET.md`、`docs/learn-this-project/`、`.claude/skills/learn-this-project-*/`
4. **只保留 `README.rst`** 作为项目说明
5. **写你自己的 README** — 用自己的话描述你学了什么、怎么用
