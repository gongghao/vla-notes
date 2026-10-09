# VLA Notes

面向 GitHub Pages 的静态学习站点。当前包含六个技术专题的主页，以及 OpenVLA、RT-1、RT-2、ACT 四篇精读页。

本项目当前以论文阅读、架构理解和实验证据整理为主，源码复现暂缓。

## 本地预览

在本目录执行：

```sh
python3 -m http.server 8765 --bind 127.0.0.1 --directory dist
```

访问 `http://127.0.0.1:8765/`。无需安装前端依赖或构建。字体可从 Google Fonts 加载，不可用时回退到系统字体。

## 文件结构

```text
dist/
  index.html                       专题与论文入口
  papers/openvla/index.html         OpenVLA 精读内容
  papers/rt1/index.html             RT-1 精读内容（八个模块）
  papers/rt2/index.html             RT-2 精读内容
  papers/act/index.html             ACT 精读内容（八个模块）
  assets/style.css                 共享样式、移动端布局
  assets/app.js                    目录、图像放大、流程切换、编码示例
  assets/openvla-architecture.svg   可编辑架构重绘
  assets/openvla-ablation.svg       从 CSV 生成的独立科学图表
  data/openvla-ablation.csv         论文 Table 9 数值及来源
  assets/rt1-architecture.svg       RT-1 架构与闭环控制重绘
  assets/rt1-results.svg            RT-1 四类评测图
  assets/rt1-data-ablation.svg      数据规模与任务多样性图
  data/rt1-*.csv                    RT-1 Tables 2、7、13 数值及来源
  assets/act-*.svg                  ACT 架构、时序和实验图
  data/act-*.csv                    ACT 主实验、完整子阶段与消融数据
```

## 增加论文

1. 以工作区的《VLA论文精读记录模板》组织内容；复制 `dist/papers/openvla/index.html` 到新的 `dist/papers/<slug>/index.html`。
2. 保留共用导航、样式和交互结构，更新标题、元数据、正文、来源与目录锚点。按论文选择重点模块；移除不适用的 OpenVLA 专属滑块、公式和表格。
3. 图存入 `assets/`，图表数值存入 `data/`。同时更新图的可访问描述与数据表，确保数值一致。
4. 在主页对应专题中，将论文名称改成链接；同步页面数量和侧栏入口。跨专题仍链接到同一篇主页面。
5. 本人阅读状态与整理状态分开；未运行实验不得记录为复现成功。数据图必须标明来源与实验协议。

第一版使用共享 CSS/JS 与独立 HTML，适合先确定阅读体验。论文数量增加后，可将导航和模板抽取到静态生成器，保留 URL、图表和内容分类。

## GitHub Pages

仓库：[gongghao/vla-notes](https://github.com/gongghao/vla-notes)。网站地址：[VLA Notes](https://gongghao.github.io/vla-notes/)。页面全部使用相对资源链接，同时兼容用户名站点和项目子路径。

网站通过 `vla-site-source.zip` 发布。远程工作流只读取仓库，解压文件包中的 `dist/` 后发布；源文件包同时包含图表脚本和本文档。本地完整源文件保留在本目录。

更新网站时，将 `dist/`、图表脚本、说明与部署配置重新打包为 `vla-site-source.zip`，替换远程同名文件。主分支源码包或部署配置变化会触发发布，也可在 Actions 手动运行。工作流保持 `contents: read`，只解包并发布站点，不向仓库写入内容。

当前维护项目以论文阅读为主，模型训练与源码复现暂缓。云端工作区可从源码包恢复完整 HTML、CSS、JavaScript、CSV 与绘图脚本。

## 内容版本

### OpenVLA

- 论文：arXiv:2406.09246v3（2024-09-05）。
- 代码：官方仓库 main，2026-10-08 检索；尚未固定 commit。
- 架构图：理解重绘，不是论文原图。
- 消融图：作者报告值，不是本地模型评测。
- 动作编码交互：复现源码的数值规则，基础词表大小示例取 32000。

### RT-1

- 论文：arXiv:2212.06817v2（2023-08-11；首次发表于 2022 年）。
- 代码：google-research/robotics_transformer，固定提交 `4569641b8111f3f402c32d8e24becd2a6e952ecc`。
- 内容包括 FiLM、TokenLearner、动作量化与损失、闭环延迟、实验与数据消融、OpenVLA 对照、代码走读与复现边界。
- 实验图使用论文报告值，CSV 保留表号与来源；原表未提供的不确定度不自行补绘。
- 动作编码交互使用公开源码的截断、255 倍缩放与向下取整规则；演示范围为 −1 到 1。
- 区分默认策略与动作自回归消融配置；注明正文及附录的任务统计口径差异。尚未运行模型或机器人实验。

### RT-2

- 论文：arXiv:2307.15818v1（2023-07-28），阅读与整理日期 2026-10-09。
- 八个模块覆盖架构、动作 token、联合微调、泛化评测、规模消融、语义能力/CoT、局限与跨论文联系。
- 数据固定使用 Tables 6、7、8；未见汇总 62/63 的表间差异分别保留。
- 主机器人实验与 Language-Table、默认策略与 CoT 微调变体分开；不补造控制规格。
- `scripts/render_rt2_figures.py` 从 CSV 生成三个 SVG 图表及一张架构理解重绘。
- 新内容不包含源码复现任务。

### ACT

- 论文：arXiv:2304.13705v1（2023-04-23，RSS 2023）；整理日期 2026-10-09。
- 八个模块：研究问题、架构、CVAE、动作分块、时序集成、主实验、消融、局限与联系。
- 两张机制理解重绘、三张实验图、四份 CSV（包括 Tables I–II 完整子阶段数据）。
- 时序权重交互是教学演算；明确原文对旧候选的权重次序，不代表策略运行。
- PDF 与 HTML 图号、正文 L1 与 Algorithm 1 的 MSE、论文 k=100 与项目视频 k=90 分开说明。
- 真实任务（1 种子 × 25 评测）、仿真任务（3 种子 × 50 评测）与人类遥操作用户研究分别解读；源码复现暂缓。
- `scripts/render_act_figures.py` 从 CSV 生成独立 SVG 图表，并重绘架构与时序机制图。

## 验证

用浏览器检查主页到论文页、章节导航、架构放大与关闭、编码滑块、训练/推理切换、原始数据展开、窄屏侧栏。检查相对链接、图片和 CSV 资源可达；无需训练模型即可预览网站。

消融图由 `scripts/render_figures.py` 使用 ReportLab Charts 从 CSV 生成；更新数据后重跑脚本，并同步 HTML 数据表和说明。

RT-1 图表由 `scripts/render_rt1_figures.py` 使用同一图表库生成。
