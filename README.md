# CN-Grad-Consult-Dataset
本数据集是面向考研与高校招生教育领域的垂直语料库，包含知识性文本（CPT）与问答式指令微调数据（SFT）。内容覆盖招生目录、政策解读、院校概况、历年分数线、考试结构解析与考生备考经验等。  总文件大小：约 398.36 MB 数据来源：研招网、各高校研究生院官网、公开经验分享贴等。 适用场景： 教育领域大模型微调 (SFT) 检索增强生成 (RAG) 知识库构建 领域适应连续预训练 (CPT)


```markdown
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

# Kaoyan-Consult-Dataset (高等教育考研咨询数据集)

## 📖 Dataset Summary (数据集简介)

本数据集是面向**考研与高校招生教育领域**的垂直语料库，包含**知识性文本（CPT）**与**问答式指令微调数据（SFT）**。内容覆盖招生目录、政策解读、院校概况、历年分数线、考试结构解析与考生备考经验等。

* **总文件大小**：约 398.36 MB
* **数据来源**：研招网、各高校研究生院官网、公开经验分享贴等。
* **适用场景**：
    * 教育领域大模型微调 (SFT)
    * 检索增强生成 (RAG) 知识库构建
    * 领域适应连续预训练 (CPT)

---

## 📥 Usage (如何下载与使用)

### 方式一：Git Clone (推荐，无需安装依赖)
如果您遇到 Python 环境依赖冲突，建议直接通过 Git 下载原始 JSONL 文件，然后使用 Pandas 读取。

```bash
# 1. 安装 Git LFS (若已安装可跳过)
git lfs install

# 2. 克隆仓库
git clone [https://www.modelscope.cn/datasets/L7July/CN-Grad-Consult-Dataset.git](https://www.modelscope.cn/datasets/L7July/CN-Grad-Consult-Dataset.git)

```

**Python 读取示例 (Pandas):**

```python
import pandas as pd
import os

# 读取 SFT 数据示例
file_path = './CN-Grad-Consult-Dataset/data/sft/catalog_qa.jsonl'
if os.path.exists(file_path):
    df = pd.read_json(file_path, lines=True)
    print(df.head())

```

### 方式二：ModelScope SDK

```python
from modelscope.msdatasets import MsDataset

# 加载特定子集 (需安装 modelscope 库)
ds = MsDataset.load('L7July/CN-Grad-Consult-Dataset', subset_name='default', split='train')

```

---

## 📂 Data Structure (文件结构与统计)

数据集文件按用途分为两类：`sft`（指令微调问答）与 `cpt`（连续预训练文本）。所有数据均为 `.jsonl` 格式。

> **统计口径说明**：
> * **条数**：JSONL 行数
> * **Tokens**：使用 `Qwen2.5-7B-Instruct` tokenizer 统计 (SFT统计 `instruction+input+output`，CPT统计 `text`，不含 special tokens)。
> 
> 

### 1. SFT 指令微调数据 (`data/sft/`)

| 文件名 | 大小 | 条数 | Tokens | 内容描述 |
| --- | --- | --- | --- | --- |
| `catalog_qa.jsonl` | 161.59 MB | 264,248 | 27.10 M | 招生目录/专业目录问答 |
| `scores_qa.jsonl` | 28.42 MB | 44,917 | 6.19 M | 分数线/成绩相关问答 |
| `experience_general_plan.jsonl` | 2.28 MB | 521 | 575 K | 备考经验-总体规划 |
| `experience_key_lessons.jsonl` | 1.73 MB | 521 | 400 K | 备考经验-关键教训 |
| `experience_personal_advice.jsonl` | 1.99 MB | 570 | 456 K | 备考经验-个人建议 |
| `exam_structure_qa.jsonl` | 1.40 MB | 101 | 360 K | 考试结构与题型分值 |
| `school_profile_qa.jsonl` | 0.21 MB | 429 | 32 K | 院校概况问答 |
| `common_qa.jsonl` | 0.01 MB | 48 | 2 K | 通用常见问答 |

### 2. CPT 连续预训练文本 (`data/cpt/`)

| 文件名 | 大小 | 条数 | Tokens | 内容描述 |
| --- | --- | --- | --- | --- |
| `policies_cpt.jsonl` | 116.43 MB | 27,762 | 27.50 M | 政策/通知类文本 |
| `catalog_cpt.jsonl` | 68.95 MB | 132,124 | 15.84 M | 招生目录类文本 |
| `scores_cpt.jsonl` | 15.22 MB | 44,917 | 4.30 M | 分数线/成绩类文本 |
| `school_profile_cpt.jsonl` | 0.11 MB | 349 | 22 K | 院校简介文本 |

---

## 🔍 Data Schema (数据字段说明)

### SFT 问答格式

适用于 `data/sft/*.jsonl` 下的所有文件：

```json
{
    "instruction": "问题/指令",
    "input": "可选补充信息（可能为空字符串）",
    "output": "回答/目标输出",
    "meta": {
        "source": "数据来源 (如: yz.chsi.com.cn)",
        "school": "学校名称（可选）",
        "year": "年份（可选）"
    }
}

```

### CPT 文本格式

适用于 `data/cpt/*.jsonl` 下的所有文件：

```json
{
    "text": "连续预训练文本内容",
    "meta": {
        "source": "数据来源",
        "url": "原始链接（可选）",
        "title": "标题（可选）"
    }
}

```

---

## ⚠️ Limitations (局限性说明)

1. **时效性**：部分数据（如分数线、政策）具有强时效性，模型训练时请注意参考 `meta` 中的年份信息，避免混淆不同年份的政策。
2. **数据噪声**：经验类数据来源于公开网络信息整理，可能包含少量营销词汇（如直播预告、推广语）或格式噪声，建议在正式训练前根据需求进行二次清洗。
3. **免责声明**：本数据集仅供科研与模型训练参考，实际报考信息请以研招网或各学校官网发布的官方通知为准。

---

## 📧 Contact (联系方式)
本次只提供CPT继续预训练数据集，如果需要获得完整SFT数据集请联系↓
如需获取更多垂直领域数据、反馈数据问题或进行商业合作，请联系：**864034128@qq.com**

```

```
