# 金融商品历史价格下载报告

- 生成时间（UTC）：2026-10-30T11:53:09Z
- 本报告只记录实际运行、实际下载和实际检查的结果。
- 数据**不是** point-in-time 版本库；没有供应商版本历史，不得当作 PIT 数据。
- 月度 IndexMundi 仅作为 `monthly_reference`，未插值、未前向填充、未复制成日线。
- 铜与原油的 Dukascopy 序列是 **CFD 代理行情**，不是 EIA 现货、LME 现货或交易所原始期货。
- 金银 Dukascopy 序列是供应商即期报价，不是 LBMA 官方定盘。
- 不同口径分别存储，未拼接成一条“统一历史”。

## 日期包含规则（已按文档/代码核实）

- **dukascopy_node**：Official filter is startDateMs <= timestamp < endDateMs. CLI --date-to / dates.to is exclusive. A call -from 2023-01-01 -to 2023-02-01 returns bars with open time in [2023-01-01T00:00:00Z, 2023-02-01T00:00:00Z).
- **eia_xls**：The XLS is a full published history snapshot, not a ranged query. The last data row is the last published trading day and is inclusive.
- **indexmundi**：Monthly labels are calendar months. Listed months are inclusive. This is monthly averages, never to be resampled into daily bars.
- **fred**：FRED CSV includes the DATE column on each observation day. The last dated row is inclusive. Missing values appear as a dot.

## 请求区间

- 高频默认：`[2023-01-01, 2026-01-01)`
- 日线截止：`2026-10-30`（dukascopy-node 的 `--date-to` 为开区间终点）

## 总表

