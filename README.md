# Social Media Pro Skill

社交媒体专业运营技能 - 完整的自媒体运营自动化解决方案

## 🚀 一键式自动化搭建

**用户只需提供一个空 HAP 应用的 MCP，AI 会自动完成所有配置！**

### 极简部署（2 分钟）

```
我想搭建社交媒体运营系统，这是我的 HAP MCP 配置：
https://api.mingdao.com/mcp?HAP-Appkey=YOUR_APPKEY&HAP-Sign=YOUR_SIGN
```

**AI 会自动：**
1. ✅ 创建所有 HAP 工作表（6个表，含字段和别名）
2. ✅ 添加初始数据（话题标签、热点平台）
3. ✅ 配置 API 服务（询问 API Key 后自动配置）
4. ✅ 配置 Playwright MCP（自动安装和设置）
5. ✅ 配置账号人设（询问信息后自动创建）
6. ✅ 验证系统配置完整性

**你只需要提供：**
- HAP 空应用的 MCP 配置（Appkey + Sign）
- API Key（Tavily、TUZI 等）
- 账号人设信息（定位、受众、风格等）

详细步骤请参考 [自动化搭建指南](AUTO_SETUP.md)

---

## 传统安装方式（可选）

如果你想手动配置，可以按以下步骤操作：

### 1. 安装 Skill

```bash
# 将 skill 目录复制到 Claude Code 的 skills 目录
cp -r social-media-pro ~/.claude/skills/
```

### 2. 配置 HAP MCP

在 `~/.claude/mcp.json` 中添加 HAP MCP 配置:

```json
{
  "mcpServers": {
    "hap-mcp-your-app-name": {
      "url": "https://api.mingdao.com/mcp?HAP-Appkey=YOUR_APPKEY&HAP-Sign=YOUR_SIGN"
    }
  }
}
```

### 3. 手动创建 HAP 工作表

必须创建以下工作表（建议设置别名）:

1. **内容管理表** (alias: `content_management`)
2. **账号人设表** (alias: `account_persona`)
3. **API服务配置表** (alias: `api_service_config`)
4. **话题标签库表** (alias: `topic_library`)
5. **选题库表** (alias: `topic_selection`)
6. **热点平台关注表** (alias: `hot_platform_follow`)

详细字段结构请参考 [HAP 配置示例](HAP_CONFIG_EXAMPLE.md)

### 4. 配置 API 服务

在 HAP "API服务配置" 表中添加以下配置:

#### Tavily 搜索 API（必需）

- **功能名称**: Tavily 搜索API
- **服务类型**: 搜索API
- **API Key**: `tvly-dev-xxx...`
- **Endpoint URL**: `https://api.tavily.com`
- **配置状态**: 启用

#### TUZI 图片生成 API（推荐）

- **功能名称**: TUZI 图片生成
- **服务类型**: 图片生成
- **API Key**: `sk-xxx...`
- **Endpoint URL**: `https://api.tu-zi.com/v1`
- **是否默认**: 是（勾选）
- **配置状态**: 启用

#### Hot News API（可选，用于热点推荐）

- **功能名称**: 热点新闻API
- **服务类型**: 热点新闻
- **Endpoint URL**: `https://orz.ai/api/v1/dailynews`
- **配置状态**: 启用

### 5. 配置账号人设

在 HAP "账号人设" 表中添加账号配置:

**示例（小红书）:**

- **平台名称**: 小红书 - AI 产品经理
- **平台类型**: 小红书
- **账号定位**: IT公司产品经理，分享 AI 工具和效率技巧
- **目标受众**: 产品经理、互联网从业者、AI 爱好者
- **内容方向**: AI 工具科普、效率提升、产品思维
- **内容风格**: 专业但不高冷，用通俗易懂的语言解释复杂概念
- **内容原则**:
  - ✅ 用简单语言解释复杂技术
  - ✅ 结合实际应用场景
  - ❌ 避免过度使用专业术语
- **图片生成提示词**: 手绘风格插画，清新活泼的卡通风格，明亮饱和的色彩

## 使用方法

### 生成小红书笔记

```
帮我生成一篇关于 "MCP 是什么" 的小红书笔记
```

### 每日热点推荐

```
推荐今日热点内容
```

### 批量生成内容

```
帮我生成一周的小红书笔记（7篇），主题包括：
1. AI 工具推荐
2. 效率提升技巧
3. 产品思维
...
```

## 工作流程

