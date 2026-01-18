---
name: social-media-pro
description: 社交媒体专业运营技能 - 完整流程：热点抓取 → 搜索提炼 → HAP 读取人设 → Skill 生成内容 → 生成配图 → 自动发布 → 数据回流到 HAP。支持小红书、抖音、微信公众号、视频号等多平台。提供一键式 HAP 应用搭建和 Playwright MCP 自动配置。
version: 2.1.0
---

# 社交媒体专业运营技能

## 一句话说明

**热点抓取 → 搜索提炼 → HAP 读取人设 → Skill 生成内容 → 生成配图 → 自动发布 → 数据回流到 HAP。**

## 触发条件

当用户提到以下内容时自动触发：
- "社交媒体运营"
- "小红书/抖音/公众号/视频号运营"
- "生成小红书笔记"
- "发布到小红书/抖音/公众号"
- "每日热点推荐"
- "热点内容生成"
- "批量生成内容"
- "搭建社交媒体运营系统"
- "配置 HAP 应用"
- "配置 Playwright MCP"

## 核心能力

本 Skill 提供一站式社交媒体运营解决方案，涵盖从选题到发布的完整流程。

### 🚀 一键式自动化搭建

**用户只需提供一个空 HAP 应用的 MCP，AI 会自动：**
1. ✅ 创建所有必需的工作表（含字段和别名）
2. ✅ 配置 API 服务记录（搜索、图片生成、热点新闻）
3. ✅ 添加初始话题标签数据
4. ✅ 添加热点平台关注配置
5. ✅ 配置 Playwright MCP（自动安装和设置）
6. ✅ 验证系统配置完整性

**用户只需要：**
- 提供 HAP 空应用的 MCP 配置（Appkey + Sign）
- 提供 API Key（Tavily、图片生成等）
- 确认部署步骤

**AI 会自动完成所有配置工作！**

### 技术架构

```
┌─────────────────────────────────────────────────────┐
│                   用户输入主题                        │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 1: 获取选题 (Hot News API)                     │
│  - 输入: 用户主题 / 自动抓取热点                      │
│  - 输出: 明确选题（标题方向）                         │
│  - 组件: Hot News (https://github.com/orz-ai/hot_news)│
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 2: 搜索资料 (Tavily Search API)                │
│  - 输入: 选题关键词                                   │
│  - 输出: 结构化要点（观点/方法/清单）                  │
│  - 组件: Tavily Search (https://www.tavily.com/)    │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 3: 从 HAP 拉取账号配置 (HAP MCP)               │
│  - 输入: 账号ID / 账号名称                            │
│  - 输出: 人设、风格、规则                             │
│  - 组件: HAP MCP (明道云)                            │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 4: 生成内容 (AI + 人设规则)                    │
│  - 输入: 要点 + 人设风格 + 平台规则                   │
│  - 输出: 标题多版本 + 正文 + 标签 + 配图建议           │
│  - 组件: 生成内容 Skill                              │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 5: 生成配图 (TUZI / 即梦 API)                  │
│  - 输入: 配图建议 / 文案关键句                        │
│  - 输出: 封面图 + 配图                                │
│  - 组件: TUZI API / 火山引擎即梦 API                  │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 6: 自动发布 (Playwright MCP)                   │
│  - 输入: 标题 + 正文 + 标签 + 配图                    │
│  - 输出: 发布成功链接 / 发布状态                      │
│  - 组件: Playwright MCP                              │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│  Step 7: 数据回流到 HAP (HAP MCP)                    │
│  - 输入: 发布内容 + 链接 + 表现数据                   │
│  - 输出: HAP 内可复用的选题/内容/数据记录             │
│  - 组件: HAP MCP                                     │
└─────────────────────────────────────────────────────┘
```

## 工作流程

### 主工作流：内容生成与发布

