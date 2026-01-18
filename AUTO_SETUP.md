# 自动化搭建指南

本文档说明如何使用 AI 自动搭建整个社交媒体运营系统。

## 一键式自动化部署

**用户只需提供：**
1. 一个空 HAP 应用的 MCP 配置（Appkey + Sign）
2. API Key（Tavily、图片生成等）

**AI 会自动完成：**
1. ✅ 创建所有 HAP 工作表（含字段和别名）
2. ✅ 配置 API 服务记录
3. ✅ 添加初始数据（话题标签、热点平台）
4. ✅ 配置 Playwright MCP
5. ✅ 验证系统配置

---

## 自动化搭建流程

### 第 1 步：用户提供 HAP MCP 配置

用户需要提供一个空 HAP 应用的 MCP 配置：

```json
{
  "mcpServers": {
    "hap-mcp-social-media": {
      "url": "https://api.mingdao.com/mcp?HAP-Appkey=YOUR_APPKEY&HAP-Sign=YOUR_SIGN"
    }
  }
}
```

**触发方式：**

```
我想搭建社交媒体运营系统，这是我的 HAP MCP 配置：
https://api.mingdao.com/mcp?HAP-Appkey=xxx&HAP-Sign=xxx
```

### 第 2 步：AI 自动创建工作表

AI 会自动调用 HAP MCP 创建以下工作表：

#### 2.1 创建"内容管理"表

```python
mcp_hap_create_worksheet(
    name="内容管理",
    alias="content_management",
    fields=[
        {
            "name": "内容标题",
            "type": "Text",
            "isTitle": True
        },
        {
            "name": "平台类型",
            "type": "SingleSelect",
            "options": [
                {"value": "小红书", "index": 1},
                {"value": "抖音", "index": 2},
                {"value": "微信公众号", "index": 3},
                {"value": "视频号", "index": 4}
            ]
        },
        {
            "name": "封面标题",
            "type": "Text"
        },
        {
            "name": "正文内容",
            "type": "Text"
        },
        {
            "name": "话题标签",
            "type": "Text"
        },
        {
            "name": "封面图",
            "type": "Attachment"
        },
        {
            "name": "其他配图",
            "type": "Attachment"
        },
        {
            "name": "Markdown文档",
            "type": "Attachment"
        },
        {
            "name": "发布状态",
            "type": "SingleSelect",
            "options": [
                {"value": "草稿", "index": 1},
                {"value": "待发布", "index": 2},
                {"value": "已发布", "index": 3},
                {"value": "已取消", "index": 4}
            ]
        },
        {
            "name": "发布时间",
            "type": "DateTime",
            "subType": "6"  # 年月日时分秒
        },
        {
            "name": "内容链接",
            "type": "Text"
        },
        {
            "name": "浏览量",
            "type": "Number",
            "precision": 0
        },
        {
            "name": "点赞数",
            "type": "Number",
            "precision": 0
        },
        {
            "name": "收藏数",
            "type": "Number",
            "precision": 0
        },
        {
            "name": "评论数",
            "type": "Number",
            "precision": 0
        }
    ]
)
```

#### 2.2 创建"账号人设"表

```python
mcp_hap_create_worksheet(
    name="账号人设",
    alias="account_persona",
    fields=[
        {
            "name": "平台名称",
            "type": "Text",
            "isTitle": True
        },
        {
            "name": "平台类型",
            "type": "SingleSelect",
            "options": [
                {"value": "小红书", "index": 1},
                {"value": "抖音", "index": 2},
                {"value": "微信公众号", "index": 3},
                {"value": "视频号", "index": 4}
            ]
        },
        {
            "name": "账号定位",
            "type": "Text",
            "required": True
        },
        {
            "name": "目标受众",
            "type": "Text",
            "required": True
        },
        {
            "name": "内容方向",
            "type": "Text",
            "required": True
        },
        {
            "name": "内容风格",
            "type": "Text",
            "required": True
        },
        {
            "name": "内容原则",
            "type": "Text",
            "required": True
        },
        {
            "name": "图片生成提示词",
            "type": "Text"
        }
    ]
)
```

