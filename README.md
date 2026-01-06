---
license: Apache-2.0
task_categories:
  - text-generation
  - question-answering
language:
  - zh
tags:
  - education
  - exam
  - qa
  - policy
  - catalog
  - scores
  - sft
  - cpt
size_categories:
  - 100K<n<1M
---

# CN-Grad-Consult-Dataset (高等教育考研咨询数据集)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Task](https://img.shields.io/badge/Task-SFT%20%7C%20RAG-green)]()
[![Language](https://img.shields.io/badge/Language-Chinese-red)]()

## 📢 特别说明 (Availability Note)
> ⚠️ **关于数据完整性**：
> 由于 GitHub 文件大小限制，本仓库目前**仅直接提供 CPT (连续预训练) 部分**的数据文件。
> **SFT (指令微调) 部分**包含高质量问答对，如需获取完整版 SFT 数据集，请查看底部的 [📧 联系方式](#-contact-联系方式)。

---

## 📖 Dataset Summary (数据集简介)

本数据集是面向**考研与高校招生教育领域**的垂直语料库，内容覆盖招生目录、政策解读、院校概况、历年分数线、考试结构解析与考生备考经验等。

旨在帮助开发者构建教育领域的垂直大模型，适用于以下场景：
* **教育领域大模型微调 (SFT)**：提升模型在考研咨询场景下的对话能力。
* **检索增强生成 (RAG) 知识库构建**：提供高质量的外部知识源。
* **领域适应连续预训练 (CPT)**：注入教育领域的专业背景知识。

* **数据总量**：约 398.36 MB (全量)
* **数据来源**：研招网、各高校研究生院官网、公开经验分享贴等。

---

## 📂 Data Structure (文件结构与统计)

数据集分为 `sft`（问答对）与 `cpt`（纯文本）两类。

### 1. CPT 连续预训练文本 (`data/cpt/`)
> ✅ **本仓库已包含以下数据**，可直接 Clone 使用。

| 文件名 | 大小 | 条数 | Tokens | 内容描述 |
| :--- | :--- | :--- | :--- | :--- |
| `policies_cpt.jsonl` | 116.43 MB | 27,762 | 27.50 M | 政策/通知类文本 |
| `catalog_cpt.jsonl` | 68.95 MB | 132,124 | 15.84 M | 招生目录类文本 |
| `scores_cpt.jsonl` | 15.22 MB | 44,917 | 4.30 M | 分数线/成绩类文本 |
| `school_profile_cpt.jsonl` | 0.11 MB | 349 | 22 K | 院校简介文本 |

### 2. SFT 指令微调数据 (`data/sft/`)
> 🔒 **该部分数据未直接上传至 GitHub**，请联系作者获取。

| 文件名 | 大小 | 条数 | Tokens | 内容描述 |
| :--- | :--- | :--- | :--- | :--- |
| `catalog_qa.jsonl` | 161.59 MB | 264,248 | 27.10 M | 招生目录/专业目录问答 |
| `scores_qa.jsonl` | 28.42 MB | 44,917 | 6.19 M | 分数线/成绩相关问答 |
| `experience_*.jsonl` | ~6 MB | ~1.6K | ~1.4 M | 备考经验 (规划/教训/建议) |
| `exam_structure_qa.jsonl` | 1.40 MB | 101 | 360 K | 考试结构与题型分值 |
| `school_profile_qa.jsonl` | 0.21 MB | 429 | 32 K | 院校概况问答 |

*(注：Tokens 统计基于 Qwen2.5-7B-Instruct tokenizer)*

---

## 📥 Usage (如何使用)

### 获取 CPT 数据 (当前仓库)
您可以直接克隆本仓库获取 CPT 数据进行预训练实验：

```bash
git clone [https://github.com/LuoDaa/CN-Grad-Consult-Dataset.git](https://github.com/LuoDaa/CN-Grad-Consult-Dataset.git)
cd CN-Grad-Consult-Dataset
Python 读取示例 (Pandas):

Python

import pandas as pd
import os

# 读取 CPT 数据示例
file_path = './data/cpt/policies_cpt.jsonl'

if os.path.exists(file_path):
    df = pd.read_json(file_path, lines=True)
    print(f"成功加载 {len(df)} 条数据")
    print(df.head())
else:
    print("文件不存在，请检查路径")
🔍 Data Schema (数据字段说明)
1. CPT 文本格式 (本仓库包含)
适用于 data/cpt/*.jsonl：

JSON

{
    "text": "此处为长篇的政策文本或招生简章内容...",
    "meta": {
        "source": "数据来源 (如: yz.chsi.com.cn)",
        "url": "原始链接（可选）",
        "title": "文章标题"
    }
}
2. SFT 问答格式 (完整版包含)
适用于 data/sft/*.jsonl：

JSON

{
    "instruction": "请问北京大学计算机专硕的复试分数线是多少？",
    "input": "",
    "output": "根据北京大学发布的数据，计算机专硕复试线为...",
    "meta": {
        "source": "数据来源",
        "school": "北京大学",
        "year": "2024"
    }
}
⚠️ Limitations (局限性与免责)
时效性：考研政策与分数线每年更新，模型训练时请务必参考 meta 中的 year 字段，避免产生跨年份的知识幻觉。

数据噪声：经验贴数据来源于网络公开分享，可能包含主观偏差或非正式用语，建议使用前进行清洗。

免责声明：本数据集仅供学术研究与大模型训练参考，实际报考信息请以研招网或各高校研究生院官方发布的最新通知为准。

📧 Contact (获取完整 SFT 数据)
如果您需要获取 完整版 SFT 指令微调数据集 (包含所有问答对)，或者有商业合作、数据反馈需求，请通过以下方式联系：

Email: 864034128@qq.com

邮件主题建议: [数据集申请] CN-Grad-Consult-Dataset 完整版申请

💡 申请时请简要说明您的使用用途（如：个人学习、学术研究、企业项目），以便快速通过审核。
