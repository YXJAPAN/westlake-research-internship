# 2026年11月13日：三结构域生成与 AlphaFold 3 结构置信度评估

## 实验设计

本阶段围绕 DOMINO 三结构域 AB→C 任务，开展四种 Training-free 方法的结构评价及 Oracle Conditioning 对照实验。

- **四方法结构评价。** 从已有 Benchmark 中选取 1,000 条 AB 查询，分别采用 Native-3Di-Add、Pred-3Di-Add、DOMO-Pred 和 DOMO-Seq 检索第三结构域，再通过 DOMO 生成完整蛋白，共获得 4,000 条序列，并使用 AlphaFold 3 评价 pTM 和 pLDDT。
- **Oracle Conditioning 实验。** 从上述 1,000 条查询中随机抽取 100 条，将天然真实的 A、B、C 三个结构域直接输入 DOMO，生成 100 条完整蛋白序列。
- **Natural Reference 对照。** 提取相同 100 条天然蛋白的原始完整序列，与 Oracle DOMO 生成的 100 条序列共同进行 AF3 预测，比较两组结构置信度。

## 主要结论

- **简单加和方法表现最佳。** Native-3Di-Add 在四种 Training-free 方法中取得最高的平均 pTM 和 pLDDT，说明天然结构 Token 的直接加和是具有竞争力的检索基线。
- **真实第三域能够改善生成质量。** Oracle DOMO 的结构置信度高于四种检索条件下的生成结果，说明第三域选择对下游结构表现具有重要影响。
- **提供真实的三个结构域，Oracle DOMO 的结构置信度仍低于 Natural Reference，表明三域条件生成与天然蛋白之间仍存在差距。


## 本目录材料

- **天然序列与多种条件生成序列的结构置信度比较.pdf**
- **天然序列与多种条件生成序列的结构置信度比较.pptx**：对应的科研组会汇报 PPT。
