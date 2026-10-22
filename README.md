# 激子与激发态的多体理论 —— 从二次量子化到 GW-BSE 与 TDDFT

> 一份中文讲义：从二次量子化出发，经激子模型与 Wannier 方程、GW 近似与 Bethe–Salpeter 方程，一路推到 RT-TDDFT / LR-TDDFT 与 OT-SRSH 泛函，共 477 个连续编号公式。

正文见 **[exciton-tddft.md](exciton-tddft.md)**（GitHub 可直接渲染，含 LaTeX 公式）。

> **无法访问 GitHub？**
> 本文档在 **[allergicrosen.xyz/p/excition_re](https://allergicrosen.xyz/p/excition_re/)** 有镜像页面，国内网络可直接打开。
> 注意：**镜像站的更新会滞后**，本仓库里的内容才是最新的；需要最新版本或想提 Issue 请以 GitHub 为准。

这份讲义想解决的是一个具体的困难：激子、准粒子、激发态这几套语言在文献里分散于凝聚态多体理论、量子化学和含时密度泛函三个传统，符号约定各不相同。初学者往往能在各自的框架内推导，却接不上"同一个物理量在另一套语言里长什么样"。本文按一条主线串起来——先把多体哈密顿量用二次量子化写清楚，再由它导出激子模型与 Wannier 方程，然后分别走向 GW-BSE（多体微扰）和 RT/LR-TDDFT（含时密度泛函），最后落到实际可用的 OT-SRSH 泛函。

## 内容概览

| 章节 | 主要内容 |
| --- | --- |
| 什么是激子 | 激子的定义、电子-空穴库仑束缚、自陷激子态（STE） |
| 单粒子表象 | 平均场近似的适用性，以及它在处理关联作用与粒子产生/湮灭时的失效 |
| 多粒子表象 | 一次量子化 → 二次量子化 → 产生湮灭算符与对易关系 → 场算符及其性质 |
| 激子模型 | 相互作用的二次量子化、双带近似、双电子到电子-空穴对、激子哈密顿量与激子态、**Wannier 方程** |
| GW + BSE | **GW 近似**（准粒子能带与自能）、**Bethe–Salpeter 方程**（激子束缚与光吸收谱） |
| 含时密度矩阵形式 | 场算符展开、密度矩阵的时间演化方程 |
| RT-TDDFT 与 LR-TDDFT | 实时传播与线性响应两条路线的等价性及各自适用场景 |
| OT-SRSH 泛函 | 最优调控范围分离杂化泛函，含微扰论一阶修正的实用做法 |

## 按目的阅读

- **想从零建立多体语言**：§多粒子表象 → §激子模型，把二次量子化与电子-空穴对表示打牢。
- **要做激子光谱计算**：§GW + BSE，重点是自能与 BSE 核的构造；Wannier 方程部分给出紧束缚图像下的对照。
- **已熟悉 TDDFT，想搞清两条路线的关系**：§含时密度矩阵形式 → §RT-TDDFT 与 LR-TDDFT。
- **关心泛函选择**：直接看 §OT-SRSH，以及文中关于"PBE 波函数形状已经足够好、错的只是本征值"的讨论。

## 仓库结构

```
.
├── README.md         # 本文件
├── exciton-tddft.md  # 讲义正文
├── LICENSE           # CC BY 4.0 许可全文
└── .gitignore        # 排除编辑器配置与本地旧版本
```

## 阅读方式

- **在线**：直接点开 [exciton-tddft.md](exciton-tddft.md)。GitHub 支持 LaTeX 公式渲染，但本文公式量大（约 270 个公式块），复杂环境偶有排版偏差。
- **本地（推荐，公式排版最稳）**：用支持 MathJax/KaTeX 的编辑器打开，例如 VS Code + Markdown Preview Enhanced、Typora、Obsidian。

```bash
git clone https://github.com/wkpahuwkp98/gw-bse-wannier-lrtddft.git
```

## 说明

- 公式编号在全文连续（至 (477)），便于交叉引用与回查。
- 文末附 **[延伸阅读与原始文献](exciton-tddft.md#延伸阅读与原始文献)**，按主题分组：多体方法、激子理论、Wannier 方程、GW 近似、Bethe–Salpeter 方程、TDDFT、OT-SRSH。这部分由整理者补充，用于回溯方法的原始出处，**并非正文的行内引用**——正文未做文献标注。
- 讲义为学习整理性质，公式推导与文献信息如与原始出版物不符，以原始文献为准；欢迎提 Issue 指正。

## 相关仓库

- **[cp2k-nac-fssh](https://github.com/wkpahuwkp98/cp2k-nac-fssh)** —— 非绝热载流子动力学的中文教程：从 Tully 的 FSSH 推导到 PYXAID / CP2K 实操，含 CP2K 高斯基 NAC 计算脚本。本文讲的是"激发态怎么描述"，那边讲的是"激发态怎么动"，可对照阅读。

## 许可

本文档采用 **[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.zh-hans)**（署名 4.0 国际）协议授权，全文见 [LICENSE](LICENSE)。

© 2026 wkpahuwkp98

你可以自由地**共享**（以任何媒介或格式复制、发行）与**演绎**（修改、转换、再创作），包括商业用途，但须遵守：

- **署名**：注明作者 `wkpahuwkp98`、给出本仓库链接，并标明是否作了修改
- **不得增加限制**：不得对他人在法律上施加额外限制

建议引用格式：

> wkpahuwkp98. 激子与激发态的多体理论——从二次量子化到 GW-BSE 与 TDDFT[EB/OL]. GitHub, 2026. https://github.com/wkpahuwkp98/gw-bse-wannier-lrtddft
