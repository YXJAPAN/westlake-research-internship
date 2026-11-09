# 2026年11月9日：三结构域 Training-free Benchmark 构建与四种检索方法评估

## 本次工作

围绕 DOMINO 从双结构域向三结构域组合检索的扩展，本阶段主要完成了三结构域 Benchmark 的构建、四种 Training-free 检索方法的实现，以及不同候选库规模下的系统评估。

- **三结构域测试集构建与同源性筛查。** 基于 TED 数据库中约 2,320 万条三结构域蛋白链，随机抽取 300,000 条，并使用 MMseqs2 与 DOMIN 训练集中的约 6,009 万条序列进行同源性筛查。最终获得 84,222 条至少包含一个 CATH 注释结构域的链，并从中随机冻结 10,000 条链，构建统一的 AB→C 检索 Benchmark。
- **四种 Training-free 方法设计。** 构建四条检索 Pipeline：Native-3Di-Add（原始 3Di Embedding 加和）、Pred-3Di-Add（预测 3Di Embedding 加和）、DOMO-Pred（DOMO 融合结合预测 3Di）以及 DOMO-Seq（DOMO 融合结合纯序列输入）。四种方法统一使用 DOMIN Encoder 提取查询嵌入。
- **大规模候选库检索。** 使用包含 40,782,345 个结构域的 FAISS-IVF 候选库，设置 nprobe=16，比较四种方法在 CATH 四个层级上的 Top-1 检索表现。
- **闭合候选库构建与精确检索。** 从 10,000 条测试链中提取 A、B、C 三个物理结构域实例，构建包含 30,000 个唯一结构域的闭合候选库，其中 23,143 个具有 CATH 标签。通过 Exact Search 计算完整相似度，分析四种方法在 Rank 1–10 的检索表现。
- **CATH 递进式宏平均评价。** 基于 CATH 的 Class、Architecture、Topology 和 Homology 四级分类，采用完整前缀匹配与宏平均统计，分别评价不同检索排名下的分类命中率。

## 主要实验结果

### 1. 四种方法的检索性能比较

在两种候选库设置下，Native-3Di-Add 均取得最佳的 Top-1 检索表现。

| Pipeline | 40.78M H-level | 30K H-level |
|---|---:|---:|
| Native-3Di-Add | 35.25% | 47.35% |
| Pred-3Di-Add | 21.63% | 28.16% |
| DOMO-Pred | 21.46% | 26.24% |
| DOMO-Seq | 0.38% | 0.34% |

### 2. 主要发现

- **高质量结构 Token 的重要性。** Native-3Di-Add 整体优于使用 ProstT5 预测结构 Token 的方法，说明原始结构表征能够提供更有效的检索信息。
- **结构 Token 对精细检索至关重要。** DOMO-Pred 与 DOMO-Seq 复用相同的 DOMO 生成序列，但将有效 3Di Token 替换为 `#` 后，H-level Top-1 从 26.24% 降至 0.34%，体现了有效结构信息对精细结构域检索的重要作用。
- **简单融合具有竞争力。** Native-3Di-Add 优于复杂的生成式融合方案，表明在当前任务中，结构表征质量可能比融合机制的复杂程度更加重要。
- **不同候选库具有互补性。** 40.78M FAISS-IVF 反映大规模候选空间中的 Top-1 检索能力；30K Exact Search 则在保证真实答案全部入库的条件下，进一步分析 Rank 1–10 的排序表现。

## 后续计划

### Training-free：AlphaFold 结构验证

从已经冻结的 10,000 条 AB→C Benchmark 查询中，随机抽取 1,000 条代表性样本，结合检索得到的第三结构域候选，将相应的 AB→C 组合输入 DOMO 进行推理，生成完整蛋白质序列。

随后将生成序列提交至 AlphaFold 3 进行结构预测，统计 pLDDT、pTM 等结构置信度指标，并结合结构域恢复情况，比较不同检索方法所得到的候选组合在结构层面的可设计性与合理性。


## 本目录材料

- **4种 Benchmark 构建.pdf**：四种 Training-free 方法的 Benchmark 构建与实验结果报告。
- **4种 Benchmark 构建.pptx**：本阶段实验汇报 PPT。
- **四种_Training_free_方法的_Benchmark_构建.pdf**：完整实验报告。
- **30K_Exact_CATH_Rank_Summary.tsv**：30K 闭合候选库 Rank 1–10 CATH 评价结果。
- **40M_IVF_CATH_Rank_Summary.tsv**：40.78M FAISS-IVF 候选库 Rank 1–10 CATH 评价结果。

本阶段完成了三结构域 AB→C 检索 Benchmark 的构建，以及四种 Training-free 方法在两种候选库下的系统比较。下一阶段将重点转向 DOMO 生成与 AlphaFold 3 结构验证，从检索分类一致性进一步评估候选组合的实际结构合理性。
