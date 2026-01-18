# 技术方案详解

本文档详细说明社交媒体运营自动化系统的 7 个核心技术方案。

## 技术架构总览

```
┌─────────────────────────────────────────────────────────────┐
│                     用户输入主题/热点推荐                      │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┴────────────────┐
        │                                 │
        ▼                                 ▼
┌──────────────────┐            ┌──────────────────┐
│  1. 平台热点 API  │            │ 2. 关键词搜索 API │
│  (Hot News API)  │            │  (Tavily Search) │
└────────┬─────────┘            └────────┬─────────┘
         │                               │
         └───────────────┬───────────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 3. HAP MCP 获取人设 │
              │   (账号人设表)      │
              └─────────┬──────────┘
                        │
                        ▼
              ┌────────────────────┐
              │ 4. AI 生成内容     │
              │ (标题+正文+配图)   │
              └─────────┬──────────┘
                        │
                        ▼
              ┌────────────────────┐
              │ 5. 图片生成 API    │
              │ (TUZI/即梦/Nano)   │
              └─────────┬──────────┘
                        │
                        ▼
              ┌────────────────────┐
              │ 6. Playwright MCP  │
              │   (自动发布)       │
              └─────────┬──────────┘
                        │
                        ▼
              ┌────────────────────┐
              │ 7. HAP MCP 数据回写│
              │   (内容管理表)     │
              └────────────────────┘
```

## 1. 平台热点 API (Hot News API)

### 技术方案

**API 地址**: `https://orz.ai/api/v1/dailynews`

