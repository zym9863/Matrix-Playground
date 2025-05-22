# 矩阵魔方 (Matrix Playground)

[English](./README_EN.md) | 中文

🧮 一个现代化的交互式矩阵运算和线性代数学习平台

## 📖 项目简介

矩阵魔方是一个基于 Vue 3 + TypeScript + Vite 构建的现代化 Web 应用，旨在让线性代数的学习变得更加直观和有趣。通过可视化的矩阵编辑器、丰富的运算功能和算法演示，帮助用户深入理解矩阵运算的原理和过程。

## ✨ 主要特性

### 🎯 核心功能
- **交互式矩阵编辑器**: 支持动态调整矩阵维度（1×1 到 10×10）
- **丰富的矩阵运算**: 加法、减法、乘法、转置、行列式、逆矩阵等
- **算法可视化**: 高斯消元法、LU分解、Gram-Schmidt正交化的步骤演示
- **实时计算**: 即时显示运算结果，支持数值格式化

### 🎨 用户体验
- **现代化UI设计**: 采用渐变色彩和流畅动画
- **响应式布局**: 完美适配桌面端和移动端
- **直观的操作界面**: 清晰的视觉反馈和用户引导
- **中文界面**: 完全本地化的用户界面

## 🏗️ 项目结构

```
src/
├── components/
│   ├── MatrixEditor.vue      # 矩阵编辑器组件
│   ├── MatrixOperations.vue  # 矩阵运算组件
│   ├── AlgorithmVisualizer.vue # 算法可视化组件
│   └── HelloWorld.vue        # 示例组件
├── assets/                   # 静态资源
├── App.vue                   # 主应用组件
├── main.ts                   # 应用入口
├── style.css                 # 全局样式
└── vite-env.d.ts            # TypeScript 类型声明
```

## 🚀 快速开始

### 环境要求
- Node.js >= 16.0.0
- npm >= 7.0.0 或 yarn >= 1.22.0

### 安装依赖
```bash
npm install
# 或
yarn install
```

### 开发模式
```bash
npm run dev
# 或
yarn dev
```

### 构建生产版本
```bash
npm run build
# 或
yarn build
```

### 预览生产版本
```bash
npm run preview
# 或
yarn preview
```

## 🧮 功能详解

### 矩阵编辑器
- **动态维度调整**: 通过直观的控件调整矩阵行列数
- **实时输入验证**: 支持整数和小数输入
- **视觉焦点指示**: 清晰显示当前编辑的单元格
- **矩阵信息显示**: 实时显示矩阵维度信息

### 矩阵运算
#### 基础运算
- **矩阵加法** (A + B): 对应元素相加
- **矩阵减法** (A - B): 对应元素相减
- **矩阵乘法** (A × B): 标准矩阵乘法运算

#### 高级运算
- **矩阵转置** (A^T): 行列互换
- **行列式计算** (det(A)): 使用递归展开计算
- **逆矩阵** (A^(-1)): 基于高斯-约旦消元法
- **标量乘法** (k × A): 矩阵与标量相乘

### 算法可视化
- **高斯消元法**: 逐步演示线性方程组求解过程
- **LU分解**: 展示矩阵分解为下三角和上三角矩阵
- **Gram-Schmidt正交化**: 演示向量正交化过程

## 🛠️ 技术栈

- **前端框架**: Vue 3 (Composition API)
- **开发语言**: TypeScript
- **构建工具**: Vite
- **样式方案**: CSS3 (CSS Variables + Grid/Flexbox)
- **图标库**: Font Awesome 6
- **字体**: Inter + JetBrains Mono

## 🎨 设计特色

- **现代化配色**: 基于渐变色的视觉设计系统
- **流畅动画**: CSS3 动画和过渡效果
- **响应式设计**: 移动端友好的自适应布局
- **无障碍支持**: 符合 Web 无障碍标准

## 📱 浏览器支持

- Chrome >= 88
- Firefox >= 85
- Safari >= 14
- Edge >= 88

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进项目！

### 开发流程
1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

## 🙏 致谢

- Vue.js 团队提供的优秀框架
- Vite 团队提供的快速构建工具
- Font Awesome 提供的图标库
- 所有为开源社区做出贡献的开发者们

---

**让数学变得更有趣！** 🎉