#### 2.3 创建"API服务配置"表

```python
mcp_hap_create_worksheet(
    name="API服务配置",
    alias="api_service_config",
    fields=[
        {
            "name": "功能名称",
            "type": "Text",
            "isTitle": True
        },
        {
            "name": "服务提供商",
            "type": "Text"
        },
        {
            "name": "服务类型",
            "type": "SingleSelect",
            "options": [
                {"value": "搜索API", "index": 1},
                {"value": "图片生成", "index": 2},
                {"value": "热点新闻", "index": 3},
                {"value": "内容生成", "index": 4},
                {"value": "其他", "index": 5}
            ]
        },
        {
            "name": "用途说明",
            "type": "Text"
        },
        {
            "name": "是否默认",
            "type": "Checkbox"
        },
        {
            "name": "Endpoint URL",
            "type": "Text",
            "required": True
        },
        {
            "name": "API Key",
            "type": "Text"
        },
        {
            "name": "Access Key ID",
            "type": "Text"
        },
        {
            "name": "Secret Access Key",
            "type": "Text"
        },
        {
            "name": "Region",
            "type": "Text"
        },
        {
            "name": "Service",
            "type": "Text"
        },
        {
            "name": "API版本",
            "type": "Text"
        },
        {
            "name": "SDK名称",
            "type": "Text"
        },
        {
            "name": "配置状态",
            "type": "SingleSelect",
            "options": [
                {"value": "启用", "index": 1},
                {"value": "禁用", "index": 2}
            ]
        },
        {
            "name": "备注",
            "type": "Text"
        }
    ]
)
```

#### 2.4 创建"话题标签库"表

```python
mcp_hap_create_worksheet(
    name="话题标签库",
    alias="topic_library",
    fields=[
        {
            "name": "话题名称",
            "type": "Text",
            "isTitle": True
        },
        {
            "name": "话题类型",
            "type": "SingleSelect",
            "options": [
                {"value": "AI技术", "index": 1},
                {"value": "效率工具", "index": 2},
                {"value": "产品设计", "index": 3},
                {"value": "技术趋势", "index": 4},
                {"value": "其他", "index": 5}
            ]
        },
        {
            "name": "热度等级",
            "type": "SingleSelect",
            "options": [
                {"value": "高热度", "index": 1},
                {"value": "中热度", "index": 2},
                {"value": "低热度", "index": 3},
                {"value": "已过时", "index": 4}
            ]
        },
        {
            "name": "说明",
            "type": "Text"
        },
        {
            "name": "最后更新",
            "type": "DateTime",
            "subType": "3"  # 年月日
        }
    ]
)
```

#### 2.5 创建"选题库"表

```python
mcp_hap_create_worksheet(
    name="选题库",
    alias="topic_selection",
    fields=[
        {
            "name": "选题标题",
            "type": "Text",
            "isTitle": True
        },
        {
            "name": "选题角度",
            "type": "Text",
            "required": True
        },
        {
            "name": "推荐理由",
            "type": "Text",
            "required": True
        },
        {
            "name": "相关度评分",
            "type": "Number",
            "precision": 0
        },
        {
            "name": "原文链接",
            "type": "Text"
        },
        {
            "name": "内容预览",
            "type": "Text"
        },
        {
            "name": "选题状态",
            "type": "SingleSelect",
            "options": [
                {"value": "待创作", "index": 1},
                {"value": "创作中", "index": 2},
                {"value": "已创作", "index": 3},
                {"value": "已取消", "index": 4}
            ]
        },
        {
            "name": "备注",
            "type": "Text"
        }
    ]
)
```

#### 2.6 创建"热点平台关注"表

