# 更新日志

## 2026-04-28 — CSS 布局修复 + 内容重组

### 一、CSS 布局修复（Phase 1）

**问题**：Bootstrap 3 float 布局下，不等高卡片产生行间空隙（"狗洞"），卡片无法密铺。

**根因**：三层框架干扰叠加：
1. `.row` 的 clearfix 伪元素（`::before`/`::after` 带 `display: table`）被当作 flex 子项占位
2. 内层 `.row` 负 margin（`margin: 0 -10px`）导致卡片背景视觉重叠
3. 父级 `.mainContent` / `.linkList-item` 的 padding 挤占 flex 可用宽度

**修复**（`css/style.css`）：
- 清除 clearfix 伪元素：`.linkList-item .row.dh::before, .linkList-item .row.dh::after { display: none }`
- 清零内层 row 负 margin：`.linkList-item .row.dh > .col-sm-6 > .row { margin-left: 0; margin-right: 0 }`
- 覆盖父级 padding：`.mainContent, .linkList-item { padding-left: 0 !important; padding-right: 0 !important }`
- 切换为 flex 布局，`column-gap: 10px; row-gap: 10px`，三断点响应式（3/2/1 列）

**涉及文件**：`css/style.css`（重写布局段）、`css/media.css`（精简）

### 二、内容重组（Phase 2）

**分类结构调整**：

| 卡片 | 项目数 | 说明 |
|---|---|---|
| 浙大·教学 | 12 | 未改动 |
| AI·对话 | 12 | 拆分自原 AI 卡，ChatGPT / Claude / Gemini / Grok / DeepSeek / Kimi / 豆包 / 通义千问 / 智谱清言 / 文心一言 |
| AI·工具 | 18 | 拆分自原 AI 卡，Cursor / Copilot / Suno / Perplexity / HuggingFace / Claude Code 等 |
| 浙大·信息化服务 | 12 | 未改动 |
| 浙大·其它 | 12 | 原 3 个"空"替换为班车查询 / 校园地图 / 流量监控 |
| 学术搜索·查重 | 12 | 合并去重，排序调整为浙大→国内→国外 |
| 学习·影视 | 15 | 删除蜜柑计划，新增 MIT OCW，补齐空位 |
| 实用工具 | 18 | 合并原两个重复卡片，新增聚合导航站（小森林导航 / 阿虚储物间） |
| 社交·编程 | 15 | 合并去重，新增 X (x.com)，Reddit 移至知乎前 |
| 其它·关于本站 | 9 | 未改动 |

**其他变更**：
- 所有卡片项目数统一为 3 的整数倍（视觉整齐）
- 空位统一用 "—" 标识，`href="#"` + `title="自定义"`
- Perplexity 从 AI·对话移至 AI·工具，标注"需要梯子"
- 移除抖音、爱奇艺等冗余娱乐链接
- 百度统计、51.la 脚本已注释

### 三、Git 记录

```
ee93cb0 refactor: 拆分AI为对话/工具两卡，整理分类顺序，X归入社交，补空位和聚合站
2983632 fix: 修复卡片布局 — 清除 clearfix 伪元素、内层 row 负margin、父级 padding 干扰
da829c8 backup: 布局迭代中的当前状态 — flex + column-gap + calc()，内层 row 负margin 已修复
```
