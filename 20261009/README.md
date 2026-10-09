# 2026年10月9日：讨论记录与后续计划

## 本次讨论

围绕三结构域组合检索，讨论了三种可借鉴的方案及对应论文：

- **PITF：两两分数加和。** 分别计算结构域之间的两两关系，再将分数相加，作为简单基线。
- **TIRG：先组合，再检索。** 将两个已知结构域合成一个查询表示，用它检索第三个结构域。
- **Symile：直接学习联合关系。** 对三个结构域共同打分，通过联合对比学习判断完整组合是否匹配。

目前先梳理三种方案的思路，后续结合数据分析与实验确定具体方法。

## 后续计划

### 1. 数据获取与简单分析

获取双域与三域数据，统计数据规模，并检查三域组合的两两子组合是否也出现在双域数据中。

### 2. Training-free 方案的指标设计

探究在不进行额外三域训练的情况下，如何定义三结构域组合的兼容性评分及评估指标，将这一问题作为持续深入研究的方向。

### 3. Positional embedding 与 DOMIN 中 Q/K 的关系

仔细研究 DOMIN 中 Q/K 的具体实现与作用，探究 positional embedding 是否能够替代原有 Q/K 机制，并明确这种替代的具体含义与适用条件。
尝试自行训练约 35M 参数规模的小模型，通过实际训练加深对模型结构与训练流程的理解，为后续方法探索积累经验。

### 4. Symile 的迁移价值与跨数量对齐

进一步研究 Symile，分析其向多结构域组合任务迁移的价值。
重点关注多线性点积从三个表征扩展至五个、六个表征时的分数尺度变化：当参与乘积的分量绝对值小于 1 时，增加乘积项可能使结果逐渐缩小。探究不同结构域数量的组合之间，如何实现评分尺度的对齐与可比性。


## 本目录材料

- 本次汇报 PPT。
- 本次书面报告 PDF。
- 三篇参考论文原文 PDF：
  - PITF — Pairwise Interaction Tensor Factorization for Personalized Tag Recommendation
  - TIRG — Composing Text and Image for Image Retrieval — An Empirical Odyssey
  - Symile — Contrasting with Symile: Simple Model-Agnostic Representation Learning for Unlimited Modalities