```python
mcp_hap_create_worksheet(
    name="热点平台关注",
    alias="hot_platform_follow",
    fields=[
        {
            "name": "平台名称",
            "type": "Text",
            "isTitle": True
        },
        {
            "name": "平台代码",
            "type": "Text",
            "required": True
        },
        {
            "name": "平台类型",
            "type": "SingleSelect",
            "options": [
                {"value": "综合新闻", "index": 1},
                {"value": "科技", "index": 2},
                {"value": "社交媒体", "index": 3},
                {"value": "技术社区", "index": 4},
                {"value": "财经", "index": 5},
                {"value": "其他", "index": 6}
            ]
        },
        {
            "name": "是否关注",
            "type": "Checkbox"
        },
        {
            "name": "优先级",
            "type": "Number",
            "precision": 0
        },
        {
            "name": "平台说明",
            "type": "Text"
        },
        {
            "name": "最后获取时间",
            "type": "DateTime",
            "subType": "6"  # 年月日时分秒
        }
    ]
)
```

#### 2.7 添加表间关联

创建完所有表后，AI 会自动添加表间关联字段：

```python
# 1. 在"内容管理"表中添加"关联选题"字段
mcp_hap_update_worksheet(
    worksheet_id=content_management_id,
    addFields=[
        {
            "name": "关联选题",
            "type": "Relation",
            "dataSource": topic_selection_id,  # 关联选题库表
            "subType": "1",  # 单选
            "relation": {
                "bidirectional": True,
                "showFields": ["选题角度", "相关度评分"]
            }
        }
    ]
)

# 2. 在"选题库"表中添加"来源平台"和"关联内容"字段
mcp_hap_update_worksheet(
    worksheet_id=topic_selection_id,
    addFields=[
        {
            "name": "来源平台",
            "type": "Relation",
            "dataSource": hot_platform_follow_id,  # 关联热点平台关注表
            "subType": "1",  # 单选
            "relation": {
                "bidirectional": False,
                "showFields": ["平台代码", "平台类型"]
            }
        },
        {
            "name": "关联内容",
            "type": "Relation",
            "dataSource": content_management_id,  # 关联内容管理表
            "subType": "2",  # 多选
            "relation": {
                "bidirectional": True,
                "showFields": ["平台类型", "发布状态"]
            }
        }
    ]
)
```

### 第 3 步：AI 添加初始数据

#### 3.1 添加话题标签数据

```python
# 批量创建话题标签
mcp_hap_batch_create_records(
    worksheet_id=topic_library_id,
    rows=[
        {
            "fields": [
                {"id": "话题名称", "value": "#AI工具"},
                {"id": "话题类型", "value": ["AI技术"]},
                {"id": "热度等级", "value": ["高热度"]},
                {"id": "说明", "value": "AI 相关工具和应用的讨论"}
            ]
        },
        {
            "fields": [
                {"id": "话题名称", "value": "#效率提升"},
                {"id": "话题类型", "value": ["效率工具"]},
                {"id": "热度等级", "value": ["高热度"]},
                {"id": "说明", "value": "提升工作效率的技巧和方法"}
            ]
        },
        {
            "fields": [
                {"id": "话题名称", "value": "#产品经理"},
                {"id": "话题类型", "value": ["产品设计"]},
                {"id": "热度等级", "value": ["中热度"]},
                {"id": "说明", "value": "产品经理相关的话题"}
            ]
        }
        # ... 更多话题标签
    ]
)
```

#### 3.2 添加热点平台关注数据

```python
# 批量创建热点平台配置
mcp_hap_batch_create_records(
    worksheet_id=hot_platform_follow_id,
    rows=[
        {
            "fields": [
                {"id": "平台名称", "value": "36氪"},
                {"id": "平台代码", "value": "tskr"},
                {"id": "平台类型", "value": ["科技"]},
                {"id": "是否关注", "value": 1},
                {"id": "优先级", "value": 9},
                {"id": "平台说明", "value": "科技创业和商业资讯平台"}
            ]
        },
        {
            "fields": [
                {"id": "平台名称", "value": "少数派"},
                {"id": "平台代码", "value": "sspai"},
                {"id": "平台类型", "value": ["科技"]},
                {"id": "是否关注", "value": 1},
                {"id": "优先级", "value": 8},
                {"id": "平台说明", "value": "科技、数码、生活方式内容平台"}
            ]
        },
        {
            "fields": [
                {"id": "平台名称", "value": "掘金"},
                {"id": "平台代码", "value": "juejin"},
                {"id": "平台类型", "value": ["技术社区"]},
                {"id": "是否关注", "value": 1},
                {"id": "优先级", "value": 7},
                {"id": "平台说明", "value": "编程和技术文章社区"}
            ]
        }
        # ... 更多平台
    ]
)
```

