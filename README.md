# 🛡️ HCFD ISO Platform — Aitest (事故安全官作業平台測試版)

[![GitHub Pages](https://img.shields.io/badge/Deployed-GitHub%20Pages-success.svg)](https://fc861117-sketch.github.io/Aitest/)
[![HTML5](https://img.shields.io/badge/Tech-HTML5-orange.svg)]()
[![Jekyll](https://img.shields.io/badge/Jekyll-Page%20Builder-blue.svg)]()

## 📖 專案簡介 (About)

**HCFD 事故安全官作業平台 (Incident Safety Officer Platform)** 是一個專為消防與救災現場「事故安全官 (ISO)」與「指揮官 (IC)」開發的輔助決策系統。

此版本為 **Aitest 修正與測試分支**，在保留原始平台所有版面美學與核心邏輯的前提下，針對系統安全性、瀏覽器相容性、現場實用 UX 及資料持久化進行了全面性的調校與 Bug 修復。

🔗 **[點此訪問平台網站 (Aitest Live Demo)](https://fc861117-sketch.github.io/Aitest/)**

---

## ✨ 核心功能模組 (Core Features)

本平台整合了以下四大核心作業模組，協助安全官於災害現場進行全方位監控：

1. **☣️ Hazmat (化災圖資檢索)**：提供常用化學物質與毒災之處置指引、警戒距離及 GHS 危害分類查詢。
2. **📋 ISO Guide (安全官作業指南)**：提供現場安全官之標準作業檢核表，記錄管制、通訊及復原事項。
3. **🌦️ 環境氣候監控及 NOAA**：對接大氣氣象背景 API，即時運算現場熱危害指標與行動建議。
4. **📊 風險決策模型**：提供 SPE 風險評估得分、VTS 戰術決策建議及 5x5 風險矩陣。

---

## 🛠️ 修正與優化項目 (Changelog & Optimizations)

與原始版本相比，Aitest 進行了以下 **10 項核心優化與修正**：

### 🔴 系統 Bug 與防呆修正
1. **Firefox 頁籤切換失效修正** (`Hazmat/index.html`)
   * **問題**：原代碼使用隱式全域 `event.target` 物件，在 Firefox 瀏覽器中會因為相容性問題導致切換分頁無反應。
   * **修正**：在 HTML 的 onclick 中明確傳入 `this` 對象：`switchTab('tab-id', this)`，以求完全符合標準 DOM 規範，支援所有主流瀏覽器。
2. **GHS 分類空值防呆 (Crash Prevention)** (`Hazmat/index.html`)
   * **問題**：當搜尋到 GHS 資料不齊全的化學物質時，陣列處理會因調用 `split()` 失敗而導致整個 JS 崩潰阻斷。
   * **修正**：採用 Optional Chaining 防呆 `(item.ghs?.[0] || '').split(',')`。
3. **Jekyll 專案子目錄路徑修正** (`_config.yml`)
   * **問題**：原 `baseurl` 為 `"/"`，導致部署於 GitHub Pages 子路徑時，網頁選單跳轉會指向根網域而發生 404 錯誤。
   * **修正**：將 baseurl 修改為 `"/Aitest"`，使所有的 `{{ site.baseurl }}` 模板變數能自動編譯為正確的專案相對路徑。
4. **HTML 未閉合標籤清理** (`環境氣候監控及NOAA/index.html`)
   * **修正**：清理並閉合了免責聲明區塊前未對稱的 `</div>` 標籤，解決在部分螢幕尺寸下排版跑版的問題。

### 🟡 使用者體驗 (UX) 與實用性優化
5. **MEDIC 紀錄時間自動填入（保留手動修改）** (`index.html`)
   * **優化**：現場安全官切換至 MEDIC 事件紀錄分頁時，系統會自動在 `m_time` 欄位填入當前的「時:分」，節省黃金救災時間。同時，該欄位仍**保留手動點擊微調**的彈性。
6. **ISO Guide 勾選欄位狀態持久化** (`ISO guide/index.html`)
   * **優化**：為 checklist 中所有 checkbox 補齊唯一 ID，並與 `localStorage` 連動。無論是切換頁籤、瀏覽器重整，或是手機螢幕暫時休眠，所有已核取的安全檢核狀態皆會自動保存。
7. **導覽易用性優化 — 返回主平台按鈕** (`ISO guide/index.html`)
   * **優化**：在指南頂部加入一鍵「← 返回主平台」的固定按鈕，方便安全官完成檢核後隨時切換回主系統。
8. **Android 羅盤感測器超時溫馨提示** (`index.html`)
   * **優化**：部分 Android 裝置之磁力計在非 HTTPS 或權限受限時會無回應卡死。系統加入了 5 秒超時計時器，超時未收到訊號會主動提醒使用者進行感測器校準或改用手動選擇風向。
9. **風險矩陣軸標籤標記** (`風險決策模型/index.html`)
   * **優化**：在 5x5 風險矩陣的格子上側與左側動態寫入 `P1~P5`（機率軸）與 `S1~S5`（嚴重度軸）標記，使矩陣座標更加直觀易懂。
10. **雲端專案名稱 XSS 弱點防護** (`index.html`)
    * **優化**：在透過 API 渲染獲取到的專案列表名稱時，針對名稱進行了 HTML 特殊字元轉義處理，阻斷潛在的腳本注入弱點。

---

## 🚀 本地測試與執行 (Usage)

本系統完全基於靜態網頁技術開發，支援跨平台即時開啟：

1. **直接訪問線上 Demo**：開啟 [Aitest 部署網址](https://fc861117-sketch.github.io/Aitest/)。
2. **本地執行**：
   ```bash
   # Clone 本專案
   git clone https://github.com/fc861117-sketch/Aitest.git
   ```
   * **本機靜態預覽**：可直接雙擊打開 `index.html`（部分依賴 HTTPS 的功能如 GPS 定位可能會受到瀏覽器安全性限制）。
   * **Jekyll 本地伺服器預覽**（推薦，可完整編譯選單）：
     ```bash
     bundle exec jekyll serve
     ```

---

## ⚠️ 免責聲明 (Disclaimer)

本平台提供的氣候分析、GHS 化災處置距離、風險評估矩陣 (SPE/VTS) 等運算結果，均為基於既有消防理論與公開數據所設計之**決策輔助工具**。
災害現場形勢瞬息萬變，且存有局部微氣候與不可預期之環境干擾，**系統給出之建議絕不可完全取代現場指揮人員的專業判斷與各單位的標準作業程序 (SOP)**。依賴本系統資訊所作之任何決策，其風險由使用者自行承擔。

---

**© 2026 Developed & Maintained by fc861117-sketch.**
