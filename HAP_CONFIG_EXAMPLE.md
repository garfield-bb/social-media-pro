# HAP 工作表配置示例

本文档提供完整的 HAP 工作表配置示例,帮助用户快速搭建运营系统。

## 1. 内容管理表

**别名**: `content_management`

**字段配置**:

| 字段名 | 字段类型 | 是否必需 | 说明 |
|--------|---------|---------|------|
| content_title | Text（标题字段） | 是 | 内容标题 |
| platform_type | SingleSelect | 是 | 平台类型：小红书/抖音/微信公众号/视频号 |
| cover_title | Text | 是 | 封面标题 |
| content | Text（多行） | 是 | 正文内容 |
| tags | Text | 否 | 话题标签（空格分隔） |
| cover_image | Attachment | 是 | 封面图 |
| other_images | Attachment | 否 | 其他配图 |
| markdown_doc | Attachment | 否 | Markdown 文档（小红书不使用） |
| related_topic | Relation | 否 | 关联选题（关联选题库表，单选） |
| publish_status | SingleSelect | 是 | 发布状态：草稿/待发布/已发布/已取消 |
| publish_time | DateTime | 否 | 发布时间 |
| note_url | Text | 否 | 内容链接 |
| views | Number | 否 | 浏览量 |
| likes | Number | 否 | 点赞数 |
| collections | Number | 否 | 收藏数 |
| comments | Number | 否 | 评论数 |

## 2. 账号人设表

**别名**: `account_persona`

**字段配置**:

| 字段名 | 字段类型 | 是否必需 | 说明 |
|--------|---------|---------|------|
| platform_name | Text（标题字段） | 是 | 平台名称 |
| platform_type | SingleSelect | 是 | 平台类型：小红书/抖音/微信公众号/视频号 |
| positioning | Text（多行） | 是 | 账号定位 |
| target_audience | Text（多行） | 是 | 目标受众 |
| content_direction | Text（多行） | 是 | 内容方向 |
| content_style | Text（多行） | 是 | 内容风格 |
| content_principles | Text（多行） | 是 | 内容原则 |
| image_generation_prompt | Text（多行） | 否 | 图片生成提示词 |

**示例数据（小红书）**:

```
平台名称: 小红书 - AI 产品经理
平台类型: 小红书
账号定位: IT公司产品经理，分享 AI 工具和效率技巧
目标受众: 产品经理、互联网从业者、AI 爱好者、对新技术感兴趣的职场人士
内容方向: AI 工具科普、效率提升技巧、产品思维、技术趋势解读
内容风格: 专业但不高冷，用通俗易懂的语言解释复杂概念。简单直白，避免专业术语堆砌，必要时用类比和例子说明。
内容原则:
✅ 用简单语言解释复杂技术，让非技术人员也能理解
✅ 结合实际应用场景，展示工具/技术的价值
✅ 分享个人使用心得和体会，增加真实感
❌ 避免过度使用专业术语
❌ 不夸大效果，不虚假宣传
❌ 不涉及敏感话题

图片生成提示词: 手绘风格插画，清新活泼的卡通插画风格，充满正能量和活力。粗黑描边，手绘质感，流畅自然。明亮饱和的色彩（橙色、黄色、蓝色、绿色）。平面风格，简单的渐变和高光。粗体装饰性字体，带笔画和渐变效果。积极向上、充满活力、科技感、温馨可爱。
```

## 3. API服务配置表

**别名**: `api_service_config`

**字段配置**:

| 字段名 | 字段类型 | 是否必需 | 说明 |
|--------|---------|---------|------|
| function_name | Text（标题字段） | 是 | 功能名称 |
| provider | Text | 是 | 服务提供商 |
| service_type | SingleSelect | 是 | 服务类型：搜索API/图片生成/热点新闻/其他 |
| purpose | Text（多行） | 否 | 用途说明 |
| is_default | Checkbox | 否 | 是否默认（图片生成 API 使用） |
| endpoint_url | Text | 是 | Endpoint URL |
| api_key | Text | 否 | API Key |
| access_key_id | Text | 否 | Access Key ID（火山引擎等） |
| secret_access_key | Text | 否 | Secret Access Key（火山引擎等） |
| region | Text | 否 | Region（火山引擎等） |
| service | Text | 否 | Service（火山引擎等） |
| api_version | Text | 否 | API版本 |
| sdk_name | Text | 否 | SDK名称 |
| config_status | SingleSelect | 是 | 配置状态：启用/禁用 |
| notes | Text（多行） | 否 | 备注 |

**示例数据**:

### Tavily 搜索 API

```
功能名称: Tavily 搜索API
服务提供商: Tavily
服务类型: 搜索API
用途说明: 用于搜索主题相关资料，获取最新信息
API Key: tvly-dev-xxx...
Endpoint URL: https://api.tavily.com
配置状态: 启用
```

### TUZI 图片生成 API

```
功能名称: TUZI 图片生成
服务提供商: TUZI
服务类型: 图片生成
用途说明: 用于生成小红书笔记配图（OpenAI 兼容接口）
是否默认: 是（勾选）
API Key: sk-xxx...
Endpoint URL: https://api.tu-zi.com/v1
配置状态: 启用
备注: 支持多个模型：gemini-3-pro-image-preview、dall-e-3、dall-e-2
```

