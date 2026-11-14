# BTC 4h median forecast

Point forecast of BTC/USDC 240 minutes after each checkpoint.

## Model

`forecast = last_price * exp(0.5 * median(240-min log-returns))`

The median runs over the 240-minute log-returns that start at each
5-minute sample inside the last 720 minutes of 1-minute closes. Each
return spans the 240 minutes after its start, so every return is fully
inside the observed history. The 0.5 shrink pulls the median drift
halfway back toward zero (persistence).

Constants: `HORIZON_MINUTES=240`, `LOOKBACK_MINUTES=720`,
`SAMPLE_STEP_MINUTES=5`, `SHRINK=0.5`.

## Practice score (official-style, p00–p04 vs next case's last price)

- Model MAE: **42,376 cents**
- Persistence baseline MAE: **47,055 cents**

## Run

The entry reads every `*.in` file in `$MAKEFUN_INPUT_DIR` (default
`/input`) and writes one `$MAKEFUN_OUTPUT_DIR/<case>.out` per case with
`{"forecasts": [{"case": <id>, "price": <guess>}]}`.

```sh
MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```