**开源项目**: [hot_news](https://github.com/orz-ai/hot_news)

**功能**: 获取各大平台的热点新闻数据

### 支持的平台

| 平台代码 | 平台名称 | 内容类型 |
|---------|---------|---------|
| `baidu` | 百度热搜 | 社会热点、娱乐、事件 |
| `sspai` | 少数派 | 科技、数码、生活方式 |
| `weibo` | 微博热搜 | 社交媒体热点、娱乐 |
| `zhihu` | 知乎热榜 | 问答、深度内容 |
| `tskr` | 36氪 | 科技创业、商业资讯 |
| `juejin` | 掘金 | 编程、技术文章 |
| `vtex` | V2EX | 技术、编程、创意 |
| `bilibili` | 哔哩哔哩 | 视频、动漫、游戏 |
| `douyin` | 抖音 | 短视频热点 |
| `github` | GitHub Trending | 开源项目、编程语言 |
| `hackernews` | Hacker News | 科技新闻、创业、编程 |

### 使用方式

**1. 在 HAP "热点平台关注表" 中配置关注的平台**

```
平台名称: 36氪
平台代码: tskr
平台类型: 科技
是否关注: 是（勾选）
优先级: 9
```

**2. 在 HAP "API服务配置" 表中配置 Hot News API**

```
功能名称: 热点新闻API
服务提供商: orz.ai
服务类型: 热点新闻
Endpoint URL: https://orz.ai/api/v1/dailynews
配置状态: 启用
```

**3. AI 自动调用流程**

```python
# Step 1: 从 HAP "热点平台关注表" 读取关注的平台
platforms = get_followed_platforms()  # is_followed = 1

# Step 2: 从 HAP "API服务配置" 表读取热点新闻 API 配置
api_config = get_api_config(service_type="热点新闻")
endpoint_url = api_config['endpoint_url']

# Step 3: 对每个关注的平台调用 API
for platform in platforms:
    url = f"{endpoint_url}/?platform={platform['platform_code']}"
    response = requests.get(url)
    hot_news = response.json()['data']

# Step 4: 基于热点 + 账号人设分析选题
analyze_and_recommend_topics(hot_news, account_persona)
```

### API 响应示例

```json
{
  "status": "200",
  "data": [
    {
      "title": "Claude Code 新功能发布",
      "url": "https://example.com/article/123",
      "score": "98",
      "desc": "Claude Code 推出全新的 MCP 集成功能..."
    }
  ],
  "msg": "success"
}
```

---

## 2. 关键词搜索 API (Tavily Search)

### 技术方案

**API 地址**: `https://api.tavily.com`

**官网**: [Tavily](https://www.tavily.com/)

**功能**: 搜索主题相关资料，获取最新信息

### 使用方式

**1. 在 HAP "API服务配置" 表中配置 Tavily API**

```
功能名称: Tavily 搜索API
服务提供商: Tavily
服务类型: 搜索API
API Key: tvly-dev-xxx...
Endpoint URL: https://api.tavily.com
配置状态: 启用
```

**2. AI 自动调用流程**

```python
# Step 1: 从 HAP "API服务配置" 表读取 Tavily API 配置
api_config = get_api_config(service_type="搜索API")
api_key = api_config['api_key']
endpoint_url = api_config['endpoint_url']

# Step 2: 调用 Tavily API 搜索资料
response = requests.post(
    f"{endpoint_url}/search",
    headers={"Content-Type": "application/json"},
    json={
        "api_key": api_key,
        "query": topic,  # 用户输入的主题
        "search_depth": "basic",
        "max_results": 5,
        "topic": "general",
        "include_answer": False,
        "include_raw_content": False,
        "include_images": False
    }
)

search_results = response.json().get('results', [])
```

### 搜索参数说明

| 参数 | 说明 | 推荐值 |
|-----|------|-------|
| `query` | 搜索主题 | 用户输入 |
| `search_depth` | 搜索深度 | `basic` / `advanced` |
| `max_results` | 最大结果数 | `5` |
| `topic` | 主题类型 | `general` / `tech` / `business` |
| `include_answer` | 是否包含答案摘要 | `False` |
| `include_raw_content` | 是否包含原始内容 | `False` |
| `include_images` | 是否包含图片 | `False` |

---

## 3. HAP MCP 获取人设规则

### 技术方案

**MCP 协议**: Model Context Protocol

**HAP 平台**: 明道云 (mingdao.com)

**功能**: 从 HAP "账号人设" 表读取账号定位、内容风格、内容原则等配置

### HAP 表结构

**表名**: 账号人设 (alias: `account_persona`)

| 字段名 | 字段类型 | 说明 |
|--------|---------|------|
| platform_name | Text（标题） | 平台名称 |
| platform_type | SingleSelect | 平台类型（小红书/抖音/微信公众号/视频号） |
| positioning | Text（多行） | 账号定位（**必需**） |
| target_audience | Text（多行） | 目标受众（**必需**） |
| content_direction | Text（多行） | 内容方向（**必需**） |
| content_style | Text（多行） | 内容风格（**必需**） |
| content_principles | Text（多行） | 内容原则（**必需**） |
| image_generation_prompt | Text（多行） | 图片生成提示词 |

### 使用方式

**1. 在 HAP "账号人设" 表中添加配置**

```
平台名称: 小红书 - AI 产品经理
平台类型: 小红书
账号定位: IT公司产品经理，分享 AI 工具和效率技巧
目标受众: 产品经理、互联网从业者、AI 爱好者
内容方向: AI 工具科普、效率提升技巧、产品思维
内容风格: 专业但不高冷，用通俗易懂的语言解释复杂概念
内容原则:
✅ 用简单语言解释复杂技术
✅ 结合实际应用场景
❌ 避免过度使用专业术语
图片生成提示词: 手绘风格插画，清新活泼，明亮饱和的色彩
```

**2. AI 自动调用流程**

```python
# Step 1: 确认当前连接的 HAP MCP 应用
current_app = mcp_hap_get_app_info()

# Step 2: 动态获取 "账号人设" 表
worksheets = mcp_hap_get_app_worksheets_list()
persona_worksheet_id = find_worksheet_by_alias(worksheets, "account_persona")

# Step 3: 查询对应平台的账号人设
personas = mcp_hap_get_record_list(
    worksheet_id=persona_worksheet_id,
    filter={
        "type": "group",
        "logic": "AND",
        "children": [
            {
                "type": "condition",
                "field": "platform_type",
                "operator": "eq",
                "value": ["小红书"]
            }
        ]
    }
)

persona = personas[0]

# Step 4: 读取所有必需字段
positioning = persona['positioning']
target_audience = persona['target_audience']
content_direction = persona['content_direction']
content_style = persona['content_style']  # 必须严格遵循
content_principles = persona['content_principles']  # 必须严格遵守
image_prompt_base = persona['image_generation_prompt']
```

---

## 4. AI 生成内容 (标题 + 正文 + 配图说明)

### 技术方案

**AI 模型**: Claude Sonnet 4.5

**功能**: 根据搜索资料 + 账号人设生成符合定位的内容

### 生成流程

**1. 内容生成（必须遵循账号人设）**

```python
# Step 1: 读取账号人设（必须在生成内容前）
persona = get_account_persona(platform_type="小红书")

# Step 2: 结合搜索资料 + 账号人设生成内容
content = generate_content(
    search_results=search_results,
    positioning=persona['positioning'],
    target_audience=persona['target_audience'],
    content_direction=persona['content_direction'],
    content_style=persona['content_style'],  # 严格遵循
    content_principles=persona['content_principles']  # 严格遵守
)
```

**2. 标题生成（5-10个备选）**

参考小红书爆款标题风格：

| 标题类型 | 示例 |
|---------|------|
| 疑问式 | "MCP 是什么？AI 终于能自己干活了" |
| 数字式 | "3分钟搞懂 agentic AI！" |
| 痛点式 | "还在手动操作？agentic AI 让你一次指令全搞定" |
| 反差式 | "AI 不再'等指令'！agentic 让 AI 主动工作" |
| 揭秘式 | "揭秘 agentic：AI 从'问答机'到'智能助手'的进化" |
| 结果式 | "AI 也能自主决策了！agentic 到底是什么？" |
| 对比式 | "传统 AI vs agentic AI：从被动响应到主动执行" |
| 趋势式 | "agentic 是什么？AI 的'数字员工'时代来了" |

**3. 配图建议生成**

```
封面图: 展示核心概念，包含封面标题文字
对比图: 左右分屏对比，展示技术/产品差异
流程图: 展示工作流程或技术流程
应用场景图: 展示实际应用场景
```

---

## 5. 图片生成 API

### 支持的 API 服务

#### 5.1 TUZI API (OpenAI 兼容接口)

**API 地址**: `https://api.tu-zi.com/v1`

**注册地址**: [https://api.tu-zi.com/register?aff=n6X4](https://api.tu-zi.com/register?aff=n6X4)

**特点**:
- OpenAI 兼容接口，使用简单
- 支持多个图片生成模型
- 可调用 Nano Banana、DALL-E 等主流模型
- 统一的 API 格式，方便切换模型

**支持的模型**:
- `gemini-3-pro-image-preview` (推荐)
- `dall-e-3`
- `dall-e-2`
- Nano Banana 系列模型

**配置示例**:

```
功能名称: TUZI 图片生成
服务提供商: TUZI
服务类型: 图片生成
是否默认: 是（勾选）
API Key: sk-xxx...
Endpoint URL: https://api.tu-zi.com/v1
配置状态: 启用
```

**调用代码**:

```python
from openai import OpenAI

# 从 HAP 读取配置
api_config = get_api_config(service_type="图片生成", is_default=True)
api_key = api_config['api_key']
endpoint_url = api_config['endpoint_url']

# 初始化 OpenAI 客户端
client = OpenAI(
    api_key=api_key,
    base_url=endpoint_url
)

# 生成图片
response = client.images.generate(
    model="gemini-3-pro-image-preview",
    prompt=full_prompt,
    n=1,
    response_format="url",  # 返回 URL 格式
    size="768x1024"  # 小红书 3:4 格式
)

image_url = response.data[0].url
```

#### 5.2 火山引擎即梦 API

**API 地址**: `https://ark.cn-beijing.volces.com/api/v3`

**特点**: 火山引擎官方图片生成服务，文生图 3.1 版本

**配置示例**:

```
功能名称: 即梦图片生成
服务提供商: 火山引擎
服务类型: 图片生成
Access Key ID: (用户填写)
Secret Access Key: (用户填写)
Region: cn-beijing
Service: cv
API版本: jimeng_t2i_v31
SDK名称: volcenginesdkcv20240606
配置状态: 启用
```

**调用代码**:

```python
from volcenginesdkcv20240606.cv_20240606_service import CvService

# 从 HAP 读取配置
api_config = get_api_config(provider="火山引擎")
access_key_id = api_config['access_key_id']
secret_access_key = api_config['secret_access_key']
region = api_config['region']

# 初始化服务
cv_service = CvService()
cv_service.set_ak(access_key_id)
cv_service.set_sk(secret_access_key)
cv_service.set_region(region)

# 生成图片
req = {
    "req_key": f"image_{timestamp}",
    "prompt": full_prompt,
    "model_version": "jimeng_t2i_v31",
    "width": 768,
    "height": 1024,
    "scale": 3.5,
    "seed": -1,
    "use_sr": False
}

resp = cv_service.cv_process(req)
image_url = resp['data']['image_urls'][0]
```

#### 5.3 Nano Banana API

**特点**: 高性价比的图片生成 API

**配置示例**:

```
功能名称: Nano Banana 图片生成
服务提供商: Nano Banana
服务类型: 图片生成
API Key: (用户填写)
Endpoint URL: https://api.nanobanana.com/v1
配置状态: 启用
```

### 图片生成完整流程

```python
# Step 1: 从 HAP "账号人设" 表读取图片风格
persona = get_account_persona(platform_type="小红书")
image_prompt_base = persona['image_generation_prompt']

# Step 2: 从 HAP "API服务配置" 表读取图片生成 API 配置
# 优先选择 is_default = 1 的配置
api_config = get_api_config(service_type="图片生成", is_default=True)

# Step 3: 结合人设图片风格 + 配图建议生成完整提示词
full_prompt = f"{image_prompt_base}，{image_content_description}"

# Step 4: 调用 API 生成图片（支持并行生成）
images = []
with ThreadPoolExecutor(max_workers=3) as executor:
    futures = [
        executor.submit(generate_image, cover_prompt),
        executor.submit(generate_image, comparison_prompt),
        executor.submit(generate_image, flow_prompt)
    ]
    for future in futures:
        image_url = future.result()
        images.append(image_url)

# Step 5: 自动下载图片到本地
for i, image_url in enumerate(images):
    download_image(image_url, f"配图/{image_names[i]}.png")

# Step 6: 保存图片 URL 到 JSON
save_image_urls(images, "配图/image_urls.json")
```

### 图片格式要求（小红书）

| 项目 | 要求 |
|-----|------|
| 尺寸 | 768x1024（3:4竖屏） |
| 风格 | 手绘风格插画（清新活泼） |
| 封面图 | 必须包含封面标题文字 |
| 配图 | 与正文内容强相关 |

---

## 6. Playwright MCP 自动发布

### 技术方案

**MCP 服务器**: [playwright-mcp](https://github.com/microsoft/playwright-mcp)

**功能**: 使用 Playwright 自动化发布到各大平台

### 安装配置

**自动安装流程（AI 执行）**:

```bash
# Step 1: 检查是否已安装 npx
which npx

# Step 2: 如果未安装，使用 brew 安装 node
brew install node

# Step 3: 在 ~/.claude/mcp.json 中添加 Playwright MCP 配置
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "-y",
        "@executeautomation/playwright-mcp-server"
      ]
    }
  }
}

# Step 4: 验证安装
# AI 会调用 playwright MCP 的基础功能进行验证
```

### 小红书发布流程

```python
# Step 1: 导航到小红书创作者中心
playwright_navigate(url="https://creator.xiaohongshu.com/publish/publish?type=image_text")

# Step 2: 等待页面加载
time.sleep(3)

# Step 3: 上传图片
# 定位文件输入元素并上传图片
image_files = [
    "配图/封面图.png",
    "配图/对比图.png",
    "配图/流程图.png"
]
playwright_fill(selector='input[type="file"]', files=image_files)

# Step 4: 填写标题
playwright_fill(
    selector='textarea[placeholder*="填写标题"]',
    value=cover_title
)

# Step 5: 填写正文（包含话题标签）
content_with_tags = f"{content}\n\n{tags}"
playwright_fill(
    selector='textarea[placeholder*="填写正文"]',
    value=content_with_tags
)

# Step 6: 等待用户确认
# AI 会截图展示预览，等待用户确认

# Step 7: 点击发布
playwright_click(selector='button:has-text("发布")')

# Step 8: 等待发布成功，获取笔记链接
time.sleep(5)
note_url = playwright_evaluate(script="window.location.href")
```

### 支持的平台

| 平台 | 发布页面 | 特殊要求 |
|-----|---------|---------|
| 小红书 | https://creator.xiaohongshu.com/publish/publish?type=image_text | 需要先登录 |
| 抖音 | https://creator.douyin.com/ | 需要先登录 |
| 微信公众号 | https://mp.weixin.qq.com/ | 需要先登录 |
| 视频号 | (通过微信公众号后台) | 需要先登录 |

---

## 7. HAP MCP 数据回写

### 技术方案

**功能**: 将发布的内容、图片、链接等数据回写到 HAP "内容管理" 表

### HAP 表结构

**表名**: 内容管理 (alias: `content_management`)

| 字段名 | 字段类型 | 说明 |
|--------|---------|------|
| content_title | Text（标题） | 内容标题 |
| platform_type | SingleSelect | 平台类型 |
| cover_title | Text | 封面标题 |
| content | Text（多行） | 正文内容 |
| tags | Text | 话题标签 |
| cover_image | Attachment | 封面图（URL） |
| other_images | Attachment | 其他配图（URL） |
| related_topic | Relation | 关联选题 |
| publish_status | SingleSelect | 发布状态 |
| publish_time | DateTime | 发布时间 |
| note_url | Text | 内容链接 |
| views | Number | 浏览量 |
| likes | Number | 点赞数 |
| collections | Number | 收藏数 |
| comments | Number | 评论数 |

### 数据回写流程

```python
# Step 1: 准备附件数据（只上传 URL，不上传 base64）
cover_image_attachment = [
    {
        "name": "封面图.png",
        "url": cover_image_url  # 图片 URL
    }
]

other_images_attachments = [
    {
        "name": "对比图.png",
        "url": comparison_image_url
    },
    {
        "name": "流程图.png",
        "url": flow_image_url
    }
]

# Step 2: 创建内容记录
mcp_hap_create_record(
    worksheet_id=content_worksheet_id,
    fields=[
        {"id": "content_title", "value": content_title},
        {"id": "platform_type", "value": ["小红书"]},
        {"id": "cover_title", "value": cover_title},
        {"id": "content", "value": content},
        {"id": "tags", "value": tags},
        {"id": "cover_image", "value": cover_image_attachment},
        {"id": "other_images", "value": other_images_attachments},
        {"id": "related_topic", "value": [topic_id]},  # 如果有关联选题
        {"id": "publish_status", "value": ["已发布"]},
        {"id": "publish_time", "value": datetime.now().isoformat()},
        {"id": "note_url", "value": note_url}
    ],
    triggerWorkflow=True
)

# Step 3: 如果有关联选题，更新选题状态
if related_topic_id:
    mcp_hap_update_record(
        worksheet_id=topic_selection_worksheet_id,
        row_id=related_topic_id,
        fields=[
            {"id": "topic_status", "value": ["已创作"]}
        ]
    )
```

### 数据追踪

**定期更新数据**:

```python
# 定期从平台抓取数据（使用 Playwright MCP）
playwright_navigate(url=note_url)
time.sleep(3)

# 获取数据
views = playwright_evaluate(script="document.querySelector('.views').innerText")
likes = playwright_evaluate(script="document.querySelector('.likes').innerText")
collections = playwright_evaluate(script="document.querySelector('.collections').innerText")
comments = playwright_evaluate(script="document.querySelector('.comments').innerText")

# 更新 HAP 记录
mcp_hap_update_record(
    worksheet_id=content_worksheet_id,
    row_id=content_record_id,
    fields=[
        {"id": "views", "value": views},
        {"id": "likes", "value": likes},
        {"id": "collections", "value": collections},
        {"id": "comments", "value": comments}
    ]
)
```

---

## 完整技术栈总结

| 序号 | 技术方案 | 技术栈 | 用途 |
|-----|---------|-------|------|
| 1 | 平台热点 API | Hot News API | 获取各大平台热点新闻 |
| 2 | 关键词搜索 API | Tavily Search API | 搜索主题相关资料 |
| 3 | HAP MCP 获取人设 | HAP MCP + 明道云 | 读取账号人设配置 |
| 4 | AI 生成内容 | Claude Sonnet 4.5 | 生成标题、正文、配图建议 |
| 5 | 图片生成 API | TUZI/即梦/Nano Banana | 生成封面图和配图 |
| 6 | Playwright MCP | playwright-mcp | 自动发布到各大平台 |
| 7 | HAP MCP 数据回写 | HAP MCP + 明道云 | 保存内容和数据到 HAP |

---

## 数据流转图

```
┌─────────────┐
│   用户输入   │
└──────┬──────┘
       │
       ▼
┌─────────────┐     ┌──────────────┐
│ Hot News API├────→│  热点数据     │
└─────────────┘     └──────┬───────┘
                           │
                           ▼
┌─────────────┐     ┌──────────────┐
│ Tavily API  ├────→│  搜索资料     │
└─────────────┘     └──────┬───────┘
                           │
                           ▼
┌─────────────┐     ┌──────────────┐
│  HAP MCP    ├────→│  账号人设     │
│ (账号人设表) │     └──────┬───────┘
└─────────────┘            │
                           ▼
                    ┌──────────────┐
                    │ AI 生成内容   │
                    │ (Claude 4.5) │
                    └──────┬───────┘
                           │
                           ▼
┌─────────────┐     ┌──────────────┐
│图片生成 API  ├────→│  配图 URL     │
└─────────────┘     └──────┬───────┘
                           │
                           ▼
┌─────────────┐     ┌──────────────┐
│Playwright   ├────→│  发布成功     │
│    MCP      │     │  (获取链接)   │
└─────────────┘     └──────┬───────┘
                           │
                           ▼
┌─────────────┐     ┌──────────────┐
│  HAP MCP    ├────→│ 数据保存完成  │
│ (内容管理表) │     └──────────────┘
└─────────────┘
```

---

## 相关资源

- [Hot News API (GitHub)](https://github.com/orz-ai/hot_news)
- [Tavily Search API](https://www.tavily.com/)
- [明道云 HAP](https://www.mingdao.com/)
- [Playwright MCP (GitHub)](https://github.com/microsoft/playwright-mcp)
- [TUZI API](https://api.tu-zi.com/register?aff=n6X4) - 图片生成平台（支持 Nano Banana 等多模型）
- [火山引擎即梦 API 文档](https://www.volcengine.com/docs/6791/1209632)

---

**免责声明**: 本文档仅供技术学习和参考使用。文中提到的所有 API 服务仅为技术方案示例，用户可根据实际需求选择任何符合要求的 API 服务。使用任何第三方服务前，请仔细阅读其服务条款，并自行承担使用风险。推广链接仅为方便用户注册使用，不构成任何投资建议或服务推荐。