```
用户输入主题
    ↓
Step 1: 确认 MCP 连接
    ↓
Step 2: 读取搜索 API 配置（Tavily）
    ↓
Step 3: 调用 Tavily API 搜索资料
    ↓
Step 4: 从 HAP "账号人设" 表读取配置（强制要求）
    - 账号定位 (positioning)
    - 目标受众 (target_audience)
    - 内容方向 (content_direction)
    - 内容风格 (content_style) **【必须严格遵循】**
    - 内容原则 (content_principles) **【必须严格遵守】**
    - 图片生成提示词 (image_generation_prompt)
    ↓
Step 5: 根据搜索结果 + 账号人设撰写内容
    - 严格遵循 content_style 和 content_principles
    - 结合搜索资料生成符合定位的内容
    ↓
Step 6: 生成封面标题（5-10个备选）
    ↓
Step 7: 选择话题标签（从 HAP 话题库）
    ↓
Step 8: 生成配图建议
    ↓
Step 9: 保存到本地预览
    ↓
Step 10: 与用户确认内容、标题、图片
    ↓
Step 11: 读取图片生成 API 配置（TUZI/即梦）
    ↓
Step 12: 生成图片（用户确认后）
    - 封面图（含标题文字）
    - 配图（对比图/流程图/场景图）
    ↓
Step 13: 与用户确认是否发布
    ↓
Step 14: 与用户确认是否同步到 HAP
    ↓
Step 15: 同步到 HAP "内容管理" 表
    - 小红书：只上传图片 URL
    - 其他平台：根据需求上传图片和文档
    ↓
Step 16: 使用 Playwright MCP 发布到目标平台
```

### 热点推荐工作流

```
用户请求热点推荐
    ↓
Step 1: 从 HAP "热点平台关注表" 读取关注平台
    ↓
Step 2: 读取热点新闻 API 配置
    ↓
Step 3: 调用 Hot News API 获取各平台热点
    ↓
Step 4: 从 HAP "账号人设" 表读取配置
    ↓
Step 5: 基于热点 + 人设分析选题
    - 筛选相关热点
    - 生成 5-10 个选题建议
    - 评分排序（相关度 1-10 分）
    ↓
Step 6: Markdown 表格展示选题建议
    ↓
Step 6.5: 询问用户是否加入选题库
    ↓
Step 7: 用户选择选题，深度搜索资料
    ↓
Step 8: 基于热点信息 + 搜索资料生成内容
    ↓
Step 9: 后续流程同主工作流（Step 6-16）
```

## 平台支持

### 1. 小红书

**内容格式：**
- 标题：10字以内，疑问式/数字式/痛点式/反差式
- 正文：300-400字，故事化开头，结构清晰
- 话题标签：5-8个，从 HAP 话题库选取
- 图片：封面图 + 2-3张配图（768x1024，3:4竖屏）

**发布流程：**
1. 导航到 `https://creator.xiaohongshu.com/publish/publish?type=image_text`
2. 上传图片（使用 `setInputFiles`）
3. 填写标题
4. 填写正文（包含话题标签）
5. 点击发布

### 2. 抖音

**内容格式：**
- 视频时长：15-60秒
- 文案：不超过50字
- 话题标签：3-5个

### 3. 微信公众号

**内容格式：**
- 标题：不超过64字
- 正文：2000-5000字（HTML格式）
- 摘要：文章摘要
- 封面：封面图片

### 4. 视频号

**内容格式：**
- 视频时长：30秒-30分钟
- 文案：不超过1000字
- 话题：3-5个

## HAP 工作表结构

本 Skill 依赖以下 HAP 工作表（**用户需要在 HAP 应用中创建**）：

### 1. 内容管理表

**别名：** `content_management`

**字段：**
- `platform_type`: 平台类型（SingleSelect: 小红书/抖音/微信公众号/视频号）
- `content_title`: 内容标题（Text，标题字段）
- `cover_title`: 封面标题（Text）
- `content`: 正文内容（Text）
- `tags`: 话题标签（Text）
- `cover_image`: 封面图（Attachment）
- `other_images`: 其他配图（Attachment）
- `markdown_doc`: Markdown 文档（Attachment，小红书不使用）
- `related_topic`: 关联选题（Relation，关联选题库表）
- `publish_status`: 发布状态（SingleSelect: 草稿/待发布/已发布/已取消）
- `publish_time`: 发布时间（DateTime）
- `note_url`: 内容链接（Text）
- `views`: 浏览量（Number）
- `likes`: 点赞数（Number）
- `collections`: 收藏数（Number）
- `comments`: 评论数（Number）