```
用户输入主题
    ↓
热点抓取 (Hot News API)
    ↓
搜索提炼 (Tavily Search)
    ↓
HAP 读取人设 (HAP MCP)
    ↓
Skill 生成内容 (AI + 人设)
    ↓
生成配图 (TUZI/即梦 API)
    ↓
自动发布 (Playwright MCP)
    ↓
数据回流到 HAP (HAP MCP)
```

## 核心特性

- 🚀 **一键式自动化搭建**: 提供 HAP MCP，AI 自动完成所有配置
- ✅ **完整工作流**: 7 步流程，从热点抓取到数据回写
- ✅ **多平台支持**: 小红书、抖音、微信公众号、视频号
- ✅ **热点推荐**: 自动抓取各平台热点，智能推荐选题
- ✅ **人设驱动**: 从 HAP 读取账号人设，确保内容符合定位
- ✅ **图片生成**: 支持 TUZI/即梦/Nano Banana 等多个 API
- ✅ **自动发布**: 使用 Playwright MCP 自动化发布
- ✅ **数据管理**: 所有数据存储在 HAP，方便管理和分析

## 技术方案

完整的 7 步技术流程：

1. **平台热点 API** (Hot News API) - 获取各大平台热点新闻
2. **关键词搜索 API** (Tavily Search) - 搜索主题相关资料
3. **HAP MCP 获取人设** (明道云) - 读取账号人设配置
4. **AI 生成内容** (Claude 4.5) - 生成标题、正文、配图建议
5. **图片生成 API** (TUZI/即梦/Nano Banana) - 生成封面图和配图
6. **Playwright MCP** (自动化) - 自动发布到各大平台
7. **HAP MCP 数据回写** (明道云) - 保存内容和数据到 HAP

详细技术方案请参考 [技术方案详解](TECH_SOLUTIONS.md)

## 注意事项

1. **账号人设必须从 HAP 读取**: 禁止使用本地文件
2. **配置动态读取**: 所有 API 配置必须存储在 HAP
3. **MCP 连接确认**: 执行操作前先确认 MCP 连接
4. **工作表别名匹配**: 优先使用别名匹配工作表
5. **图片格式要求**: 小红书图片必须是 768x1024（3:4竖屏）

## 常见问题

### 1. 如何获取 HAP Appkey 和 Sign?

1. 登录明道云后台
2. 进入"应用管理" → "API密钥"
3. 创建新的 API 密钥
4. 复制 Appkey 和 Sign

### 2. 如何获取 Tavily API Key?

1. 访问 [Tavily](https://www.tavily.com/)
2. 注册账号
3. 在控制台获取 API Key

### 3. 如何获取 TUZI API Key?

1. 访问 [TUZI](https://api.tu-zi.com/)
2. 注册账号
3. 在控制台获取 API Key

### 4. 图片生成失败怎么办?

- 检查 API Key 是否正确
- 检查网络连接
- 检查 API 配额是否用尽
- 尝试切换其他图片生成 API

### 5. 发布失败怎么办?

- 确保已登录目标平台
- 检查 Playwright MCP 是否正确配置
- 检查内容格式是否符合平台要求

## 文档导航

- **[快速入门](README.md)** - 5分钟了解 Skill 功能
- **[自动化搭建指南](AUTO_SETUP.md)** - 一键式部署完整流程
- **[技术方案详解](TECH_SOLUTIONS.md)** - 7 个核心技术方案说明
- **[HAP 配置示例](HAP_CONFIG_EXAMPLE.md)** - HAP 工作表配置参考
- **[Skill 完整文档](SKILL.md)** - 完整技术规范和使用说明

## 技术支持

- 项目地址: [social-media-pro](https://github.com/your-org/social-media-pro)
- 问题反馈: [Issues](https://github.com/your-org/social-media-pro/issues)

## 参考资源

### 开源项目
- [Hot News API](https://github.com/orz-ai/hot_news) - 热点新闻 API
- [Playwright MCP](https://github.com/microsoft/playwright-mcp) - 自动化发布

### 商业服务
- [Tavily Search API](https://www.tavily.com/) - 关键词搜索
- [明道云 HAP](https://www.mingdao.com/) - 数据管理平台
- [TUZI API](https://api.tu-zi.com/) - 图片生成（推荐）
- [火山引擎即梦](https://www.volcengine.com/docs/6791/1209632) - 图片生成

## 许可证

MIT License

---

**开发者**: 基于 social-media-operation 项目实践总结
**版本**: v2.1.0
**最后更新**: 2026-01-18
