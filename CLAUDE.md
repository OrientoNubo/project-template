# 項目結構說明

## 項目簡介

> （此處簡述本項目的目標、方法與範圍。）

## 項目更新記錄（≤500 words）

> （此處簡述本項目的目前的基本狀況。）

## 目錄結構

```
project_root/
├── papers/          # 外部參考論文
├── repos/           # 外部 clone 的 codebase，邏輯 read-only
├── weights/         # 模型權重，按項目分子目錄
│   └── ours/        # 本項目自身權重
├── data/            # 資料與處理文件
│   ├── datasets/    # datasets 存放處
│   ├── scripts/     # 通用資料處理／前處理腳本
│   └── testdata/    # 測試用小型資料（通常放自定義的測試資料）
├── docs/            # 文檔輸出統一存放處
│   ├── latex/       # 本項目 latex 文檔/論文撰寫（tex/bib/fig）
│   ├── plans/       # 存放開發計畫，含時間戳
│   └── reports/     # 開發報告與分析文件（須創建對應的子文件夾區分 不同repo/不同版本）
├── scripts/         # 運行／測試腳本（須創建對應的子文件夾分類）
├── configs/         # YAML／JSON 設定檔
├── src/             # 本項目核心 codebase（核心開發位置）
│   ├── v1_baseline/ # 開發版本（命名 vN_<簡述>），各版自帶 CHANGELOG.md／TODO.md
│   │   ├── CHANGELOG.md  # 本版本更新日志（創建新版本時從項目遷移記錄開始）
│   │   └── TODO.md       # 本版本待辦（創建新版本時從零開始）
│   └── current →    # symlink 指向當前激活版本
├── _tmp/            # 臨時輸出，臨時文件(s)必須創建對應的子文件夾(s)，不可直接在其目錄下存放文件，gitignored
├── _bak/            # 備份壓縮包，gitignored
├── CLAUDE.md        # claude code 配置
├── CHANGELOG.md     # 項目層級更新日志（版本層級見 src/<version>/）
├── README.md        # 項目 readme
├── TODO.md          # 項目層級待辦（版本層級見 src/<version>/）
└── LICENSE.md       # LICENSE
```

## 當前正在開發的 codebase

- **`src/current` → `src/v1_baseline`**
- 切換版本只改 symlink：`ln -snf <版本目錄> src/current`；scripts 一律用穩定路徑 `src/current/...`，並在此同步更新。

## 重要規則

- **`src/` 多版本共存**：各開發版本用子目錄 `vN_<簡述>`（如 `v1_baseline`、`v2_crossattn`），不互相覆蓋；各版本彼此隔離、獨立運作。同一 version key 串起相關產物：`weights/ours/<version>/`、`configs/<version>.yaml`、`docs/reports/<version>.md`。當前激活版本由 `src/current` symlink 指定（見上節）。
- **`repos/`**：邏輯上 read-only。允許為「成功運行」調整路徑／環境／依賴，但不得修改其運行邏輯與方法架構；如必須修改，先確認並備份。每個外部項目用一個子文件夾（`repos/AAA`）。
- **`weights/`**：按項目名分子目錄（`weights/ours`、`weights/AAA`）；訓練過程權重也存於對應子目錄。
- **`data/datasets/shared_datasets`**：symlink 指向共享 datasets。可下載／使用，但不得修改或刪除既有檔案；如必須，先確認並備份。
- **`_tmp/`**：本項目所有臨時文件放這裏，禁止放到系統根目錄的 `/tmp`。
- **`_bak/`**：備份壓縮包（權重、二進制大檔、整個項目快照等）存放處。
- **`CHANGELOG.md` / `TODO.md`（兩層級）**：根目錄為**項目層級**（跨版本、結構／基礎建設變更）；`src/<version>/` 各自有**版本層級**的 `CHANGELOG.md`／`TODO.md`，只記該版本自身的開發。格式統一 — CHANGELOG 時間戳分層（`## 2026-05-23-17:23:57`）按時間降序；TODO 用 `[ ]` 排列並記時間戳。

## Git 管理規範

- 必須納管：`configs`、`docs`、`scripts`、`src`、`CHANGELOG.md`、`CLAUDE.md`、`LICENSE.md`、`README.md`、`TODO.md`。
- `.gitignore` 明確排除：`repos/`、`data/`、`weights/`、`_tmp/`、`_bak/`。
- 其它文件／文件夾若需納管，需再確認。