### 第 4 步：用户提供 API Key

AI 会询问用户提供必需的 API Key：

```
请提供以下 API Key（必需）：

1. Tavily 搜索 API Key:
   - 注册地址: https://www.tavily.com/
   - 用途: 搜索主题相关资料

2. 图片生成 API Key（选择一个即可）:

   选项 1: TUZI API (推荐)
   - 注册地址: https://api.tu-zi.com/
   - 用途: 生成小红书配图

   选项 2: 火山引擎即梦
   - 注册地址: https://www.volcengine.com/
   - 用途: 生成小红书配图

   选项 3: Nano Banana
   - 注册地址: https://nanobanana.com/
   - 用途: 生成小红书配图

3. 热点新闻 API（可选）:
   - 无需 API Key
   - 用途: 获取各平台热点新闻
```

### 第 5 步：AI 配置 API 服务记录

用户提供 API Key 后，AI 会自动创建 API 服务配置记录：

```python
# 1. 配置 Tavily 搜索 API
mcp_hap_create_record(
    worksheet_id=api_service_config_id,
    fields=[
        {"id": "功能名称", "value": "Tavily 搜索API"},
        {"id": "服务提供商", "value": "Tavily"},
        {"id": "服务类型", "value": ["搜索API"]},
        {"id": "用途说明", "value": "用于搜索主题相关资料，获取最新信息"},
        {"id": "API Key", "value": user_provided_tavily_key},
        {"id": "Endpoint URL", "value": "https://api.tavily.com"},
        {"id": "配置状态", "value": ["启用"]}
    ]
)

# 2. 配置 TUZI 图片生成 API（如果用户选择 TUZI）
mcp_hap_create_record(
    worksheet_id=api_service_config_id,
    fields=[
        {"id": "功能名称", "value": "TUZI 图片生成"},
        {"id": "服务提供商", "value": "TUZI"},
        {"id": "服务类型", "value": ["图片生成"]},
        {"id": "是否默认", "value": 1},
        {"id": "用途说明", "value": "用于生成小红书笔记配图（OpenAI 兼容接口）"},
        {"id": "API Key", "value": user_provided_tuzi_key},
        {"id": "Endpoint URL", "value": "https://api.tu-zi.com/v1"},
        {"id": "配置状态", "value": ["启用"]},
        {"id": "备注", "value": "支持模型：gemini-3-pro-image-preview、dall-e-3、dall-e-2"}
    ]
)

# 3. 配置热点新闻 API
mcp_hap_create_record(
    worksheet_id=api_service_config_id,
    fields=[
        {"id": "功能名称", "value": "热点新闻API"},
        {"id": "服务提供商", "value": "orz.ai"},
        {"id": "服务类型", "value": ["热点新闻"]},
        {"id": "用途说明", "value": "用于获取各平台的热点新闻数据，支持每日热点内容推荐"},
        {"id": "Endpoint URL", "value": "https://orz.ai/api/v1/dailynews"},
        {"id": "配置状态", "value": ["启用"]}
    ]
)
```

### 第 6 步：AI 配置 Playwright MCP

AI 会自动检查并配置 Playwright MCP：

