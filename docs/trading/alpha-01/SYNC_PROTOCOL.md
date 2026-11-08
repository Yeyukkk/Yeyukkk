# Alpha 01 — GPT / Muse / Codex 同步协议 v0.1

## 角色
- Rainy（Owner）：决定对外公开范围、资本和账户操作。
- GPT（本线研究审查 / Director）：决定研究问题、审查证据与结论、出具 GO/NO-GO。
- Muse（Research Operator）：合法数据采集、基本面筛选、研究记录、定时观察，负责发起研究材料更新。
- Codex（Engineering）：仓库代码、研究管线、单元测试、版本管理及技术修复。

## GitHub 用途
GitHub 是**已提交文件的版本与交接基准**，不是实时交易状态真源。研究运行状态以其实际日志和数据源为准；GitHub 文档必须附 `source_as_of`。

单次交接记录：
`workflow_id | logical_asset_id | source_path | source_sha256 | git_commit | authored_at | observed_as_of | evidence_level | owner | receiver_ack`。

证据标签：`REPORTED`（口头/摘要）→ `TRANSFER_VERIFIED`（收到相同文件）→ `SOURCE_VERIFIED`（来源独立核验）→ `RUNTIME_VERIFIED`（在目标环境重放/运行）。不能跨级声称。

## 分批上传过程
1. Muse 或 Codex 清点 **Alpha 01** 文件并检查许可证、隐私、来源和当前版本；生成待上传清单，不批量提交不明 ZIP。
2. 只上传可公开的研究方法、非敏感代码、经许可数据摘要、决策及脱敏实验结果。付费原始行情、密钥、私账、个人身份及带隐私附件禁止进入此公开仓库。
3. 创建独立 branch / pull request，附 SHA-256 清单及新增/更新/冲突文件表。
4. Rainy / GPT 确认研究和安全边界后合并；Codex、Muse 分别检出同一 commit 并验证哈希，回报接收版本。
5. 后续更新采用 append-only 证据原则，不回改已冻结原件；校正使用附则。发生冲突先对账，不静默覆盖。

## 运行模式
- 每次交接请在消息里附 GitHub 文件链接、commit SHA、文件 SHA-256 和 source_as_of。
- 没有接收环境回执，状态是 `NOT_SYNCED`，不是 `SYNCED`。
- 研究报告不得把价格变化、回测或工程测试数当作确定盈利。
