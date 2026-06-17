---
title: "Breaking the bottleneck: self-supervised deep learning framework for fully automated fossil CT segmentation"
title_zh: 打破瓶颈：用于全自动化石CT分割的自监督深度学习框架
authors: "Roy, A., Ghosh, P., Weston, F., Hartley, B., Salili-James, A., Poon, S. T. S., Maidment, S. C. R., Butler, R. J."
date: 2026-06-11
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.07.730692v1.full.pdf"
tags: ["query:semantic-seg"]
score: 9.0
evidence: 化石CT数据的自监督语义分割
tldr: "古生物CT化石分割因标注样本匮乏和极低对比度，长期面临劳动密集（每标本≥100小时）和主观性强等瓶颈，阻碍大规模分析。本文提出结合SimCLR v1对比预训练与U-Net的自监督框架，通过确定性伪标签生成实现全自动分割，无需任何人工标注。在涵盖两栖、爬行、恐龙和早期哺乳动物的5万余张CT图像上，未参与训练的标本上Dice达93.66%、IoU 82.42%，跨类群泛化达到亚体素精度。该方法将单标本处理时间从约100人时降至1-3分钟，有望实现化石CT数据的批量快速处理，推动定量古生物学研究。"
source: biorxiv
selection_source: fresh_fetch
motivation: 古生物CT分割因标注稀缺和低对比度，需大量人工（≥100小时/数据集），限制大规模分析，亟需免标注自动化方案。
method: 结合SimCLR v1对比预训练、确定性伪标签生成与U-Net精化的自监督框架，免除人工标注，实现全自动化石CT分割。
result: "在50,626张多类群CT图像上，未见标本Dice 93.66%、IoU 82.42%，泛化至六个外部标本并达亚体素精度。"
conclusion: 将处理时间从百小时级降至分钟级，为化石CT批量处理提供可行路径，推动古生物学定量研究。
---

## 摘要
在应用于科学领域的深度学习中，对标记训练样本稀缺且前景-背景对比度低的特定领域成像数据进行语义分割，仍然是一个开放性挑战。古生物学计算机断层扫描（CT）就是这一问题的典型例子：将化石骨骼从周围的岩石基质中数字化分离出来劳动强度大（每数据集 ≥100 小时），主观性强，且通常依赖昂贵的专有软件，从而形成了“分割瓶颈”，阻碍了大规模、快速的CT数据处理。在此，我们提出一个自监督端到端框架，结合SimCLR v1对比预训练、确定性伪标签生成和U-Net细化，实现了无需人工标注的全自动化石CT分割。利用来自中侏罗统Kilmaluag组的50,626张CT图像，涵盖两栖类、爬行类、恐龙和早期哺乳动物，该框架在训练期间未见过的留出标本上达到了93.66%的Dice系数和82.42%的交并比（IoU），与近期基于深度学习的化石CT分割研究中报道的最高值相当。通过几何方式在六个完全独立的外部标本上验证了跨分类群的泛化能力，实现了与人工阈值参考的亚体素级别网格一致。通过消除先前深度学习古生物学方法中所需的标注要求，该框架将每个标本的处理时间从约100人时减少到6小时（一次性UNet训练）+1-3分钟（每个标本的网格生成），是迈向大规模比较和定量分析的CT数据批处理和分析的关键第一步。

## Abstract
Semantic segmentation of domain-specific imaging data where labelled training examples are scarce and foreground-background contrast is low remains an open challenge in deep learning applied to science. Palaeontological computed tomography (CT) exemplifies this problem: digitally isolating fossilised bone from surrounding rock matrix is labour-intensive ([&ge;]100 hrs/dataset), subjective, and often reliant on expensive proprietary software, creating a "segmentation bottleneck" that prevents large-scale and rapid processing of CT data collections. Here we present a self-supervised, end-to-end framework combining SimCLR v1 contrastive pretraining with deterministic pseudo-label generation and U-Net refinement to fully automate fossil CT segmentation without manual annotation. Using 50,626 CT images from the Middle Jurassic Kilmaluag Formation spanning amphibians, reptiles, dinosaurs, and early mammals, the framework achieved a Dice coefficient of 93.66% and IoU of 82.42% on a held-out specimen not seen during training, comparable to the highest Dice and IoU values reported in recent Deep Learning-based fossil CT segmentation studies. Cross-taxon generalisation was validated geometrically on six fully external specimens, achieving sub-voxel mesh agreement with manually thresholded references. By eliminating the annotation requirement that has limited prior deep learning approaches in palaeontology, this framework reduces per-specimen processing from [~]100 person-hours to 6 hrs (one-time UNet training) +1-3 minutes (mesh generation per specimen), an essential first step towards batch processing and analysis of CT data for large-scale comparative and quantitative analyses.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究背景**：古生物 CT 图像中，化石骨骼与围岩基质对比度极低，需要进行语义分割以数字化分离骨骼。
- **核心瓶颈**：传统分割依赖人工，劳动强度极大（每数据集 ≥100 人时），主观性强，且常使用昂贵的商业软件，形成“分割瓶颈”，严重阻碍大规模 CT 数据的快速处理与定量古生物学研究。
- **整体含义**：本文旨在打破这一瓶颈，提出一种**完全无需人工标注的自监督深度学习框架**，实现化石 CT 的全自动分割，将处理时间从天/周级压缩至分钟级，推动古生物学进入批量、可重复的定量分析时代。