| 品种 | 产品属性 | 来源 | 频率 | 请求区间 | 实际覆盖 | 行数 | 单位 | 文件路径 | 质量结果 | 完成状态 |
| --- | --- | --- | --- | --- | --- | ---: | --- | --- | --- | --- |
| WTI原油 | official_spot | EIA | daily | [earliest available, 2026-10-30) (full published file, no ranged query) | 1986-01-02 .. 2026-10-27 | 9532 | USD per barrel | `/root/workspace/commodity_history/data/daily/wti_eia_spot_daily.csv` | warn | completed |
| Brent原油 | official_spot | EIA | daily | [earliest available, 2026-10-30) (full published file, no ranged query) | 1987-05-20 .. 2026-10-27 | 9117 | USD per barrel | `/root/workspace/commodity_history/data/daily/brent_eia_spot_daily.csv` | warn | completed |
| 黄金 | monthly_average_reference | IndexMundi | monthly | longest monthly history on IndexMundi page | 1996-09-01 .. 2026-03-01 | 355 | US Dollars per Troy Ounce | `/root/workspace/commodity_history/data/monthly_reference/gold_indexmundi_monthly.csv` | pass | completed |
| 白银 | monthly_average_reference | IndexMundi | monthly | longest monthly history on IndexMundi page | 1996-09-01 .. 2026-03-01 | 355 | US Dollars per Troy Ounce | `/root/workspace/commodity_history/data/monthly_reference/silver_indexmundi_monthly.csv` | pass | completed |
| 铜 | monthly_average_reference | IndexMundi | monthly | longest monthly history on IndexMundi page | 1996-09-01 .. 2026-03-01 | 355 | US Dollars per Metric Ton | `/root/workspace/commodity_history/data/monthly_reference/copper_indexmundi_monthly.csv` | pass | completed |
| 黄金 | monthly_average_reference | World Bank | monthly | full pink-sheet monthly history | 1960-01-01 .. 2024-12-01 | 780 | ($/troy oz) | `/root/workspace/commodity_history/data/monthly_reference/gold_worldbank_pinksheet_monthly.csv` | pass | completed |
| 白银 | monthly_average_reference | World Bank | monthly | full pink-sheet monthly history | 1960-01-01 .. 2024-12-01 | 780 | ($/troy oz) | `/root/workspace/commodity_history/data/monthly_reference/silver_worldbank_pinksheet_monthly.csv` | pass | completed |
| 铜 | monthly_average_reference | World Bank | monthly | full pink-sheet monthly history | 1960-01-01 .. 2024-12-01 | 780 | ($/mt) | `/root/workspace/commodity_history/data/monthly_reference/copper_worldbank_pinksheet_monthly.csv` | pass | completed |
| 黄金 | official_spot_benchmark | FRED | daily | [earliest available, 2026-10-30) |  |  |  | `` |  | blocked |
| 白银 | official_spot_benchmark | FRED | daily | [earliest available, 2026-10-30) |  |  |  | `` |  | blocked |
| 黄金 | dukascopy_spot_quote | Dukascopy | daily native d1 | [1999-06-03, 2026-10-30) | 1999-06-03 00:00:00+00:00 .. 2026-10-29 00:00:00+00:00 | 8312 | USD per troy ounce | `/root/workspace/commodity_history/data/daily/gold_dukascopy_native_d1.csv` | warn_ohlc_source | completed |
| 白银 | dukascopy_spot_quote | Dukascopy | daily native d1 | [1999-06-03, 2026-10-30) | 1999-06-03 00:00:00+00:00 .. 2026-10-29 00:00:00+00:00 | 8299 | USD per troy ounce | `/root/workspace/commodity_history/data/daily/silver_dukascopy_native_d1.csv` | warn_ohlc_source | completed |
| 铜 | cfd_proxy | Dukascopy | daily native d1 | [2012-03-02, 2026-10-30) | 2012-03-04 00:00:00+00:00 .. 2026-10-29 00:00:00+00:00 | 4550 | USD per pound | `/root/workspace/commodity_history/data/daily/copper_dukascopy_native_d1.csv` | warn | completed |
| Brent原油 | cfd_proxy | Dukascopy | daily native d1 | [2006-11-17, 2026-10-30) | 2010-12-02 00:00:00+00:00 .. 2026-10-29 00:00:00+00:00 | 4059 | USD per barrel | `/root/workspace/commodity_history/data/daily/brent_dukascopy_native_d1.csv` | warn_ohlc_source | partial |
| WTI原油 | cfd_proxy | Dukascopy | daily native d1 | [1983-04-20, 2026-10-30) | 1992-12-31 00:00:00+00:00 .. 2026-10-29 00:00:00+00:00 | 9271 | USD per barrel | `/root/workspace/commodity_history/data/daily/wti_dukascopy_native_d1.csv` | warn_ohlc_source | partial |
| 黄金 | dukascopy_spot_quote | Dukascopy | 5-minute native m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 212736 | USD per troy ounce | `/root/workspace/commodity_history/data/intraday_5m/gold/dukascopy/gold_m5_bid_all.csv` | warn | completed |
| 黄金 | dukascopy_spot_quote | Dukascopy | utc_daily_from_m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 934 | USD per troy ounce | `/root/workspace/commodity_history/data/daily/gold_dukascopy_utc_daily_from_m5.csv` | warn | completed |
| 白银 | dukascopy_spot_quote | Dukascopy | 5-minute native m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 212634 | USD per troy ounce | `/root/workspace/commodity_history/data/intraday_5m/silver/dukascopy/silver_m5_bid_all.csv` | warn | completed |
| 白银 | dukascopy_spot_quote | Dukascopy | utc_daily_from_m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 934 | USD per troy ounce | `/root/workspace/commodity_history/data/daily/silver_dukascopy_utc_daily_from_m5.csv` | warn | completed |
| 铜 | cfd_proxy | Dukascopy | 5-minute native m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 212118 | USD per pound | `/root/workspace/commodity_history/data/intraday_5m/copper/dukascopy/copper_m5_bid_all.csv` | warn | completed |
| 铜 | cfd_proxy | Dukascopy | utc_daily_from_m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 934 | USD per pound | `/root/workspace/commodity_history/data/daily/copper_dukascopy_utc_daily_from_m5.csv` | warn | completed |
| WTI原油 | cfd_proxy | Dukascopy | 5-minute native m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 212410 | USD per barrel | `/root/workspace/commodity_history/data/intraday_5m/wti/dukascopy/wti_m5_bid_all.csv` | warn | completed |
| WTI原油 | cfd_proxy | Dukascopy | utc_daily_from_m5 | [2023-01-01, 2026-01-01) | 2023-01-02 23:00:00+00:00 .. 2025-12-31 21:55:00+00:00 | 934 | USD per barrel | `/root/workspace/commodity_history/data/daily/wti_dukascopy_utc_daily_from_m5.csv` | warn | completed |
| Brent原油 | cfd_proxy | Dukascopy | 5-minute native m5 | [2023-01-01, 2026-01-01) | 2023-01-03 01:00:00+00:00 .. 2025-12-31 19:25:00+00:00 | 193864 | USD per barrel | `/root/workspace/commodity_history/data/intraday_5m/brent/dukascopy/brent_m5_bid_all.csv` | warn | completed |
| Brent原油 | cfd_proxy | Dukascopy | utc_daily_from_m5 | [2023-01-01, 2026-01-01) | 2023-01-03 01:00:00+00:00 .. 2025-12-31 19:25:00+00:00 | 774 | USD per barrel | `/root/workspace/commodity_history/data/daily/brent_dukascopy_utc_daily_from_m5.csv` | warn | completed |

