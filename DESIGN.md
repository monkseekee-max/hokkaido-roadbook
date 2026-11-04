# DESIGN.md — 北海道 · 七昼夜

单文件路书页面。视觉与内容决策的记录，改代码前先读这份。

- 交付物：`index.html`（HTML + CSS + JS 全内联，零依赖，无构建）
- 产品上下文：`PRODUCT.md`
- 方向记录：`.impeccable/questions/direction.json`（concept seed `469af72e`）

## 1. 核心概念：北海道駅弁 / 幕の内重箱

页面不是垂直时间轴，是一只被打开的便当盒。

| 页面区块 | 便当语言 |
| --- | --- |
| Hero | 朱漆箱盖（ lid ）+ 打开的方形重箱（ 8 格 = 7 天 + 清单） |
| 总览 | 盒盖里的「品写」（菜单纸） |
| 路线 | 一条从机场绕回机场的线路图 |
| 每天 | 一盒，四格：朝 · 昼 · 夕 · 宿 |
| 城际交通 | 盒底的黑色食切（隔条） |
| 备注 / 备份 | 虚线框的「备份方案」小卡 |
| 结尾 | 盒子盖好 |

隐喻必须服务阅读：格子的颜色 = 一天里的时段，黑条 = 这天的交通，虚线框 = 可展开的备份方案。

## 2. 色板

朱漆、米饭白、海苔黑、玉子黄、樱粉、若草绿。所有色值在 `:root`。

| 变量 | 值 | 用途 |
| --- | --- | --- |
| `--lac` `#c8323c` | 朱 | 页面底、贴纸、强调 |
| `--lac-deep` `#a71f2b` | 朱深 | 正文可用的红字、warn tag 底 |
| `--lac-dark` `#8d1620` | 朱暗 | 渐变收边 |
| `--rice` `#fbf7ee` / `--milk` `#fffdf8` | 米饭 | 纸面、正文底 |
| `--ink` `#1d1a19` / `--nori` `#241d1b` | 海苔 | 正文、深色带 |
| `--sakura` `#f2a89a` | 樱 | JR 线段、昼格条纹 |
| `--sakura-deep` `#c05a3f` | 樱深 | 昼格条纹与针脚（正文底上 ≥3:1） |
| `--wakame` `#6f9350` | 若草 | 巴士线段 |
| `--wakame-deep` `#3f5a2b` | 若草深 | 宿格条纹、tag 底（米白上 7.25:1） |
| `--tamago` `#f2c94c` | 玉子 | 枢纽、深色带上的强调 |
| `--tamago-deep` `#a97d12` | 玉子深 | 朝格条纹与针脚 |
| `--wood` `#c9a06a` | 木箱 | 重箱外框 |

### 对比度规则（已按 WCAG 2.2 计算）

- 正文一律 `--ink` 于 `--rice` / `--milk` / 四色浅底：15.2–17.0:1。
- 深色带上正文用 `--rice`（于 `--nori` 15.5:1），次要文字 `--rice-70`。
- 红字用于正文时用 `--lac-deep`（4.94:1 于米白）；`--lac` 只用于大字标题、条纹、贴纸。
- 浅底上的小色块（针脚、条纹）用 deep 变体，保证 ≥3:1 非文本对比。
- 禁止：`--sakura-deep` 作为米白上的正文；`--lac` 作为玉子黄上的正文。

## 3. 时段四色（一天一盒的身份）

由 `.tray` 的位置驱动，不写 inline style：

```css
.tray .pane:nth-child(1){--pane:var(--tamago-deep);--pane-bg:#fdf7e6} /* 朝 */
.tray .pane:nth-child(2){--pane:var(--sakura-deep);--pane-bg:#fcefed} /* 昼 */
.tray .pane:nth-child(3){--pane:var(--lac);--pane-bg:#fbeeea}         /* 夕 */
.tray .pane:nth-child(4){--pane:var(--wakame-deep);--pane-bg:#edf2e6} /* 宿 */
.pane.is-check{--pane:var(--ink);--pane-bg:#f6f1e6}                    /* DAY 07 清单格 */
```

`--pane` = 顶部 4px 条纹 + 列表针脚；`--pane-bg` = 格子底色。新增格子只改 class，不加 style。

## 4. 字体

| 角色 | 变量 | 字体 |
| --- | --- | --- |
| 展示 / 数字 | `--f-display` | ZCOOL KuaiLe（可爱、手写感）→ M PLUS Rounded 1c → 系统中文 |
| 正文 | `--f-body` | Noto Sans SC → M PLUS Rounded 1c → PingFang / Yu Gothic |
| 日文 | `--f-ja` | M PLUS Rounded 1c → Noto Sans SC → Yu Gothic |