### 2. 论文提出的方法论
- **核心思想**：通过自监督对比学习提取稳健的图像表征，并借助确定性伪标签将表征转化为高质量分割结果，最终用轻量级细化网络提升精度，全程无需人工标注。
- **关键技术环节**：
  - **SimCLR v1 对比预训练**：利用无标签 CT 切片，通过数据增强与对比损失学习像素级与语义级特征，使模型能够区分前景（化石）与背景（基质）的细微差异。
  - **确定性伪标签生成**：基于预训练表征，采用确定性策略（可能为聚类或基于激活图的阈值化）为每一张图像生成初始分割掩码，充当无监督的“教师”标签。
  - **U-Net 细化**：将伪标签作为监督信号，训练一个 U-Net 进行像素级语义分割，以修正伪标签中的噪声和边界模糊，最终输出高精度分割。
- **流程总结**：
  > 未标注 CT 切片 → SimCLR v1 预训练 → 特征图 → 确定性伪标签生成 → 训练 U-Net 精调 → 化石分割结果。

### 3. 实验设计
- **数据集与场景**：
  - 内部数据集：**50,626 张**来自中侏罗统 Kilmaluag 组的 CT 图像，涵盖两栖类、爬行类、恐龙和早期哺乳动物等多种类群。
  - 内部测试：**留出一个完整标本（未见参加训练）**，用于评估分割精度。
  - 外部泛化测试：**6 个完全独立的外部标本**，用于验证跨分类群与跨样本的几何网格一致性。
- **基准（Benchmark）**：
  - 内部测试以留出标本的人工标注（或高质量手动阈值分割）为参考，计算 Dice 系数和 IoU。
  - 跨类群泛化以人工阈值参考结果为标准，检查网格偏移是否达到亚体素级别。
- **对比方法**：
  - 本文并未显式列出并复现其他方法进行一一对比，而是将取得的 **Dice 93.66%**、**IoU 82.42%** 与“近期基于深度学习的化石 CT 分割研究中报道的最高值”进行了横向比较，宣称处于相当水平。

### 4. 资源与算力
- 文中**未明确说明**具体的 GPU 型号、数量或显存配置。
- 仅提及**一次性 U-Net 训练耗时约 6 小时**，每个标本的网格生成阶段仅需 **1–3 分钟**。
- 整体计算负载似乎较低，适合普通科研硬件，但缺乏详细硬件参考使得可复现性稍打折扣。

### 5. 实验数量与充分性
- **实验组数**：
  - 至少包含：① 内部留出标本测试（1 组，同一套数据但不同标本）；② 外部泛化验证（6 组独立标本）；可能还包含预训练/精调环节不同策略的消融实验（但摘要未提及）。
- **充分性与公平性评估**：
  - 实验覆盖了**类群内未见标本**和**完全外部数据**，验证了框架的泛化能力，整体设计较为合理。
  - 不足之处在于**未与现有深度学习方法进行同条件对比**，仅通过文献数据对标，可能因岩石性质、采集参数、标注标准不同而引入偏差，公平性存疑。
  - 外部标本数量为 6 个，虽具说服力但仍属小规模验证，未来需要更多样、更大规模的地层与分类群测试。

### 6. 论文的主要结论与发现
- 所提自监督框架在**无需任何人工标注**的条件下，在内部留出标本上达到 Dice 93.66%、IoU 82.42%，与近期最优监督/半监督方法相比毫不逊色。
- 在六个外部标本上实现了**亚体素级别的网格一致性**，证明跨类群泛化能力坚固，不易过拟合特定形态。
- 将单个标本的处理时间从约 **100 人时**急剧压缩至 **6 小时（模型训练，一次性）+ 1–3 分钟（推理与网格生成）**，彻底打破了传统分割的时间瓶颈。
- 该框架是迈向古生物 CT 数据**批处理和大规模比较定量分析**的关键一步。

### 7. 优点
- **完全免标注**：彻底摆脱了深度学习在化石分割中对昂贵且稀缺的人工标注的依赖，极大降低了应用门槛。
- **效率飞跃**：实现了3个数量级的处理速度提升，使大规模化石 CT 数据自动化分析成为可能。
- **跨类群鲁棒性**：在两栖、爬行、恐龙和早期哺乳动物等形态差异极大的类群上均表现优异，且经受住了外部标本的几何级验证。
- **方法清晰可复现**：采用 SimCLR v1 + 确定性伪标签 + U-Net 的主流架构，模块化设计便于后续改进和迁移。实验结果公开透明，推动开放科学。

### 8. 不足与局限
- **对比实验缺失**：未在同一数据集上复现并严格比较其他监督、自监督或传统方法，仅通过文献最高值进行非直接比较，削弱了“有效性”论证的可靠性。
- **数据来源单一**：训练数据全部来自**单一地层（Kilmaluag 组）**，面对不同沉积环境、成岩作用、扫描协议的数据时，泛化能力可能下降（虽已做外部测试，但未涉及极端异质场景）。
- **外部验证规模有限**：仅 6 个外部标本，不足以涵盖古生物学中广泛的CT成像多样性和埋藏学噪声。
- **方法论细节不清**（限于摘要）：确定性伪标签生成的具体算法未阐明；未讨论伪标签质量对最终精度的敏感性及可能存在的确认偏差；未分析何时可能失败（如化石与基质密度完全相同的情况）。
- **资源透明性不足**：缺乏 GPU 型号、训练 batch size 等细节，虽说明总耗时，但对计算资源的可复现性仍有影响。

（完）
