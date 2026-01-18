# 快速部署指南

本文档提供一键式部署步骤,帮助用户 5 分钟内完成系统配置。

## 部署步骤

### 第 1 步: 安装 Skill

将本 skill 复制到 Claude Code 的 skills 目录:

```bash
# 方式 1: 从项目目录复制
cp -r social-media-pro ~/.claude/skills/

# 方式 2: 从 GitHub 克隆
git clone https://github.com/your-org/social-media-pro.git
cd social-media-pro
cp -r . ~/.claude/skills/social-media-pro/
```

### 第 2 步: 配置 HAP MCP

编辑 `~/.claude/mcp.json` 文件:

```json
{
  "mcpServers": {
    "hap-mcp-social-media": {
      "url": "https://api.mingdao.com/mcp?HAP-Appkey=YOUR_APPKEY&HAP-Sign=YOUR_SIGN"
    }
  }
}
```

**获取 Appkey 和 Sign:**
1. 登录明道云后台
2. 进入"应用管理" → "API密钥"
3. 创建新的 API 密钥
4. 复制 Appkey 和 Sign，替换上面的 `YOUR_APPKEY` 和 `YOUR_SIGN`

### 第 3 步: 在 HAP 中创建应用和工作表

#### 方式 1: 使用 AI 自动创建（推荐）

在 Claude Code 中运行:

```
请帮我在 HAP 中创建社交媒体运营应用，包含以下工作表:
1. 内容管理表 (alias: content_management)
2. 账号人设表 (alias: account_persona)
3. API服务配置表 (alias: api_service_config)
4. 话题标签库表 (alias: topic_library)
5. 选题库表 (alias: topic_selection)
6. 热点平台关注表 (alias: hot_platform_follow)

请按照 HAP_CONFIG_EXAMPLE.md 中的字段配置创建
```

#### 方式 2: 手动创建

1. 登录明道云后台
2. 创建新应用"社交媒体运营"
3. 按照 [HAP_CONFIG_EXAMPLE.md](HAP_CONFIG_EXAMPLE.md) 创建 6 个工作表
4. 为每个工作表设置对应的别名

### 第 4 步: 配置账号人设

在 HAP "账号人设" 表中添加一条记录:

```
平台名称: 小红书 - [你的账号名称]
平台类型: 小红书
账号定位: [填写你的账号定位，如：IT公司产品经理，分享 AI 工具和效率技巧]
目标受众: [填写目标受众，如：产品经理、互联网从业者、AI 爱好者]
内容方向: [填写内容方向，如：AI 工具科普、效率提升技巧、产品思维]
内容风格: [填写内容风格，如：专业但不高冷，用通俗易懂的语言解释复杂概念]
内容原则: [填写内容原则，如：
✅ 用简单语言解释复杂技术
✅ 结合实际应用场景
❌ 避免过度使用专业术语
]
图片生成提示词: [填写图片风格，如：手绘风格插画，清新活泼，明亮饱和的色彩]
```

### 第 5 步: 配置 API 服务

在 HAP "API服务配置" 表中添加以下配置:

#### 1. Tavily 搜索 API（必需）

```
功能名称: Tavily 搜索API
服务提供商: Tavily
服务类型: 搜索API
API Key: [你的 Tavily API Key]
Endpoint URL: https://api.tavily.com
配置状态: 启用
```

