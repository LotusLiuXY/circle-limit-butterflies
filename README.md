# 圆极限分形蝴蝶 · Circle Limit with Butterflies

基于 M.C. 埃舍尔《圆极限·蝴蝶》（*Circle Limit with Butterflies*, 1958）版画与双曲几何「圆极限」构图原理，用代码生成的分形蝴蝶可视化。蝴蝶沿**庞加莱圆盘**内的双曲测地线对称排布，越靠近边界越小越密，呼应埃舍尔原作的无限循环韵律。

> 启用 GitHub Pages 后，可在 **https://LotusLiuXY.github.io/circle-limit-butterflies/** 直接预览 V2。

## 文件清单

| 文件 | 说明 |
|------|------|
| `circle-limit-butterflies-v2.html` | **本版（V2）**：真正的庞加莱圆盘 `{p,q}` 反射群镶嵌，可交互可调参 |
| `circle-limit-butterflies.html` | V1：近似环带排布的上一版备份 |
| `config-default.json` | 默认参数快照（单一 `config` 对象，可复用于二次开发） |
| `index.html` | 重定向到 V2 的入口页 |
| `README.md` | 本说明书 |

## 快速开始

直接用浏览器打开 `circle-limit-butterflies-v2.html` 即可。无需服务器、无外部依赖。所有参数经右侧控制面板实时调节，改动后画布即时重绘。

## 参数体系（集中于单一 `config` 对象）

命名规则统一：`wing.nodes[i].x/y/tangent`、`wing.angle`、`wing.scale`、`color.stops[]` 等。

### 1. tiling — 双曲镶嵌（圆极限）
| 参数 | 默认 | 说明 |
|------|------|------|
| `symmetryP` | 6 | `{p,q}` 旋转对称阶 |
| `symmetryQ` | 4 | `{p,q}` 每顶点面数 |
| `depth` | 5 | 反射群递归深度（镶嵌层数） |
| `rotate` | 0 | 镶嵌整体旋转（度） |
| `shrink` | 0.72 | 双曲共形收缩强度 `(1−|z|²)` |
| `alternate` | true | 埃舍尔式明暗 / 手性交替 |
| `alternateShade` | 28 | 交替面明暗差 |
| `inset` | 0.0 | 圆边缘留白比例 |
| `geodesic` | — | 测地线网格：`show` / `width` / `opacity` / `color` |
| `face` | — | 面填色：`show` / `alternateFill` / `colors[]` |

### 2. scene — 全局
| 参数 | 默认 | 说明 |
|------|------|------|
| `count` | 200 | 蝴蝶数量（受不重叠与最小尺寸约束） |
| `boundaryRadius` | 0.97 | 圆极限边界（曲率半径映射） |
| `bgColor` | `#efe8d8` | 圆盘底色 |
| `colorMode` | `byRing` | 上色方式：`byRing` / `byAngle` / `mixed` / `mono` |
| `hueSpread` | 120 | 色相散布 |
| `seed` | 20260828 | 随机种子 |

### 3. butterfly — 运动与尺寸
| 参数 | 默认 | 说明 |
|------|------|------|
| `sizePct` | 0.038 | 蝴蝶大小（屏幕宽 0%–100%，UI 以百分比显示，自由设定任意大小） |
| `minPx` | 34 | 最小像素（0–100，设 0 即不保底；与 `sizePct=0` 配合可实现从零开始） |
| `opacity` | 0.97 | 不透明度（0%–100%，可为全透明） |
| `speed` | 0.5 | 飞行速度 |
| `loopDuration` | 20 | 循环动画时长（秒） |
| `displacement` | 5 | 位移范围（px） |
| `flapSpeed` | 1.4 | 振翅频率 |
| `spreadAngle` | 5 | 翅膀展开角度（度） |
| `mirror` | false | 镜像翻转 |
| `layerOrder` | `smallTop` | 层叠顺序：`smallTop` / `bigTop` / `byRing` |
| `spawnRate` | 0.9 | 出现频率 / 可见比例（0%–100%，可为 0） |
| `orient` | `tangent` | 朝向：`tangent` / `radial` / `mixed` |