### 2. 账号人设表

**别名：** `account_persona`

**字段：**
- `platform_name`: 平台名称（Text，标题字段）
- `platform_type`: 平台类型（SingleSelect）
- `positioning`: 账号定位（Text，**必需**）
- `target_audience`: 目标受众（Text，**必需**）
- `content_direction`: 内容方向（Text，**必需**）
- `content_style`: 内容风格（Text，**必需**）
- `content_principles`: 内容原则（Text，**必需**）
- `image_generation_prompt`: 图片生成提示词（Text）

### 3. API服务配置表

**别名：** `api_service_config`

**字段：**
- `function_name`: 功能名称（Text，标题字段）
- `provider`: 服务提供商（Text）
- `service_type`: 服务类型（SingleSelect: 搜索API/图片生成/热点新闻）
- `purpose`: 用途说明（Text）
- `is_default`: 是否默认（Checkbox）
- `endpoint_url`: Endpoint URL（Text，**必需**）
- `api_key`: API Key（Text）
- `access_key_id`: Access Key ID（Text）
- `secret_access_key`: Secret Access Key（Text）
- `region`: Region（Text）
- `service`: Service（Text）
- `api_version`: API版本（Text）
- `sdk_name`: SDK名称（Text）
- `config_status`: 配置状态（SingleSelect: 启用/禁用）

### 4. 话题标签库表

**别名：** `topic_library`

**字段：**
- `topic_name`: 话题名称（Text）
- `topic_type`: 话题类型（SingleSelect）
- `heat_level`: 热度等级（SingleSelect: 高热度/中热度/低热度）
- `description`: 说明（Text）
- `last_updated`: 最后更新（DateTime）

### 5. 选题库表

**别名：** `topic_selection`

**字段：**
- `topic_title`: 选题标题（Text，标题字段）
- `source_platform`: 来源平台（Relation，关联热点平台关注表）
- `topic_angle`: 选题角度（Text）
- `recommendation_reason`: 推荐理由（Text）
- `relevance_score`: 相关度评分（Number，1-10分）
- `source_url`: 原文链接（Text）
- `content_preview`: 内容预览（Text）
- `topic_status`: 选题状态（SingleSelect: 待创作/创作中/已创作/已取消）
- `related_content`: 关联内容（Relation，关联内容管理表，多选）
- `notes`: 备注（Text）

### 6. 热点平台关注表

**别名：** `hot_platform_follow`

**字段：**
- `platform_name`: 平台名称（Text，标题字段）
- `platform_code`: 平台代码（Text，如：baidu、weibo、zhihu）
- `platform_type`: 平台类型（SingleSelect: 综合新闻/科技/社交媒体）
- `is_followed`: 是否关注（Checkbox）
- `priority`: 优先级（Number，1-10）
- `description`: 平台说明（Text）
- `last_fetch_time`: 最后获取时间（DateTime）

## API 配置说明

### 1. Tavily 搜索 API

**配置示例（HAP "API服务配置" 表）：**
- **功能名称**：Tavily 搜索API
- **服务提供商**：Tavily
- **服务类型**：搜索API
- **API Key**：tvly-dev-xxx...（**必需填写**）
- **Endpoint URL**：https://api.tavily.com（**必需填写**）
- **用途说明**：用于搜索主题相关资料
- **配置状态**：启用

### 2. TUZI 图片生成 API（OpenAI 兼容）

**配置示例：**
- **功能名称**：TUZI 图片生成
- **服务提供商**：TUZI
- **服务类型**：图片生成
- **API Key**：sk-xxx...（**必需填写**）
- **Endpoint URL**：https://api.tu-zi.com/v1（**必需填写**）
- **是否默认**：是（勾选）
- **配置状态**：启用

### 3. 火山引擎即梦 API

**配置示例：**
- **功能名称**：即梦图片生成
- **服务提供商**：火山引擎
- **服务类型**：图片生成
- **Access Key ID**：（用户填写）
- **Secret Access Key**：（用户填写）
- **Region**：cn-beijing
- **Service**：cv
- **API版本**：jimeng_t2i_v31
- **SDK名称**：volcenginesdkcv20240606
- **配置状态**：启用

