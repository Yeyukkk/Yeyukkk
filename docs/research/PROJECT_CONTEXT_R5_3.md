# crypto_trading_system — R5.3 Project Context

Last updated from ChatGPT project discussion.

## 0. Project Goal

`crypto_trading_system` is a research-driven crypto trading system intended to eventually become an OKX AI-assisted trading bot. Current phase is strictly research/backtesting/diagnostics.

Hard constraints:

- No live trading.
- No OKX private endpoints.
- No API keys.
- No real orders.
- No paper live yet.
- BTC/USDT spot only for the current research phase.
- Ignore BTC/USDT:USDT swap/perpetual results for now.
- All performance results are research-only and not profitability claims.

## 1. Tool and Machine Roles

- GPT: strategy coordinator, experiment designer, result analyst, prompt writer, context compressor, anti-overfitting guard.
- Claude / Opus: methodology reviewer, overfitting-risk reviewer, research-decision-framework advisor, paper/live readiness reviewer.
- Google AI Studio / Gemini: code architecture reviewer, lookahead/data leakage reviewer, backtest-engine reviewer, production-gap reviewer.
- Codex: implementation assistant only. It writes code/tests and runs quick tests under 2 minutes. It should not run long backtests.
- M1 MacBook: original control machine, prompt/context management, light analysis, project packaging.
- M4 MacBook: main long-backtest machine. R5.3 is currently running locally in M4 Terminal.

M4 project path:

```bash
~/Downloads/crypto_trading_system
```

Conda env:

```bash
conda activate crypto_system
```

## 2. Current Research Definition

Current R5 work is **signal research**, not production execution research.

The system uses BTC/USDT spot OHLCV with a simulated leverage/risk model for relative strategy ranking. This is not a real OKX spot execution model, and it is not a real OKX spot-margin model.

Typical research parameters:

- Strategy: `SMCStrategyV2`
- Symbol: `BTC/USDT` spot
- LTF: `15m`
- HTF: `1h`
- Initial equity: `1000`
- Leverage: `10`
- Max margin pct: `0.20`
- Fee rate: `0.0005`
- Slippage: `2 bps`
- `allow_short` may be enabled for engine consistency, but key experiments are long-only.

Important production warning from Google AI: true OKX spot does not natively support leverage or shorting. If future execution uses spot margin, the engine must model borrow/repay, borrow interest, maintenance margin, collateral, liquidation, borrow limits, and auto-borrow mechanics. Therefore current R5 equity curves must not be cited as live OKX PnL.

## 3. R5.0 Summary — Spot Controlled Filters

R5.0 tested controlled filters on four 2024-only windows.

Best experiment:

```text
block_hours_7_8_13_15
```

Result:

- Avg net PnL pct: `+4.02%`
- Positive windows: `4/4`
- Worst net PnL pct: `+0.38%`
- Avg max drawdown pct: `5.10%`
- Total trades: `32`
- Avg profit factor: `2.07`

Verdict: promising but under-sampled. It failed robustness mainly because total trades were too low.

## 4. R5.1 Summary — Spot Extended Validation

R5.1 extended validation to ten windows from 2023-01-01 to 2024-09-01.

Compared:

- `reference_long_only_block_15`
- `block_hours_7_8_13_15`

`block_hours_7_8_13_15`:

- Windows: `10`
- Positive windows: `6/10`
- Positive window rate: `60%`
- Avg net PnL pct: `+0.68%`
- Worst net PnL pct: `-7.80%`
- Avg max drawdown pct: `5.78%`
- Total trades: `95`
- Avg profit factor: `1.38`

Reference:

- Avg net PnL pct: `-1.01%`
- Positive windows: `6/10`
- Worst net PnL pct: `-14.21%`
- Avg max drawdown pct: `8.50%`
- Total trades: `165`
- Avg profit factor: `1.02`

Verdict: `block_hours_7_8_13_15` materially improved the reference but failed robustness because:

- Positive window rate `< 70%`
- Worst net PnL pct `<= -6.0%`