## 小样本验证

```json
{
  "downloads": [
    {
      "ok": true,
      "skipped": false,
      "instrument": "gold",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/gold_xauusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.16
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "gold",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/gold_xauusd_m1_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m1",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.18
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "silver",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/silver_xagusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.35
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "silver",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/silver_xagusd_m1_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m1",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.34
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "copper",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/copper_coppercmdusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.29
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "copper",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/copper_coppercmdusd_m1_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m1",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.3
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "wti",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/wti_lightcmdusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.29
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "wti",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/wti_lightcmdusd_m1_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m1",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.3
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "brent",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/brent_brentcmdusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.29
      }
    },
    {
      "ok": true,
      "skipped": false,
      "instrument": "brent",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/brent_brentcmdusd_m1_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m1",
      "price_side": "bid",
      "cli": {
        "returncode": 0,
        "seconds": 0.28
      }
    }
  ],
  "m1_vs_m5": [
    {
      "instrument": "gold",
      "ok": true,
      "n_m1": 6838,
      "n_m5_native": 1368,
      "n_m5_from_m1": 1368,
      "n_overlap": 1368,
      "max_abs_close_diff": 0.0,
      "median_abs_close_diff": 0.0,
      "m1_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/gold_xauusd_m1_bid_2024-03-04_2024-03-09.csv",
      "m5_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/gold_xauusd_m5_bid_2024-03-04_2024-03-09.csv"
    },
    {
      "instrument": "silver",
      "ok": true,
      "n_m1": 6776,
      "n_m5_native": 1368,
      "n_m5_from_m1": 1368,
      "n_overlap": 1368,
      "max_abs_close_diff": 0.0,
      "median_abs_close_diff": 0.0,
      "m1_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/silver_xagusd_m1_bid_2024-03-04_2024-03-09.csv",
      "m5_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/silver_xagusd_m5_bid_2024-03-04_2024-03-09.csv"
    },
    {
      "instrument": "copper",
      "ok": true,
      "n_m1": 6176,
      "n_m5_native": 1349,
      "n_m5_from_m1": 1349,
      "n_overlap": 1349,
      "max_abs_close_diff": 0.0,
      "median_abs_close_diff": 0.0,
      "m1_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/copper_coppercmdusd_m1_bid_2024-03-04_2024-03-09.csv",
      "m5_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/copper_coppercmdusd_m5_bid_2024-03-04_2024-03-09.csv"
    },
    {
      "instrument": "wti",
      "ok": true,
      "n_m1": 6704,
      "n_m5_native": 1368,
      "n_m5_from_m1": 1368,
      "n_overlap": 1368,
      "max_abs_close_diff": 0.0,
      "median_abs_close_diff": 0.0,
      "m1_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/wti_lightcmdusd_m1_bid_2024-03-04_2024-03-09.csv",
      "m5_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/wti_lightcmdusd_m5_bid_2024-03-04_2024-03-09.csv"
    },
    {
      "instrument": "brent",
      "ok": true,
      "n_m1": 6147,
      "n_m5_native": 1260,
      "n_m5_from_m1": 1260,
      "n_overlap": 1260,
      "max_abs_close_diff": 0.0,
      "median_abs_close_diff": 0.0,
      "m1_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m1/brent_brentcmdusd_m1_bid_2024-03-04_2024-03-09.csv",
      "m5_path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/brent_brentcmdusd_m5_bid_2024-03-04_2024-03-09.csv"
    }
  ]
}
```