**获取 API Key:**
1. 访问 [Tavily](https://www.tavily.com/)
2. 注册账号并登录
3. 在控制台获取 API Key

#### 2. TUZI 图片生成 API（推荐）

```
功能名称: TUZI 图片生成
服务提供商: TUZI
服务类型: 图片生成
是否默认: 是（勾选）
API Key: [你的 TUZI API Key]
Endpoint URL: https://api.tu-zi.com/v1
配置状态: 启用
```

**获取 API Key:**
1. 访问 [TUZI](https://api.tu-zi.com/)
2. 注册账号并登录
3. 在控制台获取 API Key

#### 3. 热点新闻 API（可选）

```
功能名称: 热点新闻API
服务提供商: orz.ai
服务类型: 热点新闻
Endpoint URL: https://orz.ai/api/v1/dailynews
配置状态: 启用
```

### 第 6 步: 添加话题标签

在 HAP "话题标签库" 表中添加一些话题标签:

```
话题名称: #AI工具
话题类型: AI技术
热度等级: 高热度

话题名称: #效率提升
话题类型: 效率工具
热度等级: 高热度

话题名称: #产品经理
话题类型: 产品设计
热度等级: 中热度
```

### 第 7 步: 配置热点平台关注（可选）

在 HAP "热点平台关注表" 中添加你关注的平台:

```
平台名称: 36氪
平台代码: tskr
平台类型: 科技
是否关注: 是（勾选）
优先级: 9

平台名称: 少数派
平台代码: sspai
平台类型: 科技
是否关注: 是（勾选）
优先级: 8
```

### 第 8 步: 验证配置

在 Claude Code 中运行:

```
请帮我验证 social-media-pro skill 的配置是否完整
```

## 验证清单

部署完成后，请确认以下内容:

- [ ] Skill 已安装到 `~/.claude/skills/social-media-pro/`
- [ ] HAP MCP 已配置在 `~/.claude/mcp.json`
- [ ] HAP 中已创建所有必需的工作表
- [ ] 每个工作表都设置了别名
- [ ] "账号人设" 表中已添加账号配置
- [ ] "API服务配置" 表中已添加 Tavily API 配置
- [ ] "API服务配置" 表中已添加图片生成 API 配置
- [ ] "话题标签库" 表中已添加话题标签
- [ ] 所有 API Key 都已填写且有效

## 测试运行

### 测试 1: 生成小红书笔记

```
帮我生成一篇关于 "Claude Code 使用技巧" 的小红书笔记
```

**预期结果:**
- AI 会自动调用 Tavily API 搜索资料
- AI 会从 HAP 读取账号人设
- AI 会生成符合人设的内容（300-400字）
- AI 会生成 5-10 个封面标题备选
- AI 会从 HAP 话题库选择标签
- AI 会生成配图建议

### 测试 2: 每日热点推荐（需要配置热点平台）

```
推荐今日热点内容
```

**预期结果:**
- AI 会从 HAP 读取关注的热点平台
- AI 会调用 Hot News API 获取热点
- AI 会基于账号人设分析选题
- AI 会以表格形式展示 5-10 个选题建议
- 选题会按相关度评分排序

### 测试 3: 生成图片

```
帮我为刚才生成的笔记生成配图
```

**预期结果:**
- AI 会从 HAP 读取图片生成 API 配置
- AI 会结合账号人设的图片风格生成提示词
- AI 会调用图片生成 API 生成图片
- 图片会保存到本地 `内容/小红书/{笔记标题}/配图/` 目录
- 图片 URL 会保存到 `image_urls.json`

## 常见问题

### 1. MCP 连接失败

**原因**: Appkey 或 Sign 不正确

**解决方法**:
1. 检查 `~/.claude/mcp.json` 中的配置
2. 重新生成 API 密钥
3. 确认 Appkey 和 Sign 没有多余的空格

### 2. 工作表找不到

**原因**: 工作表别名未设置或不正确

**解决方法**:
1. 在 HAP 中检查工作表是否已创建
2. 检查别名是否正确设置
3. 尝试使用工作表名称而不是别名

### 3. 账号人设读取失败

**原因**: 账号人设表中没有数据或字段不完整

**解决方法**:
1. 检查 "账号人设" 表中是否有对应平台的数据
2. 检查必需字段是否都有值（content_style、content_principles）
3. 检查 `platform_type` 字段值是否正确

### 4. API 调用失败

**原因**: API Key 不正确或配额用尽

**解决方法**:
1. 检查 API Key 是否正确
2. 检查 API 配额是否用尽
3. 尝试重新生成 API Key

### 5. 图片生成失败

**原因**: 图片生成 API 不可用或网络问题

**解决方法**:
1. 检查图片生成 API 配置是否正确
2. 检查网络连接
3. 尝试切换其他图片生成 API（如有配置）

## 下一步

部署完成后，你可以:

1. **生成内容**: 使用主工作流生成小红书笔记
2. **热点推荐**: 使用热点推荐工作流获取选题灵感
3. **批量生成**: 一次生成多篇内容
4. **自动发布**: 配置 Playwright MCP 实现自动发布
5. **数据分析**: 在 HAP 中查看内容表现数据

## 技术支持

- 项目文档: [README.md](README.md)
- 配置示例: [HAP_CONFIG_EXAMPLE.md](HAP_CONFIG_EXAMPLE.md)
- 问题反馈: [GitHub Issues](https://github.com/your-org/social-media-pro/issues)

## 更新日志

- **v2.0.0** (2026-01-18): 完整流程整合，支持热点推荐、选题库、多平台发布
- **v1.0.0** (2026-01-10): 基础版本，支持小红书笔记生成和发布