### 4. Hot News API

**配置示例：**
- **功能名称**：热点新闻API
- **服务提供商**：orz.ai
- **服务类型**：热点新闻
- **Endpoint URL**：https://orz.ai/api/v1/dailynews（**必需填写**）
- **用途说明**：用于获取各平台热点新闻
- **配置状态**：启用

## 使用示例

### 示例 1: 生成小红书笔记

```
用户：帮我生成一篇关于 "MCP 是什么" 的小红书笔记

执行流程：
1. 确认当前连接的 HAP MCP 应用
2. 从 HAP "API服务配置" 表读取 Tavily API 配置
3. 调用 Tavily API 搜索 "MCP 是什么" 相关资料
4. 从 HAP "账号人设" 表读取小红书账号人设配置
5. 根据搜索结果 + 账号人设撰写内容（300-400字）
6. 生成 5-10 个封面标题备选
7. 从 HAP "话题库" 表选择 5-8 个话题标签
8. 生成配图建议（封面图 + 2-3张配图）
9. 保存到本地文件预览
10. 用户确认后，读取图片生成 API 配置
11. 生成图片（封面图含标题，其他配图与内容强相关）
12. 用户确认后，同步到 HAP "内容管理" 表
13. 用户确认后，使用 Playwright MCP 发布到小红书
14. 更新 HAP 发布状态和笔记链接
```

### 示例 2: 每日热点推荐

```
用户：推荐今日热点内容

执行流程：
1. 确认当前连接的 HAP MCP 应用
2. 从 HAP "热点平台关注表" 读取关注的平台（is_followed = 1）
3. 从 HAP "API服务配置" 表读取热点新闻 API 配置
4. 调用 Hot News API 获取各平台热点
5. 从 HAP "账号人设" 表读取小红书账号人设配置
6. 基于热点 + 账号人设分析选题，生成 5-10 个选题建议
7. 以 Markdown 表格形式展示选题建议（按相关度排序）
8. 询问用户是否将选题加入选题库
9. 用户选择一个选题后，深度搜索相关资料
10. 基于热点信息 + 搜索资料生成内容
11. 后续流程同示例 1（生成标题、标签、配图、发布）
```

### 示例 3: 批量生成内容

```
用户：帮我生成一周的小红书笔记（7篇）

执行流程：
1. 确认当前连接的 HAP MCP 应用
2. 为每个主题重复完整生成流程
3. 批量保存到 HAP "内容管理" 表（状态：草稿）
4. 用户确认后批量发布
```

## 技术要点

### 1. MCP 配置管理

- **不硬编码 MCP 配置**：所有配置由用户提供或指定
- **动态确认 MCP 连接**：执行操作前先调用 `get_app_info()` 确认
- **动态获取工作表**：通过 `get_app_worksheets_list()` 获取工作表列表
- **优先使用别名匹配**：优先通过 `alias` 匹配工作表，如果不存在则使用 `name` 匹配
- **绝对不使用硬编码 ID**：所有查询都必须先匹配工作表/字段，然后再使用 ID

### 2. 账号人设读取（强制要求）

**检查清单：**
1. ✅ 确认 MCP 连接
2. ✅ 动态获取 "账号人设" 表
3. ✅ 查询对应平台的账号人设记录
4. ✅ 验证必需字段（content_style、content_principles）
5. ✅ 禁止使用本地文件（AGENTS.md）
6. ✅ 严格遵循人设生成内容

### 3. 图片生成

**流程：**
1. 从 HAP "账号人设" 表读取 `image_generation_prompt`
2. 从 HAP "API服务配置" 表读取图片生成 API 配置
3. 选择默认配置（`is_default = 1`）或第一个启用的配置
4. 结合人设图片风格 + 配图建议生成完整提示词
5. 调用 API 生成图片（支持并行生成）
6. 处理 API 响应（优先 URL，支持 base64）
7. 自动下载图片到本地
8. 上传 URL 到 HAP

**图片格式（小红书）：**
- 尺寸：768x1024（3:4竖屏）
- 风格：手绘风格插画（清新活泼）
- 封面图：必须包含封面标题文字

### 4. 内容生成规范

