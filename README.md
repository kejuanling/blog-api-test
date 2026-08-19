# blog-api-test

博客文章 API 自动化测试项目，使用 Postman Collection 覆盖文章资源的完整业务流（创建 → 查询 → 更新 → 删除），并通过 GitHub Actions 在 CI 中自动执行。

## 测试内容

覆盖「资源管理业务流」的完整生命周期：

| 步骤 | 接口 | 说明 |
|------|------|------|
| 01 | `POST /article/create` | 创建帖子，从响应中提取最新文章 ID |
| 02 | `GET /article/view/{id}` | 查询刚创建的帖子 |
| 03 | `POST /article/update` | 更新帖子内容 |
| 03.5 | `GET /article/view/{id}` | 验证更新结果 |
| 04 | `GET /` | 验证更新成功（列表页包含新标题） |
| 05 | `GET /article/delete/{id}` | 删除帖子 |
| 06 | `GET /` | 验证删除成功 |

## 文件结构

```
blog-api-test/
├── blog_test_collection.json   # Postman Collection（含断言脚本）
├── .github/
│   └── workflows/
│       └── test.yml            # GitHub Actions CI 配置
└── README.md
```

## 使用方式

### 本地运行（Postman）

1. 导入 `blog_test_collection.json` 到 Postman
2. 修改 collection 中请求的 Base URL（默认指向 ngrok 代理地址）
3. 依次运行 collection 中的请求，断言脚本会自动校验每一步结果

### CI 运行（GitHub Actions）

推送代码后，`.github/workflows/test.yml` 会自动执行测试。需要配置环境变量（如 API 地址）到仓库 Secrets。

## 技术栈

- Postman / Newman（API 测试）
- GitHub Actions（CI 自动化）
- JavaScript（断言脚本，Postman 内置 pm 对象）