## 5. R5.2 Summary — Failure-Window Diagnostics

R5.2 analyzed two failed windows:

- `2023-07-01 -> 2023-09-01`
- `2023-11-01 -> 2024-01-01`

### 2023-07 -> 2023-09

- BTC spot return: `-14.80%`
- BTC max drawdown: `23.27%`
- Reference: `-9.53%`
- `block_hours_7_8_13_15`: `-7.80%`
- Trades: `10`
- Profit factor: `0.06`

### 2023-11 -> 2024-01

- BTC spot return: `+22.23%`
- BTC max drawdown: `9.08%`
- Reference: `-14.21%`
- `block_hours_7_8_13_15`: `-7.27%`
- Trades: `10`
- Profit factor: `0.26`

Trade-level diagnostics found losses clustered around:

- `14 UTC`
- `16 UTC`
- New York session

Important trade-level observations:

2023-07 -> 2023-09:

- Entry hour 14 UTC: `7 trades`, `net_pnl -64.60`, `win_rate 0%`, `PF 0`
- New York killzone: `9 trades`, `net_pnl -71.41`, `win_rate 11.1%`
- Risk bucket 0.25%-0.50%: `7 trades`, `net_pnl -70.28`, `win_rate 0%`

2023-11 -> 2024-01:

- Entry hour 14 UTC: `2 trades`, `net_pnl -57.43`, `win_rate 0%`
- Entry hour 16 UTC: `3 trades`, `net_pnl -29.85`, `win_rate 0%`
- New York killzone: `5 trades`, `net_pnl -87.28`, `win_rate 0%`
- London killzone: `5 trades`, `net_pnl +14.62`, `win_rate 60%`, `PF 2.30`

Interpretation: the old narrative “New York good / London bad” is not reliable. In failed windows, New York was the biggest loss source. The strongest R5.3 hypothesis is that `block_hours_7_8_13_15` missed toxic hours 14 UTC and 16 UTC.

## 6. R5.3 Current Stage — Spot Hour Refinement

R5.3 is currently running on the M4 Terminal, not inside Codex.

Goal: validate whether adding 14 UTC and/or 16 UTC to blocked hours improves robustness.

R5.3 log:

```text
logs/r5_3_spot_hour_refinement_validation_console.log
```

Expected outputs:

```text
logs/r5_3_spot_hour_refinement_validation_results.csv
logs/r5_3_spot_hour_refinement_validation_details.csv
logs/r5_3_spot_hour_refinement_validation_report.txt
```

Experiments observed in the log:

1. `reference_long_only_block_15`
2. `block_hours_7_8_13_15`
3. `block_hours_7_8_13_14_15`
4. `block_hours_7_8_13_15_16`
5. `block_hours_7_8_13_14_15_16`
6. `block_hours_7_8_block_new_york`

R5.3 = `10 windows × 6 experiments = 60 backtests`.

R5.3 windows:

- `2023-01-01:2023-03-01`
- `2023-03-01:2023-05-01`
- `2023-05-01:2023-07-01`
- `2023-07-01:2023-09-01`
- `2023-09-01:2023-11-01`
- `2023-11-01:2024-01-01`
- `2024-01-01:2024-03-01`
- `2024-03-01:2024-05-01`
- `2024-05-01:2024-07-01`
- `2024-07-01:2024-09-01`

R5.3 robustness criteria:

- Windows `>= 8`
- Positive window rate `>= 70%`
- Avg net PnL pct `> 0`
- Avg profit factor `> 1.2`
- Worst net PnL pct `> -6.0`
- Total trades `>= 80`
- Avg max drawdown pct `< 10.0`

In-progress observation from Window 4 (`2023-07 -> 2023-09`):

```text
reference_long_only_block_15:     -9.53%, trades=16, max_dd=10.31%
block_hours_7_8_13_15:            -7.80%, trades=10, max_dd=8.59%
block_hours_7_8_13_14_15:         -2.60%, trades=5,  max_dd=3.10%
block_hours_7_8_13_15_16:         -7.15%, trades=8,  max_dd=7.48%
block_hours_7_8_13_14_15_16:      -1.30%, trades=2,  max_dd=1.30%
block_hours_7_8_block_new_york:   -1.30%, trades=2,  max_dd=1.30%
```

