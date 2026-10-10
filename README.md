# VLA Notes

面向 GitHub Pages 的静态学习站点。当前包含六个技术专题的主页，以及 OpenVLA、RT-1、RT-2、ACT、OpenVLA-OFT、FAST 六篇精读页，以及前沿论文专题中的 Astra 具身策略报告、ActionCodec v2 和 ActMem-VLA 三篇精读。

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
  papers/openvla-oft/index.html     OpenVLA-OFT 精读内容
  papers/fast/index.html            FAST 精读内容
  frontier/astra-policy/index.html 前沿报告：GPT 6 Astra as an Embodied Policy
  frontier/actioncodec/index.html  前沿论文：ActionCodec v2
  frontier/actmem-vla/index.html    前沿论文：Remember What You Did / ActMem-VLA
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

### OpenVLA-OFT

- 论文：arXiv:2502.19645v2（2025-04-28，RSS 2025）；整理日期 2026-10-09。
- 八个模块覆盖研究问题、并行架构、连续动作与训练、执行/效率、LIBERO、ALOHA、FiLM/消融与局限。
- OFT 与 OFT+ 分开；97.1% 完整输入与 95.3% 单视角配置分开；ALOHA 阶段得分与语言目标选择成功率分开。
- 两张理解重绘、四张实验图、六份 CSV；完整保留 Table I 分组和 Tables X–XIII 子阶段评分。
- 效率交互使用 Table II 的报告值与延迟演算，区分动作吞吐量、块查询频率和机器人执行节拍。
- 标注正文 ms 单位问题、Prismatic VLM 初始化消融、整块开环执行与原版 ACT 时序集成的区别。
- `scripts/render_oft_figures.py` 生成图表与机制图；源码复现和机器人实验暂缓。

### FAST

- 论文：arXiv:2501.09747v1（2025-01-16）；整理日期 2026-10-09。
- 八个模块覆盖冗余问题、编解码、DCT/量化、BPE/自回归、FAST+、实验协议、训练/推理成本、局限。
- 两张机制理解重绘、三张数据图、七份 CSV；完整保留 Table I、词表训练混合、Tables II–III 和定性证据范围。
- DCT 交互为正交 DCT-II 教学演算，不加载 FAST+，不报告真实 BPE token 数；可调尺度、重建曲线与误差。
- 明确量化有损、BPE 无损、FAST+ 的词表学习和 VLA 策略训练的区别。
- 保留 DROID 正文16任务与表17行、Laundry任务专用微调、离线人形/驾驶编码与控制实测的区别。
- 成功率曲线未做读图猜测；约750ms与100ms内的延迟、3x步骤与5x GPU小时分别标注。
- `scripts/render_fast_figures.py` 生成图表与机制图；源码和机器人实验复现暂缓。

## 前沿论文专题

首页 `#frontier` 与全站侧栏设独立入口；技术报告与原有六个技术专题关联，但使用 Frontier 编号。

### Astra 作为具身策略 · F01

- 报告公开快照：2026-09-13；固定网站提交 `6ead090465d3b0fb19e90eb70569689c19f73d00`；整理日期 2026-10-10。
- 九个模块覆盖 agent 闭环、Outcome/Intent gate、执行接口、双 benchmark 协议、结果、行为、成本与局限。
- 两张机制重绘、五张数值图（包括逐任务 Score／成功率两种切换图）、七份 CSV 与源文件哈希记录。
- 两项教学交互：候选前缀执行长度；Direct 缺失评分的算术敏感性边界。
- 保留 RoboDojo Direct Score n=48／成功分母50及两条未终止记录；RoboLab 最终槽位包含历史结果、重试、非严格配对与决策预算差异。
- 分开标注 RoboDojo 任务微调学生、RoboLab DROID 权重零样本参照、官方成绩重加权和历史 baseline。
- token 总量含缓存输入；14.4% 是执行步占比；物理时间不包含模型墙钟等待。未补造置信区间。
- `scripts/render_astra_figures.py` 生成图表与理解重绘。本轮未运行模型或控制环境。

### ActionCodec · F02

- 论文固定为 arXiv:2602.15397v2（2026-10-08）；整理日期 2026-10-10；覆盖正文及附录 A–F。
- 十个模块解释 VQ、确定性/扰动熵、OR、容量、TCL/CLIP、架构耦合、soft prompt、RVQ 后训练、策略变体与迁移。
- 两张机制重绘、五张实验图、九份 CSV；完整保留 Tables 1(a–d)、2–5、10–11 的分组与报告值，另存训练协议。
- 三项教学交互：对应位置 OR 与集合交集对照、容量上界、动作吞吐量与查询频率。
- 区分主表无策略 VLA 预训练与 tokenizer 预训练、15K 小时大规模 VLA 预训练；区分 AR/PD/KI/BAR 和 RVQ 部署路径。
- 标注 VQVLA 原表平均数疑点、组内排名、不同 horizon、FAST 非法长度修复与未说明的推理计时设备；未猜测曲线/柱高或补造不确定度。
- `scripts/render_actioncodec_figures.py` 从 CSV 生成图表及机制图；源码训练与机器人复现暂缓。

### ActMem-VLA · F03

- 论文固定为 arXiv:2609.37307v1（2026-09-29）；整理日期2026-10-10；全文8页，无附录。
- 九个模块解释动作历史、Mamba-2状态、冻结范围、4/6次flow更新、插件训练、两种仿真预算、消融与实机结果。
- 两张机制重绘、五张实验图、七份CSV；Figure 4读取PDF印出的全部数字标签，Tables I–II完整保存；不猜Figure 3雷达读数。
- 两项交互：Table II交接比率与专家更新时间轴、Eq.4的95分位上取整/600封顶。
- 区分180次/任务主比较与60次/任务单训练种子消融；保留未明的消融预算、H/s与完整训练/计时规格。
- 实机51/80对28/80，精确差28.75个百分点；保留T8去PAE更好的消融反例，未把动作历史当成果确认。
- 原文及本页中文改编/重绘为CC BY-SA 4.0；署名、改编与许可记录存于`dist/data/actmem-license.txt`。
- `scripts/render_actmem_figures.py`可重生成图表；未运行模型或机器人复现。

## 验证

用浏览器检查主页到论文页、章节导航、架构放大与关闭、编码滑块、训练/推理切换、原始数据展开、窄屏侧栏。检查相对链接、图片和 CSV 资源可达；无需训练模型即可预览网站。

消融图由 `scripts/render_figures.py` 使用 ReportLab Charts 从 CSV 生成；更新数据后重跑脚本，并同步 HTML 数据表和说明。

RT-1 图表由 `scripts/render_rt1_figures.py` 使用同一图表库生成。
