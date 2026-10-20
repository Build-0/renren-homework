# 仁仁功課 — 項目說明（俾 Claude 同 Benny 睇）

## 係咩
為仁仁（K1，幼兒班）而整嘅溫習遊戲網站。內容**跟足學校嘅溫習大綱**，但一定要**好玩**。
Benny 會定期影學校大綱／功課相上嚟，Claude 負責將內容加入資料檔。

## 檔案結構
```
index.html          整個網站（HTML + CSS + JS 一個檔），唔使 build
data/terms.json     階段清單，例如 ["k1-t1.json"]；最後一個 = 主頁預設顯示
data/k1-t1.json     幼兒班第一段綜合評估（26/27 年度）全科內容
```
本機試：喺呢個資料夾行 `python -m http.server 8000`，再開 http://localhost:8000
（直接雙擊 index.html 會讀唔到 data/，因為瀏覽器唔俾 file:// fetch）

## 加新階段／新功課（最常做嘅事）
1. 複製 `data/k1-t1.json` 做新檔，例如 `data/k1-t2.json`
2. 改 `id`、`name`、`schedule`、`notes`、`subjects`
3. 喺 `data/terms.json` 加埋個檔名（放最後 = 主頁預設）
4. 新詞語要配圖就加入 `pics`（emoji）

### subject 格式
- `id`：yue 語文 / math 識數 / eng 英文 / gs 常識 / cmn 普通話 / rel 宗教 / sing 唱遊（決定主頁圖示同顏色）
- `lang`：讀音語言 yue 廣東話 / cmn 普通話 / en 英文
- `pre: true`：提前評估（主頁分開一組）
- `dates`、`scope`（老師原文「評估內容」）
- `items`：每項一張卡，`label` 用**老師原文**，再配：
  - `words` + `games`（cards 字卡 / hear 聽音揀字 / match 翻牌 / picword 睇圖揀英文字）
  - `sentences`：`{pre, post, opts}` + `games:["sent"]`
  - `letters` + `games:["letter","lmatch"]`
  - `quiz`：`[{q, o:[[emoji, 答案], ...]}]`，第一個選項係正確答案
  - `sorts`：分類遊戲
  - `songs`：只列歌名（**唔好放歌詞／祈禱文原文**），俾家長剔
  - 識數專用遊戲：count / order / chant / next / color / shape / pickall

## 設計決定（Benny 講過嘅）
- 跟足老師要求，每項標題用老師原文；但唔可以失去趣味（星星、貼紙簿、連續答啱、星星雨、讚佢嘅聲）
- **唔好用田字格／方格底**：會干擾認字。字直接顯示，每個詞獨立一粒按鈕，詞同詞之間要有足夠空位
- 手機上字要大、間距要闊（唔好黐埋一齊）
- 背景純色，唔要生字簿橫線
- 每組詞語右上角有「🔀 隨機排序」（進階：打亂位置都認到）
- 讀音用瀏覽器 Web Speech（zh-HK / zh-CN / en-GB）；iPhone 靜音掣問題用 audioSession='playback' + 靜音 loop 處理
- 進度（星星、識咗嘅字、剔歌）用 localStorage，只喺每部機自己度
- 頁面唔好寫仁仁全名同學校名（GitHub Pages 係公開）

## 已完成
- 第一段評估全科：語文、識數、英文、常識、普通話、宗教、唱遊、評估日程、學校備註
- 測試聲音掣、貼紙簿（每 5 粒星一張，共 30 張）

## 下一步
- [ ] 開 GitHub repo（例如 renren-zi），push，開 GitHub Pages（Settings → Pages → main / root）
- [ ] 視乎需要：用 Supabase 做全家同步進度（媽咪、婆婆部機見到同一份星星）
- [ ] 每次有新大綱／功課 → 加新 data 檔

## 舊版
Claude 網頁版 artifact：https://claude.ai/artifact/CxeMcbajCcJKcn4JvtQ4UF（私人連結；上咗 GitHub Pages 之後以 Pages 為準）
