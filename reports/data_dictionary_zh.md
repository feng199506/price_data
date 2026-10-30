# 数据字典

本字典描述 `commodity_history` 已落地文件的字段。空值表示来源没有该字段，不是填了 0。

## 日线单值（EIA、FRED、月度参考）

| 字段 | 含义 |
| --- | --- |
| date | 观测日期，日历日，时区按来源（EIA/World Bank/IndexMundi 为日历日，无日内时间） |
| instrument | 内部品种键：gold / silver / copper / wti / brent |
| provider_symbol | 来源符号，如 RWTC、RBRTE、gold |
| price | 单值价格。EIA 现货不伪造 OHLC |
| currency | 报价货币，本次均为 USD |
| unit | 来源单位原文或核实后的单位 |
| source | EIA / IndexMundi / World Bank / FRED |
| instrument_type | official_spot / monthly_average_reference / official_spot_benchmark |
| benchmark_description | 页面或工作表给出的基准说明（若有） |
| originating_source | IndexMundi 页面标注的原始来源（World Bank） |

## OHLC（Dukascopy 原生 d1、原生 m5、UTC 日聚合）

| 字段 | 含义 |
| --- | --- |
| timestamp_utc | K 线开盘时刻，UTC，左闭区间 |
| instrument | 内部品种键 |
| provider_symbol | XAU/USD、XAG/USD、COPPER.CMD/USD、LIGHT.CMD/USD、BRENT.CMD/USD |
| open / high / low / close | 该根 K 线 OHLC |
| volume | Dukascopy `volume-units=units` 报价量，不是交易所成交量 |
| price_side | 本次默认 `bid` |
| source | Dukascopy 或 Dukascopy_m5_aggregated_utc_daily |
| instrument_type | dukascopy_spot_quote 或 cfd_proxy |
| n_input_obs | 聚合时实际输入观测数；原生 m5/d1 为空 |
| volume_semantics | `dukascopy_quote_volume_units` |

## 5 分钟边界与日界线

- 输出时区：UTC。
- 5 分钟：左闭左标 `[t, t+5min)`，例如 `[00:00, 00:05)`。
- 原生 d1：供应商日线开盘时间戳，**不是** UTC 自然日聚合。
- `*_utc_daily_from_m5`：把该 UTC 自然日内存活的 m5 根做 first/max/min/last。无行情日不生成平盘 K 线。
- 禁止把月度均价插值、前向填充或重复成日线。
- 禁止把现货、CFD、期货历史拼成一条统一序列。

## 质量标记

| 标记 | 含义 |
| --- | --- |
| pass | 文件可读、排序、无重复时间戳、无空/无穷；OHLC 不等式通过（若适用） |
| warn | 存在未知缺口、价格突变标记，或 EIA 负值等来源真实值；数据保留 |
| warn_ohlc_source | 供应商原生 OHLC 不满足 high>=max(open,close) 或 low<=min(open,close)；未删除、未平滑 |
| fail | 文件缺失、损坏或无法解析 |

## 完成状态

| 状态 | 含义 |
| --- | --- |
| completed | 请求目标已实际下载并通过检查（高频需覆盖三个完整日历年的可交易时段） |
| partial | 下到了数据，但覆盖短于请求/文档最早日期，或工具无法取出全部历史 |
| blocked | 该来源本次无法取得数据；其他品种/频率继续完成 |

