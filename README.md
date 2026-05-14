# 导览器管理系统

一个高效的导览器管理系统，用于管理导览设备的借用、归还和状态跟踪。

## 功能特性

- 📊 **仪表盘**：实时显示设备状态统计和最近活动
- 📱 **设备管理**：管理15套导览器设备，支持添加、编辑、删除
- 🔄 **借用归还**：简化设备借用和归还流程
- 📈 **统计报表**：可视化数据统计和分析
- 👥 **导游管理**：管理导游信息和借用记录
- 📱 **响应式设计**：支持桌面和移动设备

## 快速开始

### 本地使用

1. 下载项目文件
2. 直接用浏览器打开 `index/index.html` 文件
3. 开始使用系统

### 在线访问

该项目已配置为可部署到GitHub Pages。

## 部署说明

### 部署到GitHub Pages

1. Fork本仓库到您的GitHub账户
2. 在GitHub仓库页面中：
   - 点击 "Settings"
   - 选择 "Pages"
   - 在 "Build and deployment" 部分：
     - Source选择 "GitHub Actions"
     - 选择 "Deploy to GitHub Pages" workflow
   - 点击 "Save"
3. 等待自动部署完成
4. 访问生成的GitHub Pages URL

## 技术栈

- HTML5
- CSS3 (Tailwind CSS)
- JavaScript (原生)
- Font Awesome 图标
- Chart.js 图表库

## 数据存储

- 使用浏览器localStorage进行本地数据存储
- 支持数据备份和恢复功能

## 项目结构

```
tour-guide-manager/
├── index/
│   └── index.html          # 主应用文件
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions部署配置
├── README.md               # 项目说明
└── SHARE_GUIDE.md          # 分享指南
```

## 使用说明

### 初始数据

系统预置了：
- 15台导览器设备
- 4位导游信息
- 示例借用记录

### 数据持久化

- 所有数据自动保存到浏览器本地存储
- 清除浏览器缓存会删除数据，请定期备份

## 浏览器支持

- Chrome 80+
- Firefox 75+
- Safari 13+
- Edge 80+

## 许可证

MIT License
