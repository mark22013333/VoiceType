<p align="center">
  <img src="icon.png" width="128" alt="VoiceType icon">
</p>

<h1 align="center">VoiceType</h1>

<p align="center">
  macOS / Windows 桌面應用。語音轉文字，內建離線辨識 + AI 智慧潤稿（預設 Claude CLI，可改用其他 LLM）。
</p>

<p align="center">
  <a href="https://github.com/mark22013333/VoiceType/releases/latest">下載最新版本</a>
</p>

---

## 下載安裝

前往 [Releases](https://github.com/mark22013333/VoiceType/releases/latest) 下載最新版本。

| 平台 | 檔案 |
|------|------|
| macOS (Apple Silicon) | `VoiceType_x.x.x_aarch64.dmg` |
| Windows (x64) | `VoiceType_x.x.x_x64-setup.exe` |

### 系統需求

- **macOS 10.15+**（Catalina 以上）或 **Windows 10+**
- AI 潤稿需要至少一個 LLM Provider：預設為 [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code/overview)（需已安裝並登入），也可在「設定 → LLM Provider」改用 Antigravity CLI、Google Gemini、Groq 或 Ollama（本機）

### macOS 安裝步驟

1. 下載 `.dmg` 檔案
2. 開啟後將 VoiceType 拖入 Applications 資料夾
3. **首次開啟前，請先執行以下指令**（每次更新後需重新執行一次）

> **macOS 會阻擋未簽名的應用程式**
>
> 由於 VoiceType 尚未取得 Apple 開發者簽名，macOS 會顯示「已損毀，無法打開」的錯誤訊息。
> 這不是檔案損壞，而是 macOS Gatekeeper 安全機制的限制。
>
> **開啟終端機，貼上以下指令即可解決：**
>
> ```bash
> xattr -cr /Applications/VoiceType.app
> ```
>
> 執行後即可正常開啟 VoiceType。**每次更新版本後需要重新執行一次。**

### Windows 安裝步驟

1. 下載 `.exe` 安裝檔
2. 如果 Microsoft Edge 顯示「不常下載，請確定您信任此檔案」，請點選下載項目旁的選單，依序選擇「保留」→「顯示更多」→「仍要保留」
3. 執行安裝程式；如果 Microsoft Defender SmartScreen 顯示「Windows 已保護您的電腦」，請選擇「其他資訊」→「仍要執行」
4. 依安裝程式指示完成安裝

> **Windows 為什麼會顯示安全警告？**
>
> 目前 Windows 安裝檔尚未使用付費的程式碼簽章憑證，因此發行者會顯示為「不明」，SmartScreen 也可能將新版安裝檔標示為不常下載。這不代表檔案已損壞；請只從本頁連結的 [GitHub Releases](https://github.com/mark22013333/VoiceType/releases/latest) 下載。若檔案來自其他網站或來源不明，請取消安裝。

### Windows／VMware 啟動後閃退

若 VoiceType 的介面出現後立刻關閉，尤其是在 VMware 的 Windows 11 虛擬機器內，可能是 WebView2 與虛擬顯示卡的硬體加速相容性問題。請先關閉 VoiceType，再依下列步驟啟用軟體繪圖：

1. 按 `Win + R`，輸入 `cmd` 後按 Enter
2. 執行：

   ```bat
   setx VOICETYPE_SOFTWARE_RENDERING 1
   ```

3. 關閉命令提示字元，重新啟動 VoiceType

若仍然閃退，請到 `%APPDATA%\com.voicetype.app\logs\` 取得 `bootstrap.log` 與最新的當機記錄，並到 [Issues](https://github.com/mark22013333/VoiceType/issues) 回報。若要關閉軟體繪圖模式，可執行 `setx VOICETYPE_SOFTWARE_RENDERING 0` 後重新啟動 VoiceType。

---

## 功能特色

### 語音辨識（預設離線）
- 內建 [Whisper](https://github.com/openai/whisper) 與 [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) 語音辨識引擎，預設所有辨識在本機完成；也可在「設定 → 模型管理 → 外部 ASR 伺服器」改送到外部 ASR 伺服器（例如 whisper.cpp 的 `whisper-server`）辨識
- 支援多種模型（首次使用在「設定 → 模型管理」下載）：Whisper 系列、SenseVoice、Qwen3-ASR、Paraformer、Fire-Red-ASR2，以及 3 個「日常即時聽寫」串流模型
- 即時串流辨識，錄音中即時顯示辨識文字：只在已安裝串流模型並啟用「日常即時聽寫」（設定 → 模型管理）、且只用麥克風錄音時使用；會議或含系統音訊的錄音改用一般高品質模型

### AI 智慧潤稿
- 透過 AI 將口語轉為書面文字；支援 5 個 LLM Provider：Claude CLI（預設）、Antigravity CLI、Google Gemini、Groq、Ollama（本機）
- 停錄後自動潤稿預設關閉，可在「設定 → 錄音」分別開啟「日常即時聽寫」與「會議／離線錄音」的自動潤稿；會議／離線錄音另有秒數門檻（預設 60 秒，較短的錄音直接輸出原文）
- 8 種內建潤稿風格：日常自然、會議紀錄、任務清單、郵件草稿、簡短訊息、正式語氣、輕鬆語氣、提示詞最佳化
- 支援自訂風格，可新增或從內建風格「複製後自訂」，建立專屬的 AI prompt
- 使用 Claude CLI 時可選擇 Claude 模型（Haiku / Sonnet / Opus）；未指定時由 Claude CLI 自動選擇

### 手動輸入潤稿（TYPE 模式）
- 不錄音也能使用 — 直接貼上文字進行 AI 潤稿
- 套用相同的潤稿風格和模型設定

### 音訊預處理
- RNNoise 降噪，過濾背景雜音（冷氣、風扇、鍵盤聲）
- 高通濾波預處理，提升辨識準確度
- 高通濾波與 RNNoise 降噪由同一個開關控制（設定 → 錄音 → 降噪處理），關閉降噪時兩者都不會執行

### 桌面整合
- 全域熱鍵一鍵錄音（預設 macOS 為 Cmd+Shift+O、Windows 為 Ctrl+Shift+O，可自訂）；觸發方式可選「按一下開始／再按一下停止」、「按住錄音／放開停止」或停用錄音快捷鍵
- 常駐工具列，不佔螢幕空間
- 結果預設自動貼上到游標位置：先寫入剪貼簿、模擬貼上；原本剪貼簿若是文字，貼上後會還原（圖片、檔案或空白剪貼簿不會還原，結果會留在剪貼簿）。可在「設定 → 一般 → 辨識結果輸出」改為「只複製到剪貼簿」
- 錄音檔（.wav）和逐字稿自動存檔

---

## 技術架構

| 元件 | 技術 |
|------|------|
| 桌面框架 | [Tauri v2](https://v2.tauri.app/) (Rust) |
| 前端 | Svelte 5 + Vite |
| 語音辨識 | [whisper.cpp](https://github.com/ggerganov/whisper.cpp)、[sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) (本機推理) |
| AI 潤稿 | Claude Code CLI（預設）、Antigravity CLI、Google Gemini、Groq、Ollama |
| 降噪 | RNNoise (nnnoiseless) |
