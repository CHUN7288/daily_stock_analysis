# 個人設定說明

## 環境設定

### LLM
- 使用 Gemini 2.5 Flash（免費方案）
- 環境變數：`GEMINI_API_KEY`、`GEMINI_MODEL=gemini-2.5-flash-preview-05-20`

### 數據源
- 美股行情：`FINNHUB_API_KEY`（免費方案，60 calls/min）
- 新聞：SearXNG 公共實例（不穩定，未來可補 Tavily）

### 通知
- Email：bill8888bill8888@gmail.com
- 環境變數：`EMAIL_SENDER`、`EMAIL_PASSWORD`、`EMAIL_RECEIVERS`

### 股票清單
```
AAPL,AMZN,ASML,BRK.B,CRWD,GOOG,LMND,META,MSFT,NBIS,NVDA,PLTR,TSLA,TSM,VT,WMT,GEV,CAT,ETN,HWM
```

### 報告設定
- 語言：繁體中文（`REPORT_LANGUAGE=zh`）
- 大盤複盤：美股（`MARKET_REVIEW_REGION=us`）
- 報告格式：精簡（`REPORT_TYPE=simple`）

## 排程

**GitHub Actions**（主要，電腦不需開著）
- 每週一 UTC 14:40 = 台北時間 22:40
- Workflow：`.github/workflows/00-daily-analysis.yml`
- cron：`40 14 * * 1`

**本機**（備用，需電腦開著）
```bash
cd /mnt/c/Users/User/Desktop/CHUN7288-Agent/daily_stock_analysis
.venv/bin/python main.py --force-run
```

## 程式碼修改紀錄

### src/market_analyzer.py
- 移除 US 大盤複盤硬寫英文的邏輯（`if self.region == "us": return "en"`）
- `_get_market_scope_name` 加入 US 市場的中文名稱（美股市场）
- 大盤複盤 prompt 加入「必須使用繁體中文輸出」指令

### src/analyzer.py
- `SYSTEM_PROMPT` 加入「請使用繁體中文輸出，禁止使用簡體中文」指令

## GitHub Actions Secrets 清單

| Secret | 說明 |
|--------|------|
| `GEMINI_API_KEY` | Gemini API Key |
| `STOCK_LIST` | 20 支美股清單 |
| `EMAIL_SENDER` | 寄件信箱 |
| `EMAIL_PASSWORD` | Gmail App Password |
| `EMAIL_RECEIVERS` | 收件信箱 |
| `FINNHUB_API_KEY` | 美股行情數據 |

## GitHub Actions Variables 清單

| Variable | 值 |
|----------|----|
| `GEMINI_MODEL` | `gemini-2.5-flash-preview-05-20` |
| `REPORT_LANGUAGE` | `zh` |
| `MARKET_REVIEW_REGION` | `us` |

## 手動觸發

GitHub → Actions → 每日股票分析 → Run workflow → 勾選 force_run → Run workflow
