# my-feature 分支改动详情

## 概述

本次改动在 `my-feature` 分支上完成了三项主要定制化修改：更换博客主题配色为高级蓝色、修改博客名称、以及调整导航栏按钮功能。

---

## 1. 主题配色 - 高级蓝色主题

### 改动文件

- `src/styles/theme.css`

### 改动详情

将原有的樱花粉（Sakura Pink）主题替换为高级蓝色主题，涉及三组颜色：

| 颜色组 | 修改前 | 修改后 |
|--------|--------|--------|
| **Primary** | Carnation Pink (樱花粉 `#ff9fb2`) | Royal Blue (宝蓝 `#60a5fa`) |
| **Secondary** | Spray Sora (天空蓝 `#91c7e0`) | Indigo (靛蓝 `#a5b4fc`) |
| **Accent** | Turquoise Blue (翡翠绿 `#81e6d9`) | Cyan (青蓝 `#67e8f9`) |

### 背景色调整

| 模式 | 属性 | 修改前 | 修改后 |
|------|------|--------|--------|
| **亮色模式** | background | `#fefefe` | `#f8fafc` |
| **亮色模式** | foreground | `#2d3748` | `#1e293b` |
| **亮色模式** | gray-light | `#f9fafb` | `#f1f5f9` |
| **亮色模式** | gray-dark | `#1f2937` | `#334155` |
| **暗色模式** | background | `#161b22` | `#0f172a` |
| **暗色模式** | gray-light | `#1f2937` | `#1e293b` |
| **暗色模式** | gray-dark | `#d1d5db` | `#cbd5e1` |

---

## 2. 博客名称修改

### 改动文件

- `config.yml`

### 改动详情

| 配置项 | 修改前 | 修改后 |
|--------|--------|--------|
| `title` | `Suzu` | `酱油的博客` |
| `subTitle` | `Next.js Blog Template` | `记录生活与技术的个人空间` |
| `description` | Suzu is a minimalist blog template... | 酱油的博客 - 一个以高级蓝色为主题的极简博客... |
| `keywords` | Suzu, Next.js, markdown blog... | 酱油的博客, Next.js, markdown blog... |

---

## 3. "开往"按钮改为"联系我"

### 改动文件

- `src/components/common/HeaderMenu.tsx`
- `src/services/config/locales/zh.ts`
- `src/services/config/locales/en.ts`
- `src/services/config/locales/ja.ts`
- `config.yml`

### 改动详情

#### 图标更换

- **修改前**: `TrainFront`（火车图标，来自 lucide-react）
- **修改后**: `Mail`（邮件图标，来自 lucide-react）

#### 跳转链接更换

- **修改前**: `https://www.travellings.cn/go.html`（开往 - 外部链接，新标签页打开）
- **修改后**: `/about#联系我`（站内关于页的"联系我"锚点，当前页面跳转）

#### 按钮启用

- `config.yml` 中 `travellings` 从 `false` 改为 `true`，确保按钮可见

#### 多语言文案更新

| 语言 | 修改前 | 修改后 |
|------|--------|--------|
| 中文 (zh) | 开往 | 联系我 |
| 英文 (en) | travellings | Contact Me |
| 日文 (ja) | 開通の道 | お問い合わせ |

---

## 受影响的功能

- **首页/全站**: 博客标题、描述、关键词 SEO 信息
- **Footer**: 版权信息中的站点名称 (自动读取 config.title)
- **全站 UI**: 所有使用 primary/secondary/accent 颜色的元素
- **导航栏**: 原"开往"按钮变为"联系我"，点击跳转至关于页联系信息区域
- **滚动条**: 颜色随 primary 色值变更
- **暗色/亮色模式**: 背景色和文字色均已适配蓝色主题