## 断点续传

```json
{
  "n_probed": 5,
  "n_skipped": 5,
  "all_skipped": true,
  "details": [
    {
      "ok": true,
      "skipped": true,
      "instrument": "gold",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/gold_xauusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid"
    },
    {
      "ok": true,
      "skipped": true,
      "instrument": "silver",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/silver_xagusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid"
    },
    {
      "ok": true,
      "skipped": true,
      "instrument": "copper",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/copper_coppercmdusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid"
    },
    {
      "ok": true,
      "skipped": true,
      "instrument": "wti",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/wti_lightcmdusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid"
    },
    {
      "ok": true,
      "skipped": true,
      "instrument": "brent",
      "path": "/root/workspace/commodity_history/data/raw/dukascopy/sample/m5/brent_brentcmdusd_m5_bid_2024-03-04_2024-03-09.csv",
      "from": "2024-03-04",
      "to": "2024-03-09",
      "timeframe": "m5",
      "price_side": "bid"
    }
  ]
}
```

## 质量检查摘要

- 质量表：`/root/workspace/commodity_history/reports/quality_report.csv`
- 缺口表：`/root/workspace/commodity_history/reports/gaps.csv`
- 价格突变只标记、不删除、不平滑。
- 不以 price>0 作为原油强制规则；EIA WTI 在 2020-04-20 允许为负（若来源确有记录）。
- 成交量为 Dukascopy 报价量（units），不是交易所成交量；缺失则为空。
- 无行情区间不生成虚假平盘 K 线，不补齐每天 288 根。
- 交易时段无法核实的缺口标记为 `unknown`，不一律当成休市。

## 抽查

每个已成功数据集从自身文件中抽取最多 5 个时点，供与来源对照。无法独立打开供应商网页核验的，验证范围仅限本地下载文件。