### 4. wing — 翅膀参数
| 参数 | 默认 | 说明 |
|------|------|------|
| `angle` | −7 | 翅膀基准角（度） |
| `scale` | 1.0 | 缩放 |
| `textureDensity` | 0.42 | 纹理密度 |
| `veins` | — | 脉络：`show` / `width` |
| `light` | — | 光影：`angle` / `intensity` |
| `strokeWidth` | 1.1 | 边缘描边宽度 |
| `butterfly` | `classic` | 蝴蝶形态：`classic` / `swallow` / `round` / `broad` / `slender` / `angular` |
| `nodes` | 18 | 轮廓控制节点（前翅 8 / 后翅 10），每项 `{x, y, tangent}` |

`nodes` 共 18 个：前翅 8 节点（x 0.045→0.508，y −0.118→0.036），后翅 10 节点（x 0.008→0.402，y 0.020→0.432），详见 `config-default.json`。可在节点编辑器中选中、拖拽，逐点调 `x/y/tangent`，经 Catmull-Rom→贝塞尔平滑过渡。

### 蝴蝶形态（6 种可选）

通过「翅膀」面板顶部的**蝴蝶形态**下拉切换；每种形态均为 18 个轮廓节点（前翅 8 + 后翅 10），无眼睛、头大身小：

| 形态 key | 名称 | 特征 |
|----------|------|------|
| `classic` | 标准圆润 | 原版默认，圆润饱满 |
| `swallow` | 燕尾尖翅 | 前翅尖长、后翅带飘逸尾突 |
| `round` | 圆翼 | 更圆钝、收敛的翼形 |
| `broad` | 宽翼 | 翼面更宽大丰满 |
| `slender` | 纤翅 | 纤细秀气、整体收窄 |
| `angular` | 三角翼 | 棱角分明、几何感强 |

切换形态会替换 `wing.nodes`；在节点编辑器中继续微调后，点「重置轮廓」可恢复**当前形态**的初始节点（而非回到标准形）。

### 5. color — 配色
| 参数 | 默认 | 说明 |
|------|------|------|
| `stops` | 5 段 | 多段渐变：每项 `{t, c, a}`（位置 / 颜色 / 透明度），支持 ≥4 色标 |
| `backHueShift` | 12 | 反翅色相偏移（正反面色差） |
| `backAlpha` | −0.05 | 反翅透明度差 |
| `backLight` | −10 | 反翅明度差 |
| `strokeColor` | `#231f16` | 描边色 |

## 交互操作

- **参数面板**：右侧滑块 / 取色器 / 下拉实时调参。
- **轮廓编辑器**：在节点编辑视图中点击节点选中，拖动改坐标；面板中精确输入 `x/y/tangent`。
- **键盘**：`空格` 暂停 / 继续 · `R` 重排镶嵌 · `E` 导出 PNG。
- **导出**：`导出 PNG` 按钮保存当前画面；`复制参数` 导出 `config` JSON。
- **预设**：面板顶部提供 **14 套配色预设**，一键切换并重建 —— 埃舍尔木刻 / 青瓷 / 暮色莫兰迪 / 赤金 / 水墨 / 霓虹 / 樱粉 / 敦煌 / 极光 / 普鲁士蓝 / 落日 / 苔绿 / 紫晶 / 景泰蓝。
- **蝴蝶形态**：「翅膀」面板顶部「蝴蝶形态」下拉，6 种翼形任选（见上节）。

## 约束与默认

- **无眼睛**：已移除全部眼睛相关参数与元素。
- **形态**：保留头大身小、姿态正确的标准蝴蝶形态；触角保留。
- **顶层绘制**：蝴蝶始终置于顶层 `z-index`，避免被镶嵌面遮挡。
- **尺寸保证**：蝴蝶大小滑块范围 0%–100%（默认 3.8%，下限 0 可设），实际尺寸按所在镶嵌面容量自适应（不与其他元素重叠）；超出面容量时自动跳过放置。
- **飞行自然**：位移范围、速度、透明度、出现频率、循环动画时长已协同调校，显示稳定。

## 许可与署名

本可视化为对埃舍尔原作《圆极限·蝴蝶》的算法致敬与再创作，仅供学习与艺术研究使用。原版画 © M.C. Escher / The M.C. Escher Company。
