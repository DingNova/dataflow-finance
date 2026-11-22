# dataflow-finance-qa

> DataFlow-Extension：金融财报/研报 → 高质量 QA 数据集。含 4 个算子、1 条端到端 Pipeline、金标评测集、数据卡模板与合规说明。
>
> 简体中文 ｜ [English](#english) ｜ License: Apache-2.0

## ✨ 功能

| 算子 | 类型 | 说明 |
|---|---|---|
| `FinancialPDFParser` | 解析 | 金融 PDF（年报/研报）→ 纯文本，pdfplumber/pypdf 双引擎 |
| `FinancialQAGenerator` | 生成 | LLM 生成严格锚定原文的 QA 对（数值/事实/归因三类） |
| `FinancialHallucinationFilter` | 过滤 | LLM 逐条判定答案是否被来源文本支撑，剔除幻觉样本 |
| `NumericConsistencyRefiner` | 精炼 | 逐项核对数字/单位/比例一致性，只修不改 |

Pipeline：`FinanceQAPipeline`（parse → generate → filter → refine），配套 YAML 配置。

## 🚀 快速开始

```bash
# 1. 安装主框架与扩展
pip install open-dataflow
pip install -e .

# PDF 解析需要
pip install -e ".[pdf]"

# 2. 配置 LLM 服务
export DF_API_KEY=sk-xxx

# 3. 运行 Pipeline
python -c "
from dataflow.utils.storage import FileStorage
from dataflow.serving import APILLMServing_request
from dataflow_finance_qa import FinanceQAPipeline

storage = FileStorage(first_entry_file_name='./input.jsonl')  # [{\"pdf_path\": ...}]
llm = APILLMServing_request(api_url='https://api.openai.com/v1/chat/completions')
FinanceQAPipeline(llm_serving=llm, storage=storage).run()
"
```

## 📊 评测集

- `eval/finance_qa_eval_set.jsonl`：30 条人工金标（factual/numeric/causal × easy/medium/hard）
- `eval/expand_evalset.py`：LLM 扩充至 200 条（**发布前须人工抽检 ≥20%**，见 COMPLIANCE.md 第 4 节）

## 🗂 数据卡与合规

- `data_cards/DATA_CARD_TEMPLATE.md`：数据卡模板（来源/口径/敏感性/质量五节）
- `data_cards/finance_qa_dataset_card_example.md`：本包评测集数据卡
- `COMPLIANCE.md`：法规映射、来源红线、算子合规设计、免责声明

## 🧪 测试

```bash
pip install -e ".[dev]" && pytest
```

## 📖 相关资源

- 上游框架：[OpenDCAI/DataFlow](https://github.com/OpenDCAI/DataFlow)（SIGMOD'27）
- 生态指南：[DataFlow-Doc Ecosystem](https://opendcai.github.io/DataFlow-Doc/)

## <a id="english"></a>English

`dataflow-finance-qa` is a [DataFlow](https://github.com/OpenDCAI/DataFlow) extension that turns financial PDFs (annual reports, research notes) into source-grounded QA datasets, with an LLM hallucination filter and a numeric-consistency refiner. It ships 4 operators, one end-to-end pipeline, a 30-sample golden eval set (expandable to 200), data-card templates and a compliance guide. See the Chinese sections above for details; all operator descriptions support `get_desc(lang="zh"/"en")`.

---

⚠️ **Disclaimer**: for research and engineering purposes only. Not investment advice.