- 通过 Google Fonts `<link>` 加载（子集化，CJK 按需分片）。
- 日文文本必须 `<span class="ja" lang="ja">`：字体正确 + 读法正确。
- 数字用 `.num`（展示字体）保持路书感。

## 5. 图标

- 全部内联 SVG，`<symbol>` 定义在文件顶部，`<use href="#i-xxx"/>` 引用。
- 不用 emoji：渲染不可控、不可控色、不可控对比。
- 装饰性图标一律 `aria-hidden="true"`。

## 6. 组件

- `.cell` 重箱格：`<a>` 包裹，整格可点，跳进当天。格底 `.fill` 细条 = 这天的排得有多满（hero 里有图例）。
- `.day-no` 贴纸：微旋转，DAY + 数字。
- `.leg` 黑色食切条：当天城际交通、耗时、参考票价。
- `details.fold`：备注与备份方案，默认收起；打印前 JS 全展开，打印后恢复。
- `.pack` 清单：原生 `input[type=checkbox]`（opacity 0 但保留可聚焦），`:focus-visible` 在自绘方框上画 outline。
- `.hashi` 筷子进度轨：滚动超过 320px 出现。

## 7. 交互（约 90 行 JS，渐进增强）

| 功能 | 机制 |
| --- | --- |
| 读到哪一天 | `IntersectionObserver`（rootMargin −45%）→ `localStorage["hokkaido.roadbook.lastDay"]` → 折角胶带旗 + 跳转按钮 |
| 筷子进度 | `scroll` + `requestAnimationFrame`，`transform: scaleX()`（不动 width，不触发 layout） |
| 装备清单 | `localStorage["hokkaido.roadbook.pack"]` 存 data-k 列表；进度条同样用 `scaleX()` |
| 复制清单 | `navigator.clipboard` + `execCommand` 回退；按钮文案变「已复制」后复原 |
| 打印 / 存 PDF | `beforeprint` 展开所有 fold，`afterprint` 复原；`prefers-reduced-motion` 时跳过动画直接打印 |

禁用 JS 时：所有内容仍完整可读，fold 用 `details` 原生语义，链接、清单（未勾选态）都能用。

## 8. 可访问性

- 键盘：skip link → 重箱格 → fold summary → 清单 → 按钮，顺序即视觉顺序。
- 焦点：统一 `:focus-visible` outline，深色带用玉子黄，浅色带用朱。
- 对比：正文 ≥4.5:1，非文本图形 ≥3:1（见 §2）。
- 动效：`prefers-reduced-motion` 关闭所有 transition/animation 与 smooth scroll。
- 语义：`role="list"` 线路图、`aria-hidden` 装饰、`lang="ja"` 日文、checkbox 有可见 label。
- 触控：格子和按钮的最小高度 ≥44px。

## 9. 响应式

- `<640` 单列；`≥640` 重箱 4 列、便当格 2 列；`≥1000` hero 双栏、清单 3 列、线路图等宽不滚动。
- 线路图在窄屏横向滚动，文案里写明「可以左右拖动」。

## 10. 打印

`@media print`：白底黑字、去阴影、隐藏进度轨与按钮、`.leg` 转成描边框、fold 展开、避免天与天之间断页。

## 11. 内容规则（不可妥协）

- 不伪造真实酒店、实时价格、库存、营业状态。所有票价 / 时刻 / 气温 / 预算都是**规划参考值**，页面里明写。
- 需要出发前复核的信息打 `待核实` / `先拿号` / `强风停运` 等 tag。
- 简体中文为主，保留日文地名。
- 正文不用 em dash（当前全文 2 处，均在非正文语境）。

## 12. 已知限制

- 字体依赖 Google Fonts；离线时回退系统圆体 / 黑体，版式不塌。
- `impeccable detect` 在本机缺 `htmlparser2 / css-select / css-tree / domutils`，只能正则扫描（结果偏低）；已人工补做对比度计算、标签闭合校验、1440 / 390 双端截图检查。

## 13. 改动指引

1. 改行程：直接编辑 `index.html` 的 `<article class="day" id="day-N">`，四格顺序保持 朝→昼→夕→宿（颜色按位置走）。
2. 加一天：复制一个 `.day`，改 `id` / `data-day` / 贴纸数字，并在 hero `.cells` 加一格。
3. 加颜色：先进 `:root`，再算对比度，再写进本文件 §2。
4. 不要加 inline style 表达语义；用 class。
