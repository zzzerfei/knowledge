# knowledge-hub / server

知识库中台后端，基于 NestJS 11 + TypeORM + PostgreSQL（pgvector）。

## 主要能力

- 用户、角色、权限、团队与四层鉴权（JWT → 角色 → 权限 → 资源归属）
- 文档生命周期：草稿 → 提交审核 → 发布 / 驳回 / 归档 / 下架
- 发布后异步管线：分块 → 向量索引（Elasticsearch）→ 知识图谱（Neo4j）
- 混合检索：关键词 + 向量 → RRF 融合 → rerank
- Agentic RAG 问答：意图识别、知识库 / 图谱检索、结果评估与改写、联网兜底、短期与长期记忆
- 外部依赖（ES、Neo4j、RustFS、Langfuse）不可用时降级，不阻塞主流程

## 技术栈

NestJS、TypeORM、PostgreSQL/pgvector、MongoDB、Redis、RabbitMQ、Elasticsearch 8、Neo4j、RustFS（S3 兼容）、Langfuse、OpenTelemetry。

前端位于同级 `client` 目录（Vite + React + Ant Design）。

## 本地启动

```bash
cp .env.example .env      # 按需填写模型 Key 等配置
docker compose up -d      # 基础设施：Postgres / MongoDB / Redis / RabbitMQ / ES / Neo4j / RustFS
pnpm install
pnpm start:dev            # http://localhost:3000
```

默认账号见 `init-scripts/postgresql/init.sql`：`admin` / `reviewer` / `user`，密码均为 `123456`。

## 目录

```
src/
  auth/          登录、JWT、鉴权守卫
  user/          用户与账号
  team/          团队与成员
  document/      文档生命周期与可见性
  search/        全文搜索
  pipeline/      分块、向量索引、图谱构建、ES 索引
  ai/            混合检索、Agentic RAG 问答、对话记忆、评测
  graph/         图谱检索
  mq/            RabbitMQ 发布管线
  storage/       RustFS 对象存储
  redis/         Redis 缓存与短期记忆
  observability/ Langfuse / OpenTelemetry
```

## 常用命令

```bash
pnpm build          # 编译
pnpm lint           # ESLint
pnpm test           # 单元测试
pnpm eval:rag       # RAG 评测
```