```json
[
  {
    "instrument": "wti",
    "kind": "wti_eia_daily",
    "path": "/root/workspace/commodity_history/data/daily/wti_eia_spot_daily.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1986-01-02",
        "price": 25.56,
        "instrument": "wti"
      },
      {
        "date": "1995-12-18",
        "price": 19.71,
        "instrument": "wti"
      },
      {
        "date": "2007-08-13",
        "price": 71.6,
        "instrument": "wti"
      },
      {
        "date": "2017-04-18",
        "price": 52.46,
        "instrument": "wti"
      },
      {
        "date": "2026-10-27",
        "price": 103.37,
        "instrument": "wti"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "brent",
    "kind": "brent_eia_daily",
    "path": "/root/workspace/commodity_history/data/daily/brent_eia_spot_daily.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1987-05-20",
        "price": 18.63,
        "instrument": "brent"
      },
      {
        "date": "1998-02-03",
        "price": 14.87,
        "instrument": "brent"
      },
      {
        "date": "2008-10-17",
        "price": 66.05,
        "instrument": "brent"
      },
      {
        "date": "2017-10-26",
        "price": 58.75,
        "instrument": "brent"
      },
      {
        "date": "2026-10-27",
        "price": 123.1,
        "instrument": "brent"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "gold",
    "kind": "gold_indexmundi_monthly",
    "path": "/root/workspace/commodity_history/data/monthly_reference/gold_indexmundi_monthly.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1996-09-01",
        "price": 383.14,
        "instrument": "gold"
      },
      {
        "date": "2004-01-01",
        "price": 413.79,
        "instrument": "gold"
      },
      {
        "date": "2011-06-01",
        "price": 1529.36,
        "instrument": "gold"
      },
      {
        "date": "2018-11-01",
        "price": 1220.65,
        "instrument": "gold"
      },
      {
        "date": "2026-03-01",
        "price": 4855.54,
        "instrument": "gold"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "gold",
    "kind": "gold_worldbank_monthly",
    "path": "/root/workspace/commodity_history/data/monthly_reference/gold_worldbank_pinksheet_monthly.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1960-01-01",
        "price": 35.27,
        "instrument": "gold"
      },
      {
        "date": "1976-04-01",
        "price": 127.94,
        "instrument": "gold"
      },
      {
        "date": "1992-07-01",
        "price": 352.72,
        "instrument": "gold"
      },
      {
        "date": "2008-10-01",
        "price": 806.62,
        "instrument": "gold"
      },
      {
        "date": "2024-12-01",
        "price": 2648.01,
        "instrument": "gold"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "silver",
    "kind": "silver_indexmundi_monthly",
    "path": "/root/workspace/commodity_history/data/monthly_reference/silver_indexmundi_monthly.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1996-09-01",
        "price": 5.04,
        "instrument": "silver"
      },
      {
        "date": "2004-01-01",
        "price": 6.3,
        "instrument": "silver"
      },
      {
        "date": "2011-06-01",
        "price": 35.8,
        "instrument": "silver"
      },
      {
        "date": "2018-11-01",
        "price": 14.35,
        "instrument": "silver"
      },
      {
        "date": "2026-03-01",
        "price": 77.88,
        "instrument": "silver"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "silver",
    "kind": "silver_worldbank_monthly",
    "path": "/root/workspace/commodity_history/data/monthly_reference/silver_worldbank_pinksheet_monthly.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1960-01-01",
        "price": 0.9137,
        "instrument": "silver"
      },
      {
        "date": "1976-04-01",
        "price": 4.35552,
        "instrument": "silver"
      },
      {
        "date": "1992-07-01",
        "price": 3.9675,
        "instrument": "silver"
      },
      {
        "date": "2008-10-01",
        "price": 10.4413,
        "instrument": "silver"
      },
      {
        "date": "2024-12-01",
        "price": 30.764,
        "instrument": "silver"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "copper",
    "kind": "copper_indexmundi_monthly",
    "path": "/root/workspace/commodity_history/data/monthly_reference/copper_indexmundi_monthly.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1996-09-01",
        "price": 1941.45,
        "instrument": "copper"
      },
      {
        "date": "2004-01-01",
        "price": 2423.57,
        "instrument": "copper"
      },
      {
        "date": "2011-06-01",
        "price": 9066.85,
        "instrument": "copper"
      },
      {
        "date": "2018-11-01",
        "price": 6195.92,
        "instrument": "copper"
      },
      {
        "date": "2026-03-01",
        "price": 12528.71,
        "instrument": "copper"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "copper",
    "kind": "copper_worldbank_monthly",
    "path": "/root/workspace/commodity_history/data/monthly_reference/copper_worldbank_pinksheet_monthly.csv",
    "n_checked": 5,
    "rows": [
      {
        "date": "1960-01-01",
        "price": 715.4,
        "instrument": "copper"
      },
      {
        "date": "1976-04-01",
        "price": 1510.0,
        "instrument": "copper"
      },
      {
        "date": "1992-07-01",
        "price": 2520.32,
        "instrument": "copper"
      },
      {
        "date": "2008-10-01",
        "price": 4925.696,
        "instrument": "copper"
      },
      {
        "date": "2024-12-01",
        "price": 8916.32,
        "instrument": "copper"
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "gold",
    "kind": "gold_dukascopy_d1",
    "path": "/root/workspace/commodity_history/data/daily/gold_dukascopy_native_d1.csv",
    "n_checked": 5,
    "rows": [
      {
        "timestamp_utc": "1999-06-03 00:00:00+00:00",
        "instrument": "gold",
        "open": 265.05,
        "high": 266.7,
        "low": 263.75,
        "close": 265.7,
        "volume": 1978867200
      },
      {
        "timestamp_utc": "2006-10-27 00:00:00+00:00",
        "instrument": "gold",
        "open": 594.788,
        "high": 599.794,
        "low": 591.0,
        "close": 598.831,
        "volume": 26299562
      },
      {
        "timestamp_utc": "2013-06-19 00:00:00+00:00",
        "instrument": "gold",
        "open": 1368.894,
        "high": 1376.553,
        "low": 1342.727,
        "close": 1346.278,
        "volume": 54552840
      },
      {
        "timestamp_utc": "2020-02-24 00:00:00+00:00",
        "instrument": "gold",
        "open": 1665.475,
        "high": 1689.255,
        "low": 1650.838,
        "close": 1653.825,
        "volume": 116475720
      },
      {
        "timestamp_utc": "2026-10-29 00:00:00+00:00",
        "instrument": "gold",
        "open": 4054.425,
        "high": 4068.604,
        "low": 4005.145,
        "close": 4007.664,
        "volume": 50605995
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "gold",
    "kind": "gold_utc_daily_from_m5",
    "path": "/root/workspace/commodity_history/data/daily/gold_dukascopy_utc_daily_from_m5.csv",
    "n_checked": 5,
    "rows": [
      {
        "timestamp_utc": "2023-01-02 00:00:00+00:00",
        "instrument": "gold",
        "open": 1826.837,
        "high": 1829.958,
        "low": 1823.287,
        "close": 1829.295,
        "volume": 1650560
      },
      {
        "timestamp_utc": "2023-10-02 00:00:00+00:00",
        "instrument": "gold",
        "open": 1848.295,
        "high": 1848.835,
        "low": 1820.405,
        "close": 1824.845,
        "volume": 17403885
      },
      {
        "timestamp_utc": "2024-07-04 00:00:00+00:00",
        "instrument": "gold",
        "open": 2358.725,
        "high": 2362.385,
        "low": 2350.364,
        "close": 2356.655,
        "volume": 25971430
      },
      {
        "timestamp_utc": "2025-04-02 00:00:00+00:00",
        "instrument": "gold",
        "open": 3114.998,
        "high": 3167.548,
        "low": 3104.175,
        "close": 3162.475,
        "volume": 45516857
      },
      {
        "timestamp_utc": "2025-12-31 00:00:00+00:00",
        "instrument": "gold",
        "open": 4338.655,
        "high": 4372.905,
        "low": 4273.805,
        "close": 4318.679,
        "volume": 65199280
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "gold",
    "kind": "gold_m5",
    "path": "/root/workspace/commodity_history/data/intraday_5m/gold/dukascopy/gold_m5_bid_all.csv",
    "n_checked": 5,
    "rows": [
      {
        "timestamp_utc": "2023-01-02 23:00:00+00:00",
        "instrument": "gold",
        "open": 1826.837,
        "high": 1827.357,
        "low": 1826.137,
        "close": 1826.737,
        "volume": 36310
      },
      {
        "timestamp_utc": "2023-10-02 07:30:00+00:00",
        "instrument": "gold",
        "open": 1842.845,
        "high": 1843.085,
        "low": 1842.325,
        "close": 1842.735,
        "volume": 71760
      },
      {
        "timestamp_utc": "2024-07-02 18:25:00+00:00",
        "instrument": "gold",
        "open": 2326.615,
        "high": 2327.395,
        "low": 2326.368,
        "close": 2326.515,
        "volume": 239710
      },
      {
        "timestamp_utc": "2025-04-02 07:55:00+00:00",
        "instrument": "gold",
        "open": 3123.935,
        "high": 3125.885,
        "low": 3123.445,
        "close": 3125.075,
        "volume": 153790
      },
      {
        "timestamp_utc": "2025-12-31 21:55:00+00:00",
        "instrument": "gold",
        "open": 4317.095,
        "high": 4318.719,
        "low": 4316.525,
        "close": 4318.679,
        "volume": 127090
      }
    ],
    "note": "Values copied from downloaded files for manual source comparison; not independently re-fetched here."
  },
  {
    "instrument": "silver",
    "kind": "silver_dukascopy_d1",
    "path": "/root/workspace/commodity_history/data/daily/silver_dukascopy_native_d1.csv",
    "n_checked": 5,
    "rows": [
      {
        "timestamp_utc": "1999-06-03 00:00:00+00:00",
        "instrument": "silver",
        "open": 4.96,
        "high": 4.99,
        "low": 4.94,
        "close": 4.93,
        "volume": 156750000
      },
      {
        "timestamp_utc": "2006-11-06 00:00:00+00:00",
        "instrument": "silver",
        "open": 12.503,
        "high": 12.782,
        "low": 12.281,
        "close": 12.557,
        "volume": 546200000
      },
      {
        "timestamp_utc": "2013-06-25 00:00:00+00:00",
        "instrument": "silver",
        "open": 19.695,
        "high": 19.85,
        "low": 19.463,
        "close": 19.554,
        "volume": 971326000
      },
      {
        "timesta
```