Interpretation: adding 14 UTC improves that failed window materially, but trade count collapses. This may be risk reduction by inactivity rather than proof of a real edge.

## 7. Claude Review Summary

Claude’s core methodology warning: R5.3 is high overfitting risk because the team observed losses on 2023-2024 data, added hour filters, and re-validates on the same windows.

Adopted Claude rules:

- R5.3 is the final in-sample hour-refinement stage.
- After R5.3, no more `block_hours_*` variants may be tested on the same 2023-01 to 2024-09 dataset.
- R5.3 winner, if any, must be frozen.
- Next stage must be R5.4 sealed holdout.
- No paper trading after R5.3, even if it passes.

Claude recommended R5.4 Tier-A holdout windows:

- `2022-01-01 -> 2022-03-01`
- `2022-03-01 -> 2022-05-01`
- `2022-05-01 -> 2022-07-01`
- `2022-07-01 -> 2022-09-01`
- `2022-09-01 -> 2022-11-01`
- `2022-11-01 -> 2023-01-01`
- `2024-09-01 -> 2024-11-01`
- `2024-11-01 -> 2025-01-01`
- `2025-01-01 -> 2025-03-01`
- `2025-03-01 -> 2025-05-01`

Claude also recommended later overfit diagnostics:

- Leave-one-out filter test
- Random-hour baseline
- Filter-marginal test
- Adjacent-hour test
- Forward-time stability

## 8. Google AI Review Summary

Google AI’s main engineering warning: current spot + leverage/margin-style backtest is a signal research tool, not a production-ready OKX spot execution model.

Other Google AI findings:

- HTF alignment appears lookahead-safe but may introduce conservative structural lag.
- Manual FVG/sweep lookbacks appear lookahead-safe.
- Same-candle SL/TP conflict defaults to stop-loss first, which is conservative and should be documented.
- Transaction costs are simplified and need 2x/3x slippage stress later.
- Signal generation is O(N²), because strategy is recalculated over expanding historical slices. Future optimization should precompute features or use stateful incremental structures.
- Production would need: state reconciliation, deterministic append-only audit trail, heartbeat alerts, risk engine, kill switch, private WebSocket manager, rate limiter, and execution layer.

## 9. Post-R5.3 Decision Framework

After R5.3 completes, do not ask “how do we optimize further?” Ask only: “does the result qualify for R5.4 sealed holdout?”

Decision tree:

### No experiment passes

- Close the hour-filter route.
- Do not add more hour filters.
- Move to regime diagnostics or strategy structure review.

### Winner passes but total trades < 100

- Weak / undersampled candidate.
- May run R5.4 only as falsification.
- Cannot paper trade.

### Winner has 100-150 trades

- Medium candidate.
- Freeze winner.
- Run R5.4 sealed holdout.

### Winner has >=150 trades and strong metrics

- Strong candidate.
- Still only moves to R5.4.
- No paper/live.

## 10. Current Prohibited Actions

Do not:

- Connect OKX private endpoint.
- Configure API keys.
- Start paper live.
- Live trade.
- Add more hour filters after R5.3.
- Modify engine logic before R5.4.
- Treat spot OHLCV + simulated leverage results as live PnL.
- Switch back to swap during current phase.
- Expand to altcoins.
- Let Codex run long backtests.

## 11. Current Next Step

Wait for R5.3 to finish on M4.

Then run:

```bash
cat logs/r5_3_spot_hour_refinement_validation_report.txt
```

and:

```bash
python - <<'PY'
import pandas as pd
df = pd.read_csv("logs/r5_3_spot_hour_refinement_validation_results.csv")
print(df.sort_values("score", ascending=False).to_string(index=False))
PY
```

Then classify the R5.3 result using the pre-committed decision framework.
