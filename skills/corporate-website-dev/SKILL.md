---
name: corporate-website-dev
description: 面向从零启动的公司/产品官网和品牌站项目，覆盖需求澄清、信息架构、UI 方向、Vite+Bootstrap+Tailwind 技术栈与交付。仅在用户需要 0→1 建设全新官网并期望结构化调研、设计与实现指引时使用。
---

# 企业官网开发（Corporate Website Dev）

## 使用场景
- 仅当用户需要**从零开始**构建全新的企业/产品官网或品牌站（非局部改版、内容替换、单页更新）。
- 需求必须覆盖完整链路：调研、项目文档、编码实现、部署测试，且包含多页面、UI 风格、官方色彩等品牌要求。
- 启用本技能时必须**同步加载 `ui-ux-pro-max` 技能**，所有 UI 设计方案要遵循该技能提供的设计思路与流程。
- 前端实现强制采用 **Vite + Bootstrap + Tailwind CSS** 组合，不允许更换框架或样式体系；若用户提出不同技术诉求，需要先确认是否仍适用本技能。

## 工作流程（四步）

### 1. 需求调研
- 打开 `references/questionnaire.md`，使用 Codex **提问工具**按顺序提问：
  1. 业务背景（行业/受众/价值主张，自由文本）。
  2. 需要的页面（多选，允许自定义）。
  3. 官方色彩方案（背景色、主色及辅色，自由文本或限定选项）。
  4. UI 风格（多选，可组合）。
- 任一问题若出现“自定义/其他”，继续用提问工具追问细节（颜色数值、页面目标等）。
- 字体仍由用户主动提供时记录，无需主动提问。
- 所有必答项信息完整且无歧义后才能进入下一阶段；否则需要继续提问澄清。

### 2. 编写项目文档
- 将调研结果整理成项目简报，至少包含：
  - **行业/受众/价值主张**：引用用户原话，确定叙事基调。
  - **页面清单与层级**：依据 `references/page-templates.md` 描述各页面区块、所需素材和交叉链接。
  - **官方色彩与视觉策略**：基于问卷得到的配色方案，并对照 `ui-ux-pro-max` 的设计框架，说明如何把背景/主色/辅色与所选 UI 风格融合，落实到组件和交互准则。
  - **字体信息（若用户提供）**：注明授权/替代方案。
  - **成功指标与约束**：性能、转化目标、法务要求等；列出未决问题待确认。
- 将文档发给用户确认，必要时迭代更新；若 UI 方案与 `ui-ux-pro-max` 原则冲突，需回到调研阶段修正。

### 3. 开发编码（固定栈：Vite + Bootstrap + Tailwind）
- 执行以下步骤建立基础代码：
  - `pnpm create vite@latest corp-site --template react-ts`，进入目录后 `pnpm add -D tailwindcss postcss autoprefixer sass` 与 `pnpm add bootstrap @popperjs/core classnames`。
  - 运行 `pnpx tailwindcss init -p`；在 `tailwind.config.cjs` 的 `theme.extend.colors` 注入调研确定的背景色、主色、辅色。
  - 在 `src/styles/theme.scss` 中先覆盖 Bootstrap 变量（如 `$body-bg`, `$primary`, `$border-radius`），再 `@import "bootstrap/scss/bootstrap"`，最后在 `src/styles/tailwind.css` 中依次引入 Tailwind 指令与 `theme.scss`。
  - 建立目录结构：`src/components/`, `src/sections/`, `src/pages/`（或路由入口）、`src/data/`, `src/styles/`, `src/lib/`。
- 实现内容：全局布局、导航/页脚、至少一个页面区块示例（结合 Bootstrap 结构 + Tailwind 微调）、设计令牌（颜色/间距/阴影）、示例数据文件。
- 所有组件和交互方案需对照 `ui-ux-pro-max` 的设计模式（信息密度、留白、动效）。

### 4. 部署测试
- 构建与验证命令：`pnpm lint && pnpm test && pnpm build && pnpm preview`；如有 Playwright，追加 `pnpm test:e2e` 用于烟测导航/CTA/表单。
- 部署：
  - **Vercel**：框架选择 Vite，命令 `pnpm build`，输出 `dist`，配置预览域名。
  - **Netlify**：`build = "pnpm build"`，`publish = "dist"`；如需要表单收集可启用 Netlify Forms。
  - **Cloudflare Pages**：设置 `pnpm install && pnpm build`，框架选择 Vite。
- 验收：运行 Lighthouse（FCP < 1.5s，CLS < 0.1）、axe 无障碍检查、表单回执测试；确认最终界面、色彩、间距符合 `ui-ux-pro-max` 规范。
- 补充监控与运维：记录所需环境变量、API Mock/Stub、Plausible 或 Vercel Analytics 集成、回滚策略。

## 参考资源
- `references/questionnaire.md`：必答问题与提问工具使用说明。
- `references/page-templates.md`：常见页面结构与组件提示。
- `skills/ui-ux-pro-max`（技能）：必须同步加载，UI 设计方案需遵循其中的设计思路与流程。
