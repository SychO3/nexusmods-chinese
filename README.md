# NexusMods 中文化插件

一个用于 Tampermonkey 的浏览器脚本，将 [Nexus Mods](https://www.nexusmods.com/) 网站界面翻译为简体中文。

## ✨ 功能特性

- 🌐 **智能翻译**：仅翻译网站界面元素，保留 Mod 标题和用户撰写的描述内容
- 📅 **日期本地化**：自动将英文日期格式转换为中文习惯格式（如 "15 Nov 2025" → "2025-11-15"）
- 🚫 **广告屏蔽**（可选）：可选择性隐藏网站广告和 Premium 推广
- ⚡ **性能优化**：
  - 使用翻译缓存减少重复计算
  - MutationObserver 智能监听 DOM 变化
  - 支持 Shadow DOM 组件翻译
- 🎯 **精准控制**：智能识别页面类型，应用对应翻译词典
- 💾 **配置持久化**：用户设置保存在本地

## 📦 安装

### 前置要求

1. 安装浏览器扩展 [Tampermonkey](https://www.tampermonkey.net/)
   - Chrome / Edge: [Chrome 网上应用店](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
   - Firefox: [Firefox 附加组件](https://addons.mozilla.org/firefox/addon/tampermonkey/)
   - Safari: [Mac App Store](https://apps.apple.com/app/tampermonkey/id1482490089)

### 安装脚本

**方法 1：直接安装（推荐）**

点击下面的链接，Tampermonkey 会自动打开安装页面：

> 🔗 [安装 nexusmods-chinese.user.js](https://raw.githubusercontent.com/SychO3/nexusmods-chinese/gh-pages/nexusmods-chinese.user.js)

**方法 2：手动安装**

1. 打开 Tampermonkey 管理面板
2. 点击「添加新脚本」标签
3. 复制 `nexusmods-chinese.user.js` 的全部内容
4. 粘贴到编辑器中并保存

## 🚀 使用方法

### 基本使用

安装完成后，访问 [Nexus Mods](https://www.nexusmods.com/) 任意页面，脚本会自动工作：

- ✅ 界面按钮、菜单、标签页等自动翻译为中文
- ✅ 日期和时间显示为中文格式
- ✅ 页面标题自动翻译

### 配置选项

点击浏览器工具栏的 **Tampermonkey 图标** → **NexusMods 中文化插件** → 选择配置项：

#### 广告屏蔽开关

- **广告屏蔽：已开启** - 隐藏网站广告和 Premium 推广
- **广告屏蔽：已关闭** - 显示网站原有内容

> 💡 **提示**：切换配置后页面会自动刷新以应用更改

## 🔧 高级配置

### 忽略翻译的区域

脚本默认**不会翻译**以下内容：

- Mod 详情页的长描述（`.mod_description_container`）
- 使用 Lexical 编辑器的富文本内容（`.prose-lexical.prose`）
- 合集说明、变更日志等用户撰写的内容

### 自定义配置

可以通过修改词典文件 `nexusmods-locals.js` 来扩展翻译内容。词典支持：

- **公共词典**（`public`）：全站通用翻译
- **页面专用词典**：针对特定页面类型的翻译
- **正则规则**（`regexpRules`）：处理动态内容（如日期、数字等）

示例配置：

```javascript
window.NEXUS_I18N = {
  conf: {
    routes: [
      ['^/$', 'home'],                    // 首页
      ['/mods/\\d+', 'mod_detail'],      // Mod 详情页
    ],
    ignoreSelectors: [
      '.mod_description_container',
      '.prose-lexical.prose'
    ],
    regexpRules: [
      // 日期翻译规则
      [/(\d{1,2})\s+(Jan|Feb|Mar|...)\s+(\d{4})/, '{Y}-{M}-{D}', 'date_en_dMY'],
      // 相对时间翻译
      [/(\d+)\s+(weeks?|days?|hours?)\s+ago/, '', 'rel_time_en']
    ]
  },
  'zh-CN': {
    public: {
      'Upload': '上传',
      'Download': '下载',
      // ...更多翻译
    },
    home: {
      'Featured mods': '精选模组'
      // ...首页专用翻译
    }
  }
}
```

## 📝 支持的日期格式

脚本自动识别并转换以下英文日期格式：

| 英文格式 | 中文格式 | 示例 |
|---------|---------|------|
| `15 Nov 2025` | `2025-11-15` | 标准日期 |
| `15 November 2025, 9:16AM` | `2025-11-15 09:16` | 完整日期时间 |
| `4 weeks ago` | `4 周前` | 相对时间 |
| `Uploaded at 21:21 03 Nov 2025` | `上传于 2025-11-03 21:21` | 带前缀日期 |
| `Time range: 7 Days` | `时间范围：7 天` | 时间范围 |

## 🐛 常见问题

### Q: 为什么某些内容没有被翻译？

**A:** 可能的原因：
1. 该内容属于用户撰写的 Mod 描述（脚本设计为不翻译用户内容）
2. 词典中尚未包含该翻译项
3. 内容超过最大长度限制（默认 400 字符）

### Q: 翻译后页面显示错乱怎么办？

**A:** 
1. 刷新页面（Ctrl/Cmd + R）
2. 清除浏览器缓存
3. 确认 Tampermonkey 和脚本都是最新版本

### Q: 如何更新脚本？

**A:** Tampermonkey 会自动检查更新。也可以手动操作：
1. 打开 Tampermonkey 管理面板
2. 找到「NexusMods 中文化插件」
3. 点击「检查更新」

### Q: 脚本会影响网站性能吗？

**A:** 不会。脚本使用了多种优化技术：
- 翻译结果缓存
- 节流机制防止频繁执行
- WeakMap 避免内存泄漏
- 仅翻译可见的短文本

## 🤝 参与贡献

欢迎提交问题和改进建议！

### 报告问题

在 [GitHub Issues](https://github.com/SychO3/nexusmods-chinese/issues) 提交问题时，请提供：
- 问题描述
- 出现问题的页面 URL
- 浏览器版本和 Tampermonkey 版本
- 脚本版本号

### 贡献翻译

如果发现未翻译或翻译不当的内容：
1. 编辑 `nexusmods-locals.js` 词典文件
2. 添加或修改翻译条目
3. 提交 Pull Request

## 📄 许可证

本项目基于 MIT 许可证开源。

## 🔗 相关链接

- **项目主页**：https://github.com/SychO3/nexusmods-chinese
- **问题反馈**：https://github.com/SychO3/nexusmods-chinese/issues
- **Nexus Mods 官网**：https://www.nexusmods.com/

---

## 技术说明

### 工作原理

1. **页面类型检测**：根据 URL 路径识别当前页面类型（首页、Mod 详情页、用户页等）
2. **词典加载**：加载公共词典 + 当前页面专用词典
3. **DOM 遍历**：遍历页面元素，翻译文本节点和属性（placeholder、title 等）
4. **动态监听**：使用 MutationObserver 监听 DOM 变化，实时翻译新内容
5. **Shadow DOM 支持**：Hook `attachShadow` 方法，确保 Web Components 也能被翻译

### 架构设计

```
┌─────────────────────────────────────┐
│   nexusmods-chinese.user.js         │  (主脚本)
│   ├─ 配置管理                        │
│   ├─ 页面类型检测                    │
│   ├─ 词典管理与缓存                  │
│   ├─ DOM 遍历与翻译                  │
│   ├─ MutationObserver 监听           │
│   └─ Shadow DOM 处理                │
└─────────────────────────────────────┘
              ⬇ requires
┌─────────────────────────────────────┐
│   nexusmods-locals.js                │  (词典文件)
│   ├─ 路由配置 (routes)              │
│   ├─ 正则规则 (regexpRules)          │
│   ├─ 公共词典 (public)               │
│   └─ 各页面专用词典                  │
└─────────────────────────────────────┘
```

### 性能特性

- ⚡ **翻译缓存**：避免重复翻译相同文本
- 🎯 **节流机制**：URL 变化时限制翻译频率（500ms 节流）
- 💪 **批量处理**：聚合同一批次的 DOM 变更统一处理
- 🔍 **选择性遍历**：智能跳过不需要翻译的区域
- 🧠 **智能记忆**：记录已翻译的文本节点，避免重复处理

---

**版本**：0.2.2  
**作者**：SychO  
**更新日期**：2025-12-18