## 与来源对照 / 跨来源数量级核查

现货与 CFD、不同截点之间不要求逐值相同。以下为本次实际对照。

```json
[
  {
    "dataset": "brent_eia",
    "date": "1987-05-20",
    "source_page": "https://www.eia.gov/dnav/pet/hist/RBRTEd.htm",
    "expected": 18.63,
    "downloaded": 18.63,
    "match": true
  },
  {
    "dataset": "brent_eia",
    "date": "1987-05-21",
    "source_page": "https://www.eia.gov/dnav/pet/hist/RBRTEd.htm",
    "expected": 18.45,
    "downloaded": 18.45,
    "match": true
  },
  {
    "dataset": "brent_eia",
    "date": "1987-05-22",
    "source_page": "https://www.eia.gov/dnav/pet/hist/RBRTEd.htm",
    "expected": 18.55,
    "downloaded": 18.55,
    "match": true
  },
  {
    "dataset": "brent_eia",
    "date": "1987-05-26",
    "source_page": "https://www.eia.gov/dnav/pet/hist/RBRTEd.htm",
    "expected": 18.63,
    "downloaded": 18.63,
    "match": true
  },
  {
    "dataset": "wti_eia",
    "date": "2020-04-20",
    "note": "Cushing WTI printed negative on the April 2020 futures expiry; kept as source value.",
    "downloaded": -36.98,
    "negative_kept": true
  },
  {
    "dataset": "gold_indexmundi_vs_worldbank",
    "n_overlap": 340,
    "max_abs_diff": 0.0049999999999954525,
    "median_abs_diff": 0.0,
    "note": "Magnitude/level check only. IndexMundi cites World Bank; not required to match tick-for-tick after rounding."
  },
  {
    "dataset": "gold_dukascopy_m5_sample",
    "timestamp_utc": "2024-03-04T00:00:00.000Z",
    "open": 2083.445,
    "note": "2024-03-04 gold ~2080 USD/oz is the expected order of magnitude for Dukascopy spot gold, not LBMA fixing.",
    "magnitude_ok": true
  }
]
```