```bash
# 1. 检查是否已安装 npx
if ! command -v npx &> /dev/null; then
    echo "未检测到 npx，正在安装 Node.js..."
    brew install node
fi

# 2. 读取现有的 MCP 配置
mcp_config=$(cat ~/.claude/mcp.json)

# 3. 检查是否已配置 Playwright MCP
if ! echo "$mcp_config" | grep -q "playwright"; then
    echo "正在配置 Playwright MCP..."

    # 4. 添加 Playwright MCP 配置
    # AI 会更新 ~/.claude/mcp.json，添加：
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
fi

# 5. 验证 Playwright MCP 安装
# AI 会尝试调用 Playwright MCP 的基础功能
```

### 第 7 步：AI 询问用户配置账号人设

```
HAP 应用搭建完成！现在需要配置账号人设。

请提供以下信息（以小红书为例）：

1. 平台类型: 小红书
2. 账号定位: (例如: IT公司产品经理，分享 AI 工具和效率技巧)
3. 目标受众: (例如: 产品经理、互联网从业者、AI 爱好者)
4. 内容方向: (例如: AI 工具科普、效率提升技巧、产品思维)
5. 内容风格: (例如: 专业但不高冷，用通俗易懂的语言解释复杂概念)
6. 内容原则: (例如:
   ✅ 用简单语言解释复杂技术
   ✅ 结合实际应用场景
   ❌ 避免过度使用专业术语
   )
7. 图片生成提示词: (例如: 手绘风格插画，清新活泼，明亮饱和的色彩)
```

用户提供后，AI 会自动创建账号人设记录：

```python
mcp_hap_create_record(
    worksheet_id=account_persona_id,
    fields=[
        {"id": "平台名称", "value": f"小红书 - {user_provided_account_name}"},
        {"id": "平台类型", "value": ["小红书"]},
        {"id": "账号定位", "value": user_provided_positioning},
        {"id": "目标受众", "value": user_provided_target_audience},
        {"id": "内容方向", "value": user_provided_content_direction},
        {"id": "内容风格", "value": user_provided_content_style},
        {"id": "内容原则", "value": user_provided_content_principles},
        {"id": "图片生成提示词", "value": user_provided_image_prompt}
    ]
)
```

### 第 8 步：AI 验证系统配置

AI 会自动验证所有配置是否完整：

```python
# 1. 检查所有工作表是否创建成功
worksheets = mcp_hap_get_app_worksheets_list()
required_tables = [
    "content_management",
    "account_persona",
    "api_service_config",
    "topic_library",
    "topic_selection",
    "hot_platform_follow"
]

for alias in required_tables:
    if not find_worksheet_by_alias(worksheets, alias):
        raise Exception(f"工作表 {alias} 创建失败")

# 2. 检查账号人设是否配置
personas = mcp_hap_get_record_list(
    worksheet_id=account_persona_id,
    filter={...}
)
if not personas:
    raise Exception("账号人设未配置")

# 3. 检查 API 配置是否完整
api_configs = mcp_hap_get_record_list(
    worksheet_id=api_service_config_id,
    filter={...}
)
if not api_configs:
    raise Exception("API 配置未完成")

# 4. 检查 Playwright MCP 是否配置
# 验证 ~/.claude/mcp.json 中是否有 playwright 配置

# 5. 输出验证结果
print("✅ 系统配置验证通过！")
print("✅ 所有工作表创建成功")
print("✅ 账号人设配置完成")
print("✅ API 配置完成")
print("✅ Playwright MCP 配置完成")
print("\n系统已就绪，可以开始生成内容！")
```

---

## 完整的自动化部署对话示例

