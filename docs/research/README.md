# crypto_trading_system Research Notes

This folder contains research context, decision frameworks, and AI-review summaries for the OKX crypto trading bot project.

Current active document:

- [`PROJECT_CONTEXT_R5_3.md`](PROJECT_CONTEXT_R5_3.md)

Important current rule:

R5.3 is the final in-sample hour-refinement stage. After R5.3, any winner must be frozen and tested through R5.4 sealed holdout before any further consideration.

Current hard constraints:

- Research only.
- BTC/USDT spot only for the current phase.
- Ignore BTC/USDT:USDT swap/perpetual results for now.
- No OKX private endpoints.
- No API keys.
- No paper live or live trading.
- Current spot OHLCV + simulated leverage results are signal research only, not live OKX execution PnL.
