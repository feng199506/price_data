# 金融商品历史价格数据

本仓库只存放**已下载并检查过的数据**和说明，不含下载脚本、虚拟环境和 Dukascopy 缓存。

完整中文报告与字段定义：

- [下载报告](reports/download_report_zh.md)
- [数据字典](reports/data_dictionary_zh.md)
- [质量表](reports/quality_report.csv)
- [来源清单](metadata/sources.csv)
- [文件清单（含 SHA256）](metadata/manifest.csv)

下载时使用的区间与品种映射见 `config.yaml`。

## 口径（必读）

| 数据集 | 产品属性 | 说明 |
| --- | --- | --- |
| EIA WTI / Brent 日线 | 官方现货 | 单值 `price`，不伪造 OHLC。单位 USD/barrel |
| IndexMundi / World Bank 金银铜 | 月度均价参考 | 仅 `data/monthly_reference/`，未插值成日线 |
| Dukascopy XAU/USD、XAG/USD | 供应商即期报价 | 不是 LBMA 定盘 |
| Dukascopy COPPER.CMD/USD、LIGHT.CMD/USD、BRENT.CMD/USD | 连续 CFD 代理 | **不是** LME 现货、EIA 现货或交易所原始期货 |

不同口径分开存放，没有拼成一条“统一历史”。这些文件**不是** point-in-time 数据。

## 目录

```
data/daily/                 日线（EIA 现货、Dukascopy 原生 d1、由 5 分钟聚合的 UTC 日线）
data/intraday_5m/           5 分钟 bid OHLC，按品种/来源/年份分区（CSV + Parquet）
data/monthly_reference/     IndexMundi 与 World Bank 月度
data/raw/                   EIA XLS、World Bank Pink Sheet、IndexMundi HTML 原文
reports/                    中文下载报告、质量检查、抽查
metadata/                   来源与文件清单
```

高频默认请求区间（执行年 2026）：`[2023-01-01, 2026-01-01)`。  
`dukascopy-node` 的结束日期为**开区间**；EIA XLS 最后交易日**含当日**。  
5 分钟 K 线为 UTC 左闭左标 `[t, t+5min)`，`price_side=bid`。成交量为供应商报价量，不是交易所成交量。

## 完成状态摘要

- EIA WTI / Brent 日线：completed
- IndexMundi / World Bank 月度：completed
- 五品种 Dukascopy 5 分钟（2023–2025 可交易时段）：completed
- FRED 金银官方日线：blocked（系列 CSV 返回 HTTP 404）
- WTI / Brent Dukascopy 原生 d1：partial（实际起点晚于文档最早日期，未用现货回填）

细节、错误证据和下一步见 [下载报告](reports/download_report_zh.md)。