```
用户: 我想搭建社交媒体运营系统，这是我的 HAP MCP 配置：
https://api.mingdao.com/mcp?HAP-Appkey=xxx&HAP-Sign=xxx

AI: 好的！我会帮你自动搭建整个系统。

正在创建 HAP 工作表...
✅ 创建"内容管理"表 - 完成
✅ 创建"账号人设"表 - 完成
✅ 创建"API服务配置"表 - 完成
✅ 创建"话题标签库"表 - 完成
✅ 创建"选题库"表 - 完成
✅ 创建"热点平台关注"表 - 完成
✅ 添加表间关联 - 完成

正在添加初始数据...
✅ 添加话题标签（10条）- 完成
✅ 添加热点平台配置（8个平台）- 完成

现在需要配置 API 服务。请提供以下 API Key：

1. Tavily 搜索 API Key（必需）
2. 图片生成 API Key（选择一个）：
   - TUZI API（推荐）
   - 火山引擎即梦
   - Nano Banana

用户: 这是我的 API Key：
- Tavily: tvly-dev-xxx...
- TUZI: sk-xxx...

AI: 收到！正在配置 API 服务...
✅ 配置 Tavily 搜索 API - 完成
✅ 配置 TUZI 图片生成 API - 完成
✅ 配置热点新闻 API - 完成

正在配置 Playwright MCP...
✅ 检测到 npx 已安装
✅ 添加 Playwright MCP 配置到 ~/.claude/mcp.json - 完成
✅ 验证 Playwright MCP 安装 - 完成

最后一步：配置账号人设

请提供以下信息（以小红书为例）：
1. 账号定位:
2. 目标受众:
3. 内容方向:
4. 内容风格:
5. 内容原则:
6. 图片生成提示词:

用户: [提供账号人设信息]

AI: 正在创建账号人设配置...
✅ 账号人设配置完成

正在验证系统配置...
✅ 所有工作表创建成功（6个）
✅ 账号人设配置完成（1条）
✅ API 配置完成（3条）
✅ 初始数据添加完成（话题标签10条，热点平台8个）
✅ Playwright MCP 配置完成

🎉 系统搭建完成！你现在可以开始生成内容了。

试试这些命令：
- "推荐今日热点内容"
- "帮我生成一篇关于 'Claude Code' 的小红书笔记"
- "批量生成一周的小红书笔记"
```

---

## 验证部署成功

部署完成后，用户可以通过以下方式验证：

### 1. 在 HAP 中检查工作表

登录 HAP，检查是否有以下 6 个工作表：
- ✅ 内容管理（含 15 个字段）
- ✅ 账号人设（含 8 个字段）
- ✅ API服务配置（含 15 个字段）
- ✅ 话题标签库（含 5 个字段）
- ✅ 选题库（含 8 个字段）
- ✅ 热点平台关注（含 7 个字段）

### 2. 检查初始数据

- 话题标签库：应该有至少 10 条话题标签
- 热点平台关注：应该有至少 8 个平台配置
- API服务配置：应该有至少 3 条 API 配置
- 账号人设：应该有至少 1 条账号人设配置

### 3. 测试生成内容

```
帮我生成一篇关于 "MCP 是什么" 的小红书笔记
```

如果能成功生成内容，说明系统部署成功！

---

## 故障排查

### 1. 工作表创建失败

**可能原因**：MCP 配置不正确

**解决方法**：
- 检查 Appkey 和 Sign 是否正确
- 确认 HAP 应用是空应用
- 重新生成 API 密钥

### 2. API 配置失败

**可能原因**：API Key 不正确

**解决方法**：
- 检查 API Key 是否正确
- 确认 API Key 是否有效
- 重新生成 API Key

### 3. Playwright MCP 配置失败

**可能原因**：npx 未安装或权限不足

**解决方法**：
```bash
# 安装 Node.js
brew install node

# 手动添加 Playwright MCP 配置
# 编辑 ~/.claude/mcp.json
```

---

## 后续步骤

部署完成后，你可以：

1. **生成第一篇内容**: `帮我生成一篇关于 "AI 工具" 的小红书笔记`
2. **热点推荐**: `推荐今日热点内容`
3. **批量生成**: `帮我生成一周的小红书笔记`
4. **自动发布**: `发布到小红书`
5. **数据分析**: 在 HAP 中查看内容表现数据

---

## 相关文档

- [技术方案详解](TECH_SOLUTIONS.md)
- [HAP 配置示例](HAP_CONFIG_EXAMPLE.md)
- [快速入门指南](README.md)
