# Bug 修复日志

## Turbopack 构建失败：Noto Sans SC 字体模块解析错误

### 问题描述

在 `my-feature` 分支上执行 `git push` 时，`pre-push` 钩子触发 `pnpm run build`，构建过程中报错：

```
Build error occurred
Error: Turbopack build failed with 1 errors:
[next]/internal/font/google/noto_sans_sc_5c6bb846.module.css:375:9
Module not found: Can't resolve '@vercel/turbopack-next/internal/font/google/font'
```

构建命令执行失败，`git push` 被 husky pre-push 钩子阻断。

### 错误调用链

```
src/app/layout.tsx
  └── next/font/google → Noto_Sans_SC()
        └── [next]/internal/font/google/noto_sans_sc_5c6bb846.js
              └── [next]/internal/font/google/noto_sans_sc_5c6bb846.module.css
                    └── ❌ Can't resolve '@vercel/turbopack-next/internal/font/google/font'
```

### 根因分析

1. **Next.js 16 默认使用 Turbopack 作为构建打包器**：项目使用 Next.js 16.x，`next build` 命令默认走 Turbopack 而非 webpack。

2. **Turbopack 对 CJK 大字体的兼容性问题**：`Noto Sans SC`（思源黑体简中版）是一个超大型 CJK 字体，Google Fonts 为其生成了数百个 `@font-face` 规则（每个覆盖不同的 unicode-range）。Turbopack 在处理这些大量自动生成的 CSS 模块时，无法正确解析内部模块 `@vercel/turbopack-next/internal/font/google/font`，导致构建失败。

3. **webpack 不受此影响**：webpack 打包器对 `next/font/google` 的内部 CSS 模块解析路径有完善的支持，不存在此兼容性问题。

### 解决方案

**修改文件**：`package.json`

**修改内容**：在 `build` 脚本中为 `next build` 添加 `--webpack` 标志，强制使用 webpack 打包器进行生产构建。

```diff
- "build": "pnpm run lint:fix && next build",
+ "build": "pnpm run lint:fix && next build --webpack",
```

### 为什么选择这个方案

| 备选方案                            | 评估                                                                                                  |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **使用 `--webpack` 构建（已采用）** | 最小改动，一行修改，直接绕过 Turbopack 的 CJK 字体 bug，不影响开发体验（`next dev` 仍可用 Turbopack） |
| 使用 `next/font/local` 替代         | 需要手动下载字体文件并维护，改动量大，增加仓库体积                                                    |
| 通过 CSS `@import` 从 CDN 加载字体  | 失去 `next/font` 的自动优化（自托管、`font-display`、预加载等），性能下降                             |
| 等待 Turbopack 修复                 | 不可控，且当前需要立即修复 push 阻塞问题                                                              |

### 影响范围

- **生产构建**：使用 webpack 打包，稳定可靠
- **开发服务器**：`next dev` 仍默认使用 Turbopack，开发体验不受影响
- **pre-push 钩子**：构建可以正常通过，push 不再被阻断
- **字体加载**：无变化，`next/font/google` 的三个字体（Inter、Noto Sans SC、JetBrains Mono）均正常工作

### 修复日期

2026-04-10
