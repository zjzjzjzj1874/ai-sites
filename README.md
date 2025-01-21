<!--
 * @Author: zhoujian zjzjzjzj1874@gmail.com
 * @Date: 2025-01-21 15:54:56
 * @LastEditors: zjzjzjzj1874 zjzjzjzj1874@gmail.com
 * @LastEditTime: 2025-01-21 16:57:15
 * @FilePath: /ai-site/README.md
 * @Description: AI导航站点
-->

# AI导航站点

一个简洁美观的个人导航网站，支持网站优先级排序和点击量统计功能。

## 预览

![网站预览](./src/assets/preview.png)

## 开发者信息

- **作者**: zhoujian (zjzjzjzj1874@gmail.com)
- **开发工具**:
  - [Trae AI](https://trae.ai): 主要开发工具 
  - Git: 版本控制
  - Chrome DevTools: 调试工具

## 项目背景

在日常工作和学习中，我们经常需要同时打开多个网站和工具，这导致浏览器开启大量标签页，不仅增加了系统内存占用，还降低了工作效率。为了解决这个问题，我开发了这个AI导航站点，它可以集中管理所有常用的网站链接，让用户可以快速访问所需的网站，同时避免了开启过多标签页的困扰。

## 解决的痛点

1. **标签页管理混乱**：浏览器开启过多标签页导致管理困难，找寻特定网站耗时费力
2. **系统资源占用大**：大量标签页会占用大量内存，影响系统性能
3. **工作效率低下**：在众多标签页中切换降低工作效率
4. **常用网站分散**：重要的网站链接分散在各处，访问不便

## 解决方案

- 集中管理：将所有常用网站集中在一个界面，方便快速访问
- 智能排序：根据访问频率和优先级自动调整网站顺序，提高使用效率
- 数据统计：记录访问频率，帮助用户了解自己的使用习惯
- 轻量级设计：采用简洁的卡片式布局，占用资源少，加载迅速

## 功能特点

- 🎯 支持网站优先级设置
- 📊 自动统计网站点击量
- 🔄 根据优先级和点击量智能排序
- 💾 本地存储点击数据
- 📱 响应式布局，支持多端访问
- 🎨 简洁美观的卡片式设计

## 技术栈

- Vue 3：前端框架
- Vite：构建工具
- LocalStorage：本地数据存储

## 项目结构

```
├── src/
│   ├── data/           # 数据文件
│   │   ├── sites.json  # 网站配置
│   │   └── clicks.json # 点击统计
│   ├── App.vue        # 主组件
│   └── main.js        # 入口文件
└── vite.config.js     # Vite配置
```

## 安装运行

1. 克隆项目
```bash
git clone git@github.com:zjzjzjzj1874/ai-sites.git
cd ai-sites
```

2. 安装依赖
```bash
npm install
```

3. 开发环境运行
```bash
npm run dev
```

4. 构建生产环境
```bash
npm run build
```

## 使用说明

### 基本使用

1. 启动项目后，你会看到一个包含多个网站卡片的导航页面
2. 点击任意卡片即可跳转到对应网站
3. 网站卡片会根据优先级和点击量自动排序

### 自定义配置

#### 添加新网站

1. 打开 `src/data/sites.json` 文件
2. 按照以下格式添加新的网站配置：

```json
{
  "name": "网站名称",
  "url": "网站地址",
  "icon": "图标地址",
  "priority": 优先级数字(0, ++)
}
```

- priority说明：
  - ++：最高优先级，适用于重要且频繁访问的网站（如公司内网、项目管理工具等），这些网站会始终显示在最前面
  - 100：较高优先级，适用于重要且频繁访问的网站（如公司内网、项目管理工具等），这些网站会始终显示在最前面
  - 1：中等优先级，适用于经常使用但不是最重要的网站（如开发文档、常用工具等），这些网站会在高优先级网站之后，根据点击量排序
  - 0：普通优先级，适用于偶尔访问的网站，这些网站会根据点击量自动排序，访问频率越高排序越靠前
  > 优先级越高，网站卡片会显示在最前面，优先级越低，网站卡片会显示在最后面；0的优先级，会根据点击量来排序

#### 修改现有网站

1. 在 `src/data/sites.json` 中找到要修改的网站配置
2. 直接修改对应的属性值
3. 保存文件后，页面会自动更新

#### 删除网站

1. 打开 `src/data/sites.json` 文件
2. 找到并删除对应网站的配置对象
3. 保存文件后，页面会自动更新

### 数据存储

- 网站配置数据存储在 `src/data/sites.json` 文件中
- 点击量数据存储在浏览器的 LocalStorage 中，可以通过清除浏览器数据来重置点击量

## 贡献

欢迎提交Issue和Pull Request！

## 开源协议

MIT License