## 受阻项与下一步

### 黄金 / FRED / daily — `blocked`

- 证据：HTTP 404; url=https://fred.stlouisfed.org/graph/fredgraph.csv?id=GOLDPMGBD228NLBM; HTTP 404; content_type=text/html; charset=UTF-8; sniff=html。独立官方/基准日线未获得，未与 Dukascopy 拼接。 下一步：换仍公开的 FRED 日频序列 ID，或取得 LBMA 授权后下载定盘。
- 路径：`None`

### 白银 / FRED / daily — `blocked`

- 证据：HTTP 404; url=https://fred.stlouisfed.org/graph/fredgraph.csv?id=SLVPRUSD; HTTP 404; content_type=text/html; charset=UTF-8; sniff=html。独立官方/基准日线未获得，未与 Dukascopy 拼接。 下一步：换仍公开的 FRED 日频序列 ID，或取得 LBMA 授权后下载定盘。
- 路径：`None`

### Brent原油 / Dukascopy / daily native d1 — `partial`

- 证据：Dukascopy UK Brent Oil CFD continuous broker quote. Not EIA Europe Brent spot and not ICE raw futures. 文档/请求最早 2006-11-17，实际首根 2010-12-02（晚 1476 天）。 dukascopy-node 1.51.1 crashes with Cannot read properties of undefined (reading '0') when -from 2006-11-17 or 2007-01-02. First working d1 fetch is 2010-12-02.
- 路径：`/root/workspace/commodity_history/data/daily/brent_dukascopy_native_d1.csv`