**要求：**
- 必须先读取账号人设，再生成内容
- 严格遵循 `content_style`（内容风格）
- 严格遵守 `content_principles`（内容原则）
- 符合 `positioning`（账号定位）
- 适合 `target_audience`（目标受众）
- 符合 `content_direction`（内容方向）

**小红书笔记（300-400字）：**
1. 开场吸引：故事化开头，带情感色彩
2. 技术科普：用简单语言解释复杂概念
3. 产品视角：分析技术对行业和用户的影响
4. 总结升华：行动建议/思考启发

### 5. 数据同步

**流程：**
1. 生成内容 → 保存到本地预览
2. 用户确认 → 生成图片
3. 用户确认 → 同步到 HAP
   - **小红书**：只上传图片 URL
   - **其他平台**：根据需求上传图片和文档
4. 用户确认 → 使用 Playwright MCP 发布
5. 发布成功 → 更新 HAP 发布状态和链接

## 最佳实践

### 1. MCP 使用

- 永远不硬编码 MCP 配置
- 执行操作前先确认 MCP 连接
- 动态获取工作表和字段结构
- 使用别名或名称匹配，不使用硬编码 ID

### 2. 配置管理

- 所有密钥和配置存储在 HAP
- 使用时动态读取，不假设默认值
- 用户需要在 HAP 中填写所有必要配置

### 3. 内容运营

- 确保内容有价值，符合账号定位
- 保持稳定的更新频率（每周3-5篇）
- 定期查看 HAP 数据，分析爆款规律
- 及时回复评论，积极与用户互动

### 4. 多平台适配

- 根据不同平台特点调整内容格式和风格
- 小红书：短而精（300-400字），图片精美
- 微信公众号：长而深（2000-5000字），HTML格式
- 抖音：短视频（15-60秒），文案简洁

## 注意事项

### 1. 账号人设

- **必须从 HAP "账号人设" 表读取**
- **禁止使用本地文件（AGENTS.md）**
- **生成内容前必须验证人设字段完整性**
- **严格遵循 content_style 和 content_principles**

### 2. 图片生成

- 图片生成可能失败（SSL 错误），需要重试
- 封面图必须包含封面标题文字
- 图片内容必须与正文内容强相关
- 图片格式必须符合平台要求

### 3. 发布流程

- 发布前需要用户先登录目标平台
- 不同平台的字数要求不同，需要适配
- 话题标签必须从 HAP 话题库中选取

### 4. 数据管理

- 小红书笔记不同步 Markdown 文档到 HAP
- 只上传图片 URL，不上传 base64 数据
- 关联选题时需要更新选题状态

## 依赖工具

1. **Tavily API**（配置存储在 HAP）: 搜索相关资料
2. **Hot News API**（配置存储在 HAP）: 获取热点新闻
3. **HAP MCP**（用户提供）: 数据管理、配置管理
4. **Playwright MCP**: 自动化发布到各平台
5. **TUZI API / 火山引擎即梦 API**（配置存储在 HAP）: 图片生成

## 文件结构

```
social-media-operation/
├── 内容/                        # 内容根目录
│   ├── 小红书/                  # 平台分类
│   │   └── {笔记标题}/          # 笔记目录（使用封面标题命名）
│   │       ├── {笔记标题}.md    # Markdown 文档
│   │       └── 配图/            # 配图目录
│   │           ├── 封面图.png
│   │           ├── 对比图.png
│   │           └── 流程图.png
│   ├── 抖音/
│   ├── 微信公众号/
│   └── 视频号/
├── 话题库/                      # 话题标签库（可选）
└── README.md                    # 项目说明
```

## 参考资源

- [Hot News API](https://github.com/orz-ai/hot_news)
- [Tavily Search API](https://www.tavily.com/)
- [Playwright MCP](https://github.com/microsoft/playwright-mcp)
- [明道云 HAP](https://www.mingdao.com/)
- [小红书创作者中心](https://creator.xiaohongshu.com/)

## 版本历史

- **v2.0.0**: 完整流程整合，支持热点推荐、选题库、多平台发布
- **v1.0.0**: 基础版本，支持小红书笔记生成和发布

---

**开发者**: 基于 social-media-operation 项目实践总结
**最后更新**: 2026-01-18