### 热点新闻 API

```
功能名称: 热点新闻API
服务提供商: orz.ai
服务类型: 热点新闻
用途说明: 用于获取各平台的热点新闻数据，支持每日热点内容推荐
Endpoint URL: https://orz.ai/api/v1/dailynews
配置状态: 启用
```

## 4. 话题标签库表

**别名**: `topic_library`

**字段配置**:

| 字段名 | 字段类型 | 是否必需 | 说明 |
|--------|---------|---------|------|
| topic_name | Text（标题字段） | 是 | 话题名称 |
| topic_type | SingleSelect | 是 | 话题类型：AI技术/效率工具/产品设计/其他 |
| heat_level | SingleSelect | 是 | 热度等级：高热度/中热度/低热度/已过时 |
| description | Text（多行） | 否 | 说明 |
| last_updated | DateTime | 否 | 最后更新 |

**示例数据**:

```
话题名称: #AI工具
话题类型: AI技术
热度等级: 高热度
说明: AI 相关工具和应用的讨论

话题名称: #效率提升
话题类型: 效率工具
热度等级: 高热度
说明: 提升工作效率的技巧和方法

话题名称: #产品经理
话题类型: 产品设计
热度等级: 中热度
说明: 产品经理相关的话题
```

## 5. 选题库表

**别名**: `topic_selection`

**字段配置**:

| 字段名 | 字段类型 | 是否必需 | 说明 |
|--------|---------|---------|------|
| topic_title | Text（标题字段） | 是 | 选题标题 |
| source_platform | Relation | 否 | 来源平台（关联热点平台关注表，单选） |
| topic_angle | Text（多行） | 是 | 选题角度 |
| recommendation_reason | Text（多行） | 是 | 推荐理由 |
| relevance_score | Number | 是 | 相关度评分（1-10分） |
| source_url | Text | 否 | 原文链接 |
| content_preview | Text（多行） | 否 | 内容预览 |
| topic_status | SingleSelect | 是 | 选题状态：待创作/创作中/已创作/已取消 |
| related_content | Relation | 否 | 关联内容（关联内容管理表，多选） |
| notes | Text（多行） | 否 | 备注 |

## 6. 热点平台关注表

**别名**: `hot_platform_follow`

**字段配置**:

| 字段名 | 字段类型 | 是否必需 | 说明 |
|--------|---------|---------|------|
| platform_name | Text（标题字段） | 是 | 平台名称 |
| platform_code | Text | 是 | 平台代码（对应 Hot News API） |
| platform_type | SingleSelect | 是 | 平台类型：综合新闻/科技/社交媒体/技术社区 |
| is_followed | Checkbox | 是 | 是否关注 |
| priority | Number | 是 | 优先级（1-10，数字越大优先级越高） |
| description | Text（多行） | 否 | 平台说明 |
| last_fetch_time | DateTime | 否 | 最后获取时间 |

**示例数据（AI/科技相关平台）**:

```
平台名称: 36氪
平台代码: tskr
平台类型: 科技
是否关注: 是（勾选）
优先级: 9
平台说明: 科技创业和商业资讯平台

平台名称: 少数派
平台代码: sspai
平台类型: 科技
是否关注: 是（勾选）
优先级: 8
平台说明: 科技、数码、生活方式内容平台

平台名称: 掘金
平台代码: juejin
平台类型: 技术社区
是否关注: 是（勾选）
优先级: 7
平台说明: 编程和技术文章社区

平台名称: V2EX
平台代码: vtex
平台类型: 技术社区
是否关注: 是（勾选）
优先级: 6
平台说明: 技术、编程、创意讨论社区
```

## 配置检查清单

在开始使用前，请确认：

- [ ] 已创建所有必需的工作表
- [ ] 已设置工作表别名（推荐）
- [ ] 已添加账号人设配置
- [ ] 已添加 Tavily API 配置
- [ ] 已添加图片生成 API 配置（至少一个）
- [ ] 已添加热点新闻 API 配置（如需热点推荐）
- [ ] 已添加话题标签库数据
- [ ] 已添加热点平台关注配置（如需热点推荐）
- [ ] 已配置 HAP MCP 连接

## 配置验证

使用以下命令验证配置是否正确:

```
请帮我验证 HAP 配置是否完整:
1. 检查所有必需的工作表是否存在
2. 检查账号人设表是否有数据
3. 检查 API 服务配置表是否有数据
4. 检查话题库表是否有数据
```

## 故障排查

### 1. 工作表找不到

- 检查工作表是否已创建
- 检查别名是否正确设置
- 尝试使用工作表名称而不是别名

### 2. 账号人设读取失败

- 检查 "账号人设" 表中是否有对应平台的数据
- 检查 `platform_type` 字段值是否正确（小红书/抖音/微信公众号/视频号）
- 检查必需字段是否都有值（content_style、content_principles）

### 3. API 配置读取失败

- 检查 "API服务配置" 表中是否有对应的配置
- 检查 `config_status` 是否为"启用"
- 检查必需字段是否都有值（api_key、endpoint_url）

## 联系我们

如有问题，请在 [GitHub Issues](https://github.com/your-org/social-media-operation/issues) 提交反馈。
