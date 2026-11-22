# Changelog

本仓库遵循 [scriv](https://scriv.readthedocs.io/) fragment 工作流：新增条目放入 `changelog.d/`，
发版时合并进本文件（若维护者无此要求则直接追加）。

## [0.1.0] - 2026-11-22

### Added
- 首个金融垂类 DataFlow-Extension
- 算子：`FinancialPDFParser` / `FinancialQAGenerator` / `FinancialHallucinationFilter` / `NumericConsistencyRefiner`
- Pipeline：`FinanceQAPipeline`（parse → generate → filter → refine）+ YAML 配置
- 评测集：30 条人工金标（`eval/finance_qa_eval_set.jsonl`）+ LLM 扩充脚本
- 数据卡模板与示例（`data_cards/`）
- 合规说明（`COMPLIANCE.md`）
- 离线单测与 CI（ubuntu/windows × py3.10-3.12）