### WTI原油 / Dukascopy / daily native d1 — `partial`

- 证据：Dukascopy US Light Crude Oil CFD continuous broker quote. Not EIA Cushing WTI spot and not NYMEX raw futures. 文档/请求最早 1983-04-20，实际首根 1992-12-31（晚 3543 天）。 供应商工具返回的文件从该日起才有观测，未用其他口径回填。
- 路径：`/root/workspace/commodity_history/data/daily/wti_dukascopy_native_d1.csv`

### 其他已核实但未作为主数据集的阻塞

- **IndexMundi Excel 导出**：页面链接 `/commodities/?commodity=gold&months=360&format=excel` 实际 302/跳转到 `https://www.indexmundi.com/members/login.aspx?ReturnUrl=%2fcommodities%2f%3fcommodity%3dgold%26months%3d360`，标题 Sign In。未登录、未购买。公开路径改用 FusionCharts XML（`months=360`，355 个月）。更长月度改走 World Bank Pink Sheet。
- **FRED GOLDAMGBD228NLBM**：`https://fred.stlouisfed.org/graph/fredgraph.csv?id=GOLDAMGBD228NLBM` HTTP 404，`text/html`。
- **FRED IQ12260**：CSV 可下载，但是月度序列（observation_date 为月初），不是金属每日现货，因此不作为 daily 数据集保存。
- **LBMA JSON**：`https://prices.lbma.org.uk/json/gold_pm.json` HTTP 403 HTML。下一步需官方授权或许可数据产品，本次不自动购买。
- **Brent Dukascopy 原生 d1 2006-11-17**：`dukascopy-node` 1.51.1 报 `Cannot read properties of undefined (reading '0')`。2007-01-02 同样失败。2010-12-02 起可下载。下一步：供应商修复历史分片，或换其他授权源补 2006–2010。
- **WTI Dukascopy 原生 d1 1983-04-20**：工具接受该起始参数，但文件首根为 1992-12-31。未用 EIA 现货回填 CFD 缺口。

## 数据字典

完整字段说明见 `reports/data_dictionary_zh.md`，README 中也有摘要。

## 依赖版本

- dukascopy-node：1.51.1
- Node.js：本机实测 v20.18.3（仓库 `.tools/node`）
- Python 包见 `requirements.txt`

