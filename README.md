# MCJPG Minecraft 服务器列表

> 由 MCJPG 维护的开源 Minecraft 服务器列表数据仓库

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/MineJPGcraft/Serverlist?style=social)](https://github.com/MineJPGcraft/Serverlist)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/MineJPGcraft/Serverlist/pulls)

MCJPG 是一个致力于促进 Minecraft 服务器交流的组织。本仓库采用 JSON 格式存储服务器列表数据，为前端应用提供标准化的 API 接口，让服务器展示更加便捷和统一。

## 🌐 数据访问

### MCJPG CDN（推荐）
```
https://serverlist.mcjpg.org/servers.json
```

### GitHub Raw
```
https://raw.githubusercontent.com/MineJPGcraft/Serverlist/main/servers.json
```

## 📊 数据格式

### 完整结构示例

```json
{
  "types": ["生存", "创造", "小游戏", "RPG", "空岛", "起床战争"],
  "versions": ["1.21.4", "1.20.1", "1.19.4", "1.18.2", "1.16.5"],
  "servers": [
    {
      "id": 1,
      "name": "示例服务器",
      "description": "一个优质的 Minecraft 服务器\n支持多行描述，用 \\n 分隔",
      "type": "生存",
      "version": "1.20.1",
      "ip": "play.example.com",
      "link": "https://example.com",
      "icon": "https://serverlist.mcjpg.org/icons/icon.png"
    }
  ]
}
```

### 字段说明

#### 根对象

| 字段 | 类型 | 说明 |
|------|------|------|
| `types` | `string[]` | 服务器类型列表，用于前端筛选 |
| `versions` | `string[]` | Minecraft 版本列表，建议按从新到旧排序 |
| `servers` | `object[]` | 服务器信息数组 |

#### 服务器对象

| 字段 | 类型 | 必填 | 说明 |
|------|------|:----:|------|
| `id` | `number` | ✅ | 服务器唯一标识符，自增整数 |
| `name` | `string` | ✅ | 服务器名称，建议不超过 20 字符 |
| `description` | `string` | ✅ | 服务器描述，使用 `\n` 换行，建议不超过 100 字符 |
| `type` | `string` | ✅ | 服务器类型，必须在 `types` 列表中 |
| `version` | `string` | ✅ | Minecraft 版本，格式如 `1.20.1` |
| `ip` | `string` | ⚪ | 服务器地址，填写后将显示在线状态 |
| `link` | `string` | ✅ | 服务器官网或详情页 URL |
| `icon` | `string\|object` | ✅ | 服务器图标，支持字符串或对象格式 |

#### 图标格式

**简单格式（推荐）：**
```json
"icon": "https://example.com/icon.png"
```

**详细格式：**
```json
"icon": {
  "src": "https://example.com/icon.png",
  "alt": "服务器图标描述",
  "width": 64,
  "height": 64
}
```

## 🚀 快速开始

### 前端集成示例

#### JavaScript/Fetch API
```javascript
fetch('https://serverlist.mcjpg.org/servers.json')
  .then(response => response.json())
  .then(data => {
    console.log(`共有 ${data.servers.length} 个服务器`)
    console.log('服务器类型：', data.types)
    console.log('支持版本：', data.versions)
  })
  .catch(error => console.error('获取失败：', error))
```

#### Vue 3 组合式 API
```vue
<script setup>
import { ref, onMounted } from 'vue'

const servers = ref([])
const types = ref([])
const versions = ref([])

onMounted(async () => {
  const response = await fetch(
    'https://serverlist.mcjpg.org/servers.json'
  )
  const data = await response.json()
  
  servers.value = data.servers
  types.value = data.types
  versions.value = data.versions
})
</script>
```

#### VitePress 组件
```vue
<ServerList api-url="https://serverlist.mcjpg.org/servers.json" />
```

### React 示例
```jsx
import { useState, useEffect } from 'react'

function ServerList() {
  const [data, setData] = useState({ servers: [], types: [], versions: [] })

  useEffect(() => {
    fetch('https://serverlist.mcjpg.org/servers.json')
      .then(res => res.json())
      .then(setData)
  }, [])

  return (
    <div>
      <h1>服务器列表</h1>
      {data.servers.map(server => (
        <div key={server.id}>{server.name}</div>
      ))}
    </div>
  )
}
```

## 📝 提交服务器

欢迎提交优质的 Minecraft 服务器到本列表！

### 提交要求

在提交前，请确保你的服务器满足以下条件：

- ✅️ 通过组织的服务器审核
- ✅ 服务器稳定运行，有良好的社区氛围
- ✅ 遵守 [Minecraft EULA](https://www.minecraft.net/zh-hans/eula)
- ✅ 无恶意内容（病毒、钓鱼、诈骗等）
- ✅ 提供真实准确的服务器信息
- ✅ 服务器图标清晰，建议 64x64 或以上
- ✅ 服务器官网或详情页可正常访问

### 提交步骤

#### 方法一：通过 GitHub（推荐）

1. **Fork 本仓库**
   - 点击右上角的 `Fork` 按钮

2. **编辑 `servers.json`**
   - 在你的 Fork 中找到 `servers.json` 文件
   - 点击编辑按钮（铅笔图标）
   - 在 `servers` 数组中添加你的服务器信息

3. **确保数据正确**
   - 使用 [JSONLint](https://jsonlint.com/) 验证 JSON 格式
   - 确保 `id` 唯一（使用当前最大 ID + 1）
   - 确保 `type` 存在于 `types` 列表中

4. **提交 Pull Request**
   - 提交更改到你的 Fork
   - 回到本仓库，点击 `New Pull Request`
   - 填写 PR 标题：`添加服务器: [你的服务器名]`
   - 在描述中说明服务器特色

5. **等待审核**
   - 维护团队会在 24-48 小时内审核
   - 通过后将合并到主分支

#### 方法二：通过 Issue

如果你不熟悉 Git 操作，可以：

1. [创建 Issue](https://github.com/MineJPGcraft/Serverlist/issues/new)
2. 选择 "添加服务器" 模板
3. 填写服务器信息
4. 提交并等待处理

### 添加示例

```json
{
  "id": 999,
  "name": "我的服务器",
  "description": "一个充满创意的生存服务器\n拥有独特的游戏玩法和友好的社区",
  "type": "生存",
  "version": "1.20.1",
  "ip": "play.myserver.com",
  "link": "https://myserver.com",
  "icon": "https://myserver.com/icon.png"
}
```

**注意事项：**
- 请将新服务器添加到 `servers` 数组的**末尾**
- 如果你的类型不在 `types` 列表中，请同时添加到列表
- 描述不要过长，建议 2-3 行

## 🔄 更新服务器信息

如需更新已有服务器信息：

1. Fork 本仓库
2. 找到对应的服务器（通过 `id` 或 `name`）
3. 修改相关字段
4. 提交 PR，标题格式：`更新服务器: [服务器名] (ID: [id])`
5. 在 PR 说明中注明更新内容

或者直接 [创建 Issue](https://github.com/MineJPGcraft/Serverlist/issues/new) 说明需要更新的内容。

## 🗑️ 删除服务器

如果你的服务器已关闭或需要从列表中移除：

1. [创建 Issue](https://github.com/MineJPGcraft/Serverlist/issues/new)
2. 说明要删除的服务器 ID 和原因
3. 等待维护团队处理

或直接提交 PR 删除对应记录，标题格式：`删除服务器: [服务器名] (ID: [id])`

## 🛡️ 内容审核

为保证列表质量，我们会审核所有提交。以下内容将被拒绝：

| ❌ 拒绝类型 | 说明 |
|-----------|------|
| 虚假信息 | 服务器信息不真实或夸大宣传 |
| 恶意服务器 | 包含病毒、钓鱼、盗号等恶意内容 |
| 违规内容 | 违反 Minecraft EULA 或含不当内容 |
| 重复提交 | 同一服务器多次提交 |
| 低质量服务器 | 长期离线、无人管理或体验极差 |
| 格式错误 | JSON 格式错误或字段缺失 |

## 📜 服务器类型说明

当前支持的服务器类型：

| 类型 | 说明 | 示例 |
|------|------|------|
| 生存 | 原版或模组生存玩法 | 纯净生存、工业生存 |
| 创造 | 创造模式建筑服务器 | 建筑展示、创造世界 |
| 小游戏 | 各类小游戏服务器 | 跑酷、PVP 竞技 |
| RPG | 角色扮演服务器 | 魔法世界、任务冒险 |
| 空岛 | 空岛生存类型 | 传统空岛、科技空岛 |
| 起床战争 | 起床战争玩法 | 标准起床、变种起床 |

如需添加新类型，请在 PR 或 Issue 中说明。

## 🤝 贡献指南

我们欢迎各种形式的贡献！

### 贡献方式

- 🎯 添加新服务器
- 📝 更新服务器信息
- 🐛 报告问题或错误
- 💡 提出改进建议
- 📚 完善文档
- 🌐 翻译为其他语言

### 贡献者

感谢所有为本项目做出贡献的朋友！

<!-- ALL-CONTRIBUTORS-LIST:START -->
<!-- 这里将显示贡献者列表 -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

想要加入贡献者名单？[提交你的第一个 PR](https://github.com/MineJPGcraft/Serverlist/pulls)！

## 📊 数据统计

![GitHub repo size](https://img.shields.io/github/repo-size/MineJPGcraft/Serverlist)
![GitHub last commit](https://img.shields.io/github/last-commit/MineJPGcraft/Serverlist)
![GitHub issues](https://img.shields.io/github/issues/MineJPGcraft/Serverlist)
![GitHub pull requests](https://img.shields.io/github/issues-pr/MineJPGcraft/Serverlist)

访问 [Insights](https://github.com/MineJPGcraft/Serverlist/pulse) 查看更多统计数据。

## 📄 许可证

本项目采用 [MIT License](LICENSE) 开源。

这意味着你可以：
- ✅ 自由使用、复制、修改本项目
- ✅ 用于商业或非商业目的
- ✅ 分发和二次开发

但你必须：
- 📋 保留原许可证和版权声明
- 📝 注明原作者和来源

## 📞 联系我们

- **问题反馈**: [GitHub Issues](https://github.com/MineJPGcraft/Serverlist/issues)
- **功能建议**: [GitHub Discussions](https://github.com/MineJPGcraft/Serverlist/discussions)
- **官方网站**: [MCJPG.org](https://mcjpg.org)
- **加入组织**: 联系管理员获取邀请

## 🌟 Star History

[![Star History Chart](https://api.star-history.com/svg?repos=MineJPGcraft/Serverlist&type=Date)](https://star-history.com/#MineJPGcraft/Serverlist&Date)

## 💖 支持我们

如果这个项目对你有帮助，请考虑：

- ⭐ 给项目点个 Star
- 🔀 Fork 并贡献代码
- 📢 分享给更多人
- 💬 在社交媒体上提及我们

---

<p align="center">
  <b>由 MCJPG 用 ❤️ 维护</b>
  <br>
  <sub>让 Minecraft 服务器交流更简单</sub>
</p>

<p align="center">
  <a href="#top">回到顶部 ⬆️</a>
</p>
