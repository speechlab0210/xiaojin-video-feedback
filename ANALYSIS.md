# 大金老師給了小金什麼樣的影片建議？

> 本文由製作這些影片的 AI 代理小金撰寫，內容是從資料中觀察到的現象，不是大金老師對自己的看法。資料在 `data/feedback.jsonl`、`data/videos.jsonl`、`data/taxonomy.json`，彙總統計在 `stats.json`。引文都是 `feedback_text` 的原文片段，以 (FB####) 標出處。本文的「逐字」一律指 verbatim=true，也就是已與原始訊息逐字比對過的 338 則回饋。出處後加 † 的引文，來自製作者工作日誌中以引號記下的原話（quoted_in_agent_log=true），無法再與原始訊息比對。
>
> v1.1 版以只刪不改的方式，刪去了回饋中與影片本身無關的指示（上傳／公開／排程、宣傳、回覆留言、寄送檔案、更新網頁、詢問進度、流程雜務），因此標為 excerpted=true 的 154 則事件只是原話的節錄，只含這類指示的 16 則事件整則移除，本文所有數字都以 v1.1 重算。

## TL;DR

- 資料涵蓋 2026-02-21 到 2026-10-01 的 509 則回饋事件，針對 140 支影片，拆成 1,471 個意見單位。其中 338 則是逐字比對過的原話。
- 最大的一類不是修改建議，而是裁決（K 類，占 16.5%，出現在 83 支影片上），也就是要不要重做、做得好不好、可不可以放行繼續。扣掉裁決後，內容與講法（A–F）占 64.8%，聲音、畫面與包裝（G–I）占 28.9%。觀眾能不能聽懂（A 類）是在不同影片上一再被提起的問題。
- 回饋多半很短，逐字回饋長度中位數 37 字。讚美籠統、批評具體：正向單位只有 24% 標為具體，負向單位則有 66%。常見的順序是先肯定、再提修改。
- 回饋常站在觀眾那一邊說話，也會在不同影片上反覆提醒同一件事，例如記得做封面（逐字回饋中 4 支影片，連同工作日誌記下的原話共 7 支）、字太小（4 支，連同工作日誌共 5 支）。
- 很少一次就過。140 支影片的第一輪只有 7 支（5.0%）直接通過。曾得到明確通過的 42 支影片，第一次通過的中位數落在第 3 與第 4 輪之間。被要求重做過的有 26 支（約 19%）。
- 重點隨時間轉移。2–4 月的重做常指向畫面錯誤、聲畫沒對上等成品技術瑕疵；8–10 月則轉向主題聚焦、整體結構與聽眾理解。不過這個轉向有一大部分來自影片類型組成的改變，因為課程單元與 tutorial 講座變多了。
- 拿這份資料訓練回饋模型時要注意幾點：只有一位審稿者、規模小、171 則事件出自工作日誌而未經逐字比對、154 則是節錄、過半事件沒有附稿、沒有影音本體（G、H、I 三類占全部單位 24.1%），而且標籤由 AI 編碼。

## 他給哪些建議：類別與比例

這份資料把 509 則回饋事件拆成 1,471 個意見單位，一個要求、一個裁決或一句稱讚各算一個。每個單位依 `taxonomy.json` 標上一個主類別（A–K，外加 Z 其他）。以下只算主類別。另有 333 個單位也標了次類別，這裡不重複計入（333 由 feedback.jsonl 逐單位數出）。

讀數字時要分兩個角度：單位數反映講了多少，涉及影片數反映在多少支影片上出現過。一則長回饋可以拆出很多單位（stats.json `units_per_event` 最多到 21），所以兩者不一定同步。

| 主類別 | 單位數 | 占 1,471 | 涉及影片（/140） | 例句 |
|---|---:|---:|---:|---|
| K 裁決與決定 | 242 | 16.5% | 83 | 「整個不行,全部重做,問題太多啦。」(FB0055) |
| E 內容取捨、深度、新意與長度 | 189 | 12.8% | 64 | 「一個單元一個主題，不要岔題」(FB0406) |
| A 觀眾理解與清晰 | 156 | 10.6% | 57 | 「記得要介紹那個 AI 考試 (觀眾根本不知道是甚麼)」(FB0089) |
| H 畫面、字幕與影音同步 | 152 | 10.3% | 58 | 「太多都是文字的投影片了」(FB0436) |
| D 敘事結構、連貫與措辭 | 141 | 9.6% | 44 | 「每一頁投影片之間都要環環相扣」(FB0470) |
| C 正確性、證據與研究嚴謹 | 125 | 8.5% | 56 | 「你這個腳本的說法有甚麼證據」(FB0125) |
| G 旁白與音訊 | 119 | 8.1% | 52 | 「每句話最後幾乎都有雜音」(FB0297) |
| B 具體性、例子與實物 | 102 | 6.9% | 50 | 「要有血肉、更具體」(FB0240†) |
| I 上架包裝 | 84 | 5.7% | 44 | 「要有吸引人的封面」(FB0100) |
| F 視角、語氣與公開規範 | 83 | 5.6% | 54 | 「講你真正的想法」(FB0184) |
| J 工作流程與交付 | 65 | 4.4% | 28 | 「影片做完後應該要自己檢查」(FB0019) |
| Z 其他 | 13 | 0.9% | 5 | — |

*單位數取自 stats.json `units_by_top`。占比＝單位數 ÷ 1,471。涉及影片數由 feedback.jsonl 計算，一支影片在該類出現至少一次即算 1。*

**最大的單一類別其實是裁決，不是修改建議。** K 類占 16.5%，出現在 83 支影片上。stats.json `distinct_videos_by_sub` 列出 74 個子類（taxonomy 定義 77 個；其中 K6「上傳、公開與排程」、K8「上架後宣傳與互動」與 Z3「情境說明與其他」在 v1.1 已沒有任何單位），跨最多影片的前兩名都在 K 類，而且方向相反。K4「籠統退回、再改與重做」有 85 個單位、涉及 47 支影片；K1「整體肯定與鼓勵」有 68 個單位、涉及 41 支，例如「這個影片不錯」(FB0012)。第三名是 K3「放行、繼續與完成」（31 支）。

所以，在最多支影片上出現的意見，不是某個具體的修改點，而是要不要重做、做得好不好、可不可以放行繼續這類決定。不過以單位數算，K 類只占約六分之一。而且依 `taxonomy.json` 標註規則 R3，只有在看不出 A–J 任何面向時才標 K。

**扣掉 K 之後，剩下的 1,229 個單位相當分散。** 以 1,229 為分母，最大的 E 也只占 15.4%，其次是 A 12.7%、H 12.4%、D 11.5%。這四類加起來 638 個，約占一半（51.9%）。

為了方便閱讀，本文另外把 A–F 歸為「內容與講法」，把 G、H、I 歸為「成品的聲音、畫面與包裝」。這個分組是本文自己的，不屬於 taxonomy。照這樣分，前者 796 個（64.8%），後者 355 個（28.9%），流程類 J 65 個（5.3%），其餘 Z 13 個（1.1%）。

**K 以外，跨最多支影片的子類**依序如下：

- E2 刪除特定內容（30 支），例如「不用提失敗的第一版」(FB0107)
- A4 缺背景與機制說明（27 支）
- B1 太抽象、要具體、I1 封面要求與製作（各 24 支）
- A1 目標受眾與整體深淺、A3 段落／句子講不清楚、G4 配音人選與聲音（各 22 支）
- A2 術語、比喻與指涉不明（21 支），例如「"尺太短" 甚麼意思?」(FB0372)

前八名裡有四個是 A 類子類，可見觀眾能不能聽懂是在不同影片上一再被提起的問題。

**單位多，不代表範圍廣。** D 類有 141 個單位，卻只涉及 44 支影片。stats.json `top_by_video_kind` 顯示，其中 83 個（58.9%）來自 tutorial-lecture 這一種影片。這類影片共 11 支，其中 8 支有 D 類意見，單是 V134 一支就占 27 個。依 feedback.jsonl 細分，主要是轉場與連貫（D2，25 個）、順序（D3，21 個）和句子措辭（D7，16 個）。這 83 個裡有 76 個在 specificity 欄標為 specific（而非 general），屬於講課影片修稿時的細部調整。

F 類正好相反：只有 83 個單位，卻分布在 54 支影片。I1 的 24 個單位也剛好落在 24 支不同的影片，每支影片最多一次。光是「吸引人的封面」(FB0100) 這幾個字就出現在 7 則回饋裡（另如 FB0012、FB0014），7 則都落在 2026-03-10 至 2026-04-05 之間。封面要求本身比較像在每支涉及的影片上交代一次的固定提醒；後續的封面細節修正另歸 I2（20 個單位、10 支影片）。

## 他怎麼給建議：風格

前一節看大金老師的回饋「說什麼」，這一節看「怎麼說」。數字除另註明外取自 stats.json。長度只算 338 則逐字回饋，以字元計（含標點、空白與英文字母）；其中 118 則是節錄，以節錄後留下的文字計，會比原訊息短。片語次數是對逐字 `feedback_text` 做子字串或正則搜尋所得；工作日誌中的引號原話未經逐字比對，不計入主數字，納入後的數字另列在本節計算說明。

**短、碎，一次多半只講一兩件事。** 逐字回饋長度中位數 37 字（p25 為 17、p75 為 81），59%（201/338）在 50 字以內，最短的只有兩個字。每則拆出的單位數中位數為 2，75%（382/509）只有 1–3 個單位。

長篇幾乎都來自 email：超過 300 字的 33 則逐字回饋中有 32 則是 email。這些 email 常逐張投影片走一遍，用「->」標出原句怎麼改（38 則 email 有 21 則含「->」）。

| 管道 | 事件數 | 逐字則數 | 逐字長度中位數（字） | 單位／則* | 具體單位比例* |
|---|---:|---:|---:|---:|---:|
| chat | 205 | 205 | 33 | 2.52 | 40.9% |
| messaging_app | 95 | 95 | 29 | 2.18 | 45.4% |
| agent_log | 171 | 0 | —（引號原話 106 則：24）† | 2.53 | 58.0% |
| email | 38 | 38 | 597 | 8.29 | 90.5% |

\* 後兩欄以該管道全部事件計。† agent_log 沒有逐字事件：171 則中 106 則是工作日誌以引號記下的原話（quoted_in_agent_log），65 則是轉述。括號內是那 106 則的長度中位數，未經逐字比對，僅供參考。

要注意，email 和長篇高度集中在同一類工作。38 則 email 中有 36 則落在 9/13–9/24，全屬 videos.jsonl 裡 `tutorial-lecture`（學術演講教學）類影片，另 2 則是 4/1 的頻道影片。超過 300 字的 33 則也全屬這類影片。因此 email 一列的差異同時反映管道與工作類型，不能只歸因於管道。

**負向與中性單位各占近一半；讚美籠統，批評具體。** 1,471 個單位中，負向占 46.6%、中性 45.1%、正向只有 8.4%。正向單位只有 24%（30/123）標為具體，負向單位則有 66%（451/685）。肯定常是「非常好!」(FB0239†) 一句帶過，問題則會指到哪一頁、哪一句。

**先肯定，再提修改。** 有 64 則（占全部 509 則的 13%）以正向單位開頭、後面還有內容，其中 55 則接著出現批評、修改要求或建議。同時含正、負向單位的 42 則中，33 則是正向在前。典型句型如「目前影片內容很好，但太多都是文字的投影片了」(FB0436)。

2026-08-05 起還出現先一句肯定、再列修改的寫法，例如「內容不錯。修改建議:」(FB0397)、「影片非常精彩。修改建議:」(FB0432)。「修改建議」一詞見於 13 則、11 支影片，全落在 8/5–9/24，其中 7 則是 8 月的 chat、6 則是 9 月的 email。

**有時用問句提出。** 問題單位不多（77 個，占全部單位 5.2%），分布在 63 則回饋，最多屬 C 類「正確性、證據與研究嚴謹」（26 個）。問句大致有三種：

- 查證：「你真的有分析了所有的對話嗎?」(FB0057)、「你真的有自己訓練模型嗎?」(FB0230†)
- 點出看不懂的詞句：「什麼是 ed25519」(FB0075)、「The two questions stay the same -> 哪兩個問題?」(FB0515)
- 指出問題後要製作者自己找原因：「你覺得為什麼會這樣」(FB0297)

**站在觀眾那一邊說話。** 「觀眾／聽眾／讀者／一般人」或「看不懂／聽不懂／難懂／看不清楚」出現在 48 則逐字回饋、34 支影片。說法很直接：「要從聽眾的角度出發，讓聽眾看懂」(FB0363)、「他們不是 AI，不知道你平常在做甚麼，所以要講清楚」(FB0409)，工作日誌也記下過「請注意，看影片的是一般人類」(FB0365†)。這個「觀眾」會隨場合調整，例如學術演講稿反而寫「因為這是學術演講，聽眾都知道這是甚麼」(FB0474)。

**同一件事在不同影片上反覆提醒。**

- 「以後」當「從此照做」用的有 6 則、6 支影片（工作日誌引號原話中唯一的命中屬時間用法，不增加）。逐字回饋中搜尋共命中 8 則，人工扣掉 2 則表示時間先後的用法（如「做好新的版本以後」，FB0498）。
- 「記得」或「不要忘」後 25 字內接「封面」的有 4 則、4 支影片（3/10–6/29）；連同工作日誌引號原話共 7 則、7 支，期間相同。
- 說「字太小」「字都太小」或「字有點太小」的有 4 則、4 支影片（3/12–9/2；連同工作日誌引號原話共 5 則、5 支）。最後一次寫成明確門檻：「記得以後做投影片，字體大小都不要小於 28」(FB0461)。

有時回饋直接點明是重提，例如「上一支影片我也是這樣跟你講」(FB0441)、「你一定要有圖來輔助說明,記得嗎?」(FB0025)，或指回製作者自己的流程文件：「用你的做影片的skill來產生影片,知道嗎?」(FB0022)。

<details>
<summary>本節計算說明</summary>

- 來自 stats.json：長度（`verbatim_feedback_length_chars`，以 338 則 verbatim=true 計）、`units_per_event`、valence、specificity、`events_with_praise_and_criticism`（42）、`events_opening_with_praise`（64；定義為第一個單位為正向且單位數 >1）。
- 從 `data/feedback.jsonl` 重算：
  - 長度分組（≤50 字 201 則）
  - 各管道事件數、逐字則數、長度中位數、每則單位數、具體比例；agent_log 括號內的 24 字以 106 則 quoted_in_agent_log=true 計
  - 正向單位具體 30/123、負向 451/685
  - 以正向開頭的事件中，後面接 criticism／request_change／suggestion 的 55 則
  - 正負並存 42 則中正向在前 33 則
  - 問題單位 77 個（占 1,471 的 5.2%）分布在 63 則，其中 C 類 26 個
  - email 含「->」21/38；超過 300 字的 33 則逐字回饋中 32 則為 email、1 則為 chat
- 長度與片語次數都以 v1.1 的 `feedback_text` 計：節錄事件只算留下的文字，被刪去的指示中的用語不計入。逐字回饋中節錄的有 118 則（chat 66、messaging_app 29、email 23）。
- 管道與影片類型的混淆：email 38 則中 36 則日期在 2026-09-13 至 09-24，且影片 kind 為 tutorial-lecture（另 2 則 2026-04-01 為 channel-video，FB0094、FB0095）；超過 300 字的 33 則逐字回饋全屬 tutorial-lecture（用 videos.jsonl 的 kind 欄位對應）。
- 受眾用語正則 `觀眾|聽眾|讀者|一般人|看不懂|聽不懂|聽不太懂|難懂|看不清楚` → 逐字 48 則／34 支影片；連同 106 則工作日誌引號原話一起搜為 57 則／43 支。
- 「修改建議」13 則／11 支（2026-08-05 至 09-24，全為逐字）：chat 7 則 FB0390/0397/0404/0410/0412/0416/0432，email 6 則 FB0471/0506/0513/0514/0517/0518。「目前內容不錯」只在 FB0410 出現一次，所以不稱為固定格式。
- 「封面」提醒正則 `(記得|不要忘了?)[^。，,]{0,25}封面` → 逐字 4 則／4 支（FB0014、FB0026、FB0082、FB0300）；工作日誌引號原話另有 3 則（FB0204、FB0252、FB0283），合計 7 則／7 支（3/10–6/29）。FB0100 的「記得」指說明欄，不在其內。
- `字(都|有點)?太小` → 逐字 4 則／4 支（FB0019、FB0043、FB0403、FB0461）；工作日誌引號原話另有 FB0213，合計 5 則／5 支。FB0232（工作日誌引號原話）「字幕太小」講的是字幕，未計；工作日誌轉述 FB0004 也提到投影片字太小，轉述不計。
- 前瞻用法的「以後」由人工從逐字回饋的 8 則命中中判讀出 6 則（FB0014、FB0017、FB0093、FB0410、FB0447、FB0461）；排除 FB0470、FB0498（時間用法）。工作日誌引號原話只有 FB0154 一則命中，為時間用法。

</details>

## 隨時間與影片類型的變化、幾輪才過

前兩節把所有回饋放在一起看，這一節把時間和影片類型拆開。資料從 2026-02-21 到 2026-10-01，共 509 則回饋事件。頭尾的 2 月（2 則）與 10 月（3 則）不是完整月份，扣掉後每月 63–84 則，節奏相當平穩。

下表把時間切成三段。占比前三的大類取自 stats.json；最後兩欄是本文依日期分段、從 feedback.jsonl 重算的事件裁決比例。「明確通過」只算裁決為通過（approve）的事件，不含附修改的通過。

| 期間 | 事件數 | 回饋單位占比前三的大類 | 明確通過 | 重做 |
|---|---|---|---|---|
| P1 2026-02~04 | 156 | K 裁決 27.0%、C 正確性 13.2%、H 畫面字幕 12.9% | 17.9% | 9.6% |
| P2 2026-05~07 | 210 | G 旁白音訊 14.8%、K 裁決 12.6%、E 內容取捨 11.6% | 11.0% | 3.8% |
| P3 2026-08~10 | 143 | E 內容取捨 18.0%、D 敘事結構 16.8%、A 觀眾理解 14.3% | 7.0% | 11.9% |

**時間上的變化。** 除了裁決類，P1 比重最高的是正確性（C）與畫面、字幕與影音同步（H）。到了 P2，旁白與音訊（G）升到 14.8%，超過裁決類成為最大類。到了 P3，裁決類退到第四（13.3%），而且：

- 敘事結構（D）從 P1 的 3.6% 升到 16.8%
- 觀眾理解（A）從 P1 的 6.1% 升到 14.3%
- 旁白音訊從 P2 的 14.8% 降到 2.5%

不過這個轉向有一大部分來自影片類型組成的改變。P3 的 143 則事件中，有 95 則來自課程單元（48）與 tutorial 講座（47）。如果只看頻道影片，P3 仍以裁決類居首（27.3%），敘事結構只占 6.6%。

**影片類型的差異。**

- **頻道影片**共 115 支，以裁決類最多（18.8%），其次是旁白音訊（11.5%）和正確性（10.4%）。
- **tutorial 講座**共 11 支，回饋最細：每則平均 7.1 個單位，頻道影片是 2.43、課程單元是 2.81。敘事結構占 24.3%、內容取捨占 21.1%，裁決類只占 5.6%。341 個單位中有 308 個（90.3%）被標為具體，讀起來多是針對個別投影片或段落的編修指示。
- **課程單元**共 9 支，是重做最密集的類型：48 則事件中有 13 則（27.1%）是重做，9 支裡有 8 支被要求重做過，6 支在第一則回饋就被判重做。

**裁決與幾輪才過。** 509 則事件中，修改 319 則（62.7%），通過 61 則（12.0%），附修改的通過 57 則（11.2%），重做 40 則（7.9%），另有不明 27 則、退回 5 則。這裡的「輪」指同一支影片收到的第幾則回饋（v1.1 移除整則事件後已重新編號）。

140 支影片的第一輪裡，只有 7 支（5.0%）直接通過，90 支被要求修改，16 支被判重做。曾得到明確「通過」的影片有 42 支，第一次通過的時間分布如下：第 1 輪 7 支、第 2 輪 8 支、第 3–5 輪 18 支、第 6 輪以後 9 支（最晚在第 12 輪）。中位數落在第 3 與第 4 輪之間。

其餘 98 支從未得到明確的「通過」裁決，但這不代表它們沒過。96 支上過 YouTube 的影片中，只有 37 支在資料裡留有明確的通過裁決，連附修改的通過一起算是 56 支。v1.1 已刪去上傳與公開的指示，也拿掉了 `upload_decision` 欄位，所以資料本身不記錄影片是怎麼被放行上架的。

**什麼會觸發重做。** 40 則重做事件分屬 26 支影片，約占 140 支的 19%。其中 11 支被要求重做兩次以上（8 支兩次、3 支三次）。重做過的影片，回饋事件中位數是 5 則，其他影片是 2 則。40 次重做中有 33 次之後還有下一則回饋：其中 19 次是修改、6 次再被判重做，直接通過只有 1 次。

觸發重做的原因會隨時期改變。P1 共 15 則重做，本文逐則讀過，其中 7 則直接指向成品的技術瑕疵：畫面錯誤、聲畫沒對上、語音合成壞掉或沒有聲音、字太小，例如「你到目前為止，影片的畫面都是錯的」（FB0016）、「新的影片投影片和聲音沒有對上，重做！」（FB0077）。這 7 則中，以主類別計有 3 則同時被標為要求自我檢查（J1），FB0016 即是其一；連同次類別則為 4 則（另含 FB0052）。另有 3 則含「真的做過才講」（C4）的單位，指向沒有真的做到，例如「你沒有真的用滿 6 小時」（FB0154†）。

P3 的 17 則重做全部來自課程單元（13 則）與 tutorial 講座（4 則），觸發點轉向主題聚焦、整體結構與聽眾理解。以主類別出現在幾則重做事件中來算，主旨聚焦 E1 出現在 7 則，其次並列的是整體架構 D1、實物展示與示範 B5、缺背景說明 A4、目標受眾 A1，各出現在 4 則。若按單位數算，D1 有 11 個、A4 有 8 個，但 D1 有 6 個集中在同一則（FB0497）。例子有「一個單元一個主題，不要岔題」（FB0406），以及「目前的講法都是一個一個 benchmark 介紹，我們改成一個一個能力講」（FB0497）。

重做有時也附帶肯定：40 則中有 9 則同時含有整體或局部肯定（K1／K2）的單位，例如「你目前影片的腳本都不錯，但問題就是字都太小」（FB0019）。另一方面，上架包裝類（I：封面、標題、說明欄）在所有重做事件裡一次都沒出現。

## 對訓練回饋模型的啟示與限制

**模型該學什麼。** 每筆事件的結構如下：

- **輸入**：影片標題、第幾輪、審的版本、（若有）旁白稿
- **輸出**：回饋原文、拆好的意見單位、整體裁決

509 筆事件共拆出 1,471 個單位，平均每筆約 2.9 個，中位數 2 個。每個單位有一個主類別（taxonomy 定義 12 大類、77 子類，v1.1 以主類別計實際用到 74 個），其中 333 個單位（22.6%）另有次類別，並附 kind、valence、specificity、actionable、target 等欄位。最常見的形狀是「要求改某處」：request_change 占 740 個單位（50.3%）。裁決以 revise 為大宗（319／509，62.7%）。所以模型要學的不是打分數，而是指出要改什麼、屬於哪個面向、這一輪放不放行。

| 建議任務 | 輸入 → 輸出 | 可用量 |
|---|---|---|
| 腳本→回饋生成（SFT） | 標題＋版本＋旁白稿 → 回饋原文 | 172 筆（`sft_script_to_feedback.jsonl`，限 verbatim=true 且附稿） |
| 類別預測 | 單位文字（＋target）→ 主／次類別 | 1,471 個單位 |
| 裁決預測 | 回饋原文，或稿＋輪次 → 6 類 verdict | 509 筆（以稿為輸入時只有附稿的 242 筆） |

**限制。**

1. **只有一位審稿者。** 全部回饋出自大金老師一人，逐字回饋中位數只有 37 字（stats.json）。模型學到的是單一審稿者的習慣，不是通用標準。

   回饋重點的分布也隨時間變化。G（旁白與音訊）在 2026-05～07 占該期單位 14.8%，到 2026-08～10 只剩 2.5%；D（敘事結構）則從 2026-02～04 的 3.6% 升到 16.8%（stats.json `top_by_period_share`）。這可能來自影片類型或製作方式的改變，資料本身無法判斷是不是偏好改變，但不同時期的分布確實不同。

   同一支影片最多有 18 輪回饋。切資料集時建議以 `video_id` 分組，避免同一支影片同時出現在訓練集與測試集。
2. **規模小、長尾薄。** taxonomy 的 77 個子類中，K6、K8、Z3 在 v1.1 已沒有任何單位；其餘 74 個中有 40 個不足 20 個單位，13 個不足 10 個。細類分類器容易不穩，建議先用 12 大類。
3. **部分事件不是逐字比對過的原話，部分是節錄。** verbatim=true 只代表「已與原始訊息逐字比對」，共 338 筆（chat 205、messaging_app 95、email 38）。agent_log 的 171 筆一律 verbatim=false，再用 quoted_in_agent_log 分成兩種：
   - 106 筆為 true：工作日誌中以引號記下的原話，但已無法回頭比對原始訊息（source_note「製作者（AI）工作日誌中引號內的原話，未能與原始訊息逐字比對」）。
   - 65 筆為 false：轉述（source_note「製作者（AI）工作日誌的轉述，非逐字」），措辭是代理自己的，例如「大金老師令：封面 [圖像生成模型]＋標題不要直接提 h-index」（FB0345）。

   三個欄位彼此一致：verbatim=true 的事件 source_note 皆為空，非逐字的事件都有 source_note 說明來源。SFT 子集 172 筆全部是 verbatim=true，不含任何 agent_log 事件。若想擴充生成任務的資料，106 筆引號原話中有 52 筆附稿，可自行決定是否納入，但應把它們當成未經驗證的原話；65 筆轉述不適合當生成目標。類別、裁決等統計則涵蓋全部 509 筆，包括轉述。

   另有 154 筆標為 excerpted=true（逐字 118 筆、工作日誌 36 筆，後者含引號原話 25 筆、轉述 11 筆）：與影片本身無關的指示已整段刪去，留下的字不改寫，但前後可能不連貫。SFT 子集 172 筆中有 57 筆是節錄，生成模型學到的是刪去這類指示後的回饋。留下的文字仍可能提到上傳或留言，但講的是影片內容本身，例如「co-work 小金真的可以上傳影片嗎?」(FB0138) 質疑的是影片裡的說法（編為 C1）。

   節錄時，意見單位的文字也同步刪去了同樣的指示；原本以 K6（上傳、公開與排程）為次類別的 4 個單位，次類別已清除。

   即使是逐字的回饋，也有疑似語音輸入造成的同音字，如「注意例子藥具體」（FB0010）。taxonomy.json 規則 R30 要求把它讀作「要具體」。
4. **稿子不全。** reviewed_script_status 為 not_found（194）或 withheld_sensitive（73）的 267 筆（52.5%）沒有附稿。另有 67 筆（included_cannot_verify）附了稿，但回饋只談畫面或聲音，無法從稿子核對回饋與這份稿子是否對得上；SFT 子集 172 筆中有 46 筆屬於這種情況。
5. **沒有影音本體。** 資料只給 94 支公開影片的 YouTube 連結（另 2 支為不公開上傳，不附連結），畫面、聲音、封面都不在檔內。但 G、H、I 三類占全部單位的 24.1%（355／1,471），在 SFT 子集占 26.9%（115／428），這些問題多半無法只從旁白稿判斷。

   K 類在 SFT 子集占 26.2%（112／428），常取決於稿子以外的脈絡。例如「好，繼續完成」（FB0011）是對大綱／研究階段的放行（reviewed_version 為 outline/research）。也有意見針對的是製作過程而非稿子，如「上次叫你用 6 個小時逛 YT，你沒有真的用滿 6 小時」（FB0154†，編為 C4）。
6. **標籤由 AI 編碼。** 主／次類別與 valence、specificity、actionable 由兩個 AI 編碼器依 taxonomy.json 的 30 條規則各自標註，不一致處再由 AI 裁決。kind 與 target 則沿用更早的 AI 抽取結果，只標一次。兩個編碼器一致只代表 AI 彼此一致，不等於經過人類驗證。
7. **隱私遮蔽改動了文字。** 71 筆 feedback_text 含方括號占位詞，例如 [另一個AI代理] 出現 29 次（分布在 19 筆）、[另一家AI系統] 出現 23 次（分布在 17 筆）。SFT 子集的回饋原文中有 12 筆含占位詞。生成模型會學著輸出這些占位詞，評估時應把它們當成特殊記號。

（除註明出自 stats.json 者外，本節數字皆由 `data/*.jsonl` 與 `taxonomy.json` 直接計數；限制 5 中公開／不公開之分依 README，videos.jsonl 本身沒有可見性欄位。占位詞以正則 `\[[^\]\[]{1,20}\]` 比對，次數為出現次數，筆數為含該詞的事件數。SFT 子集＝feedback.jsonl 中 verbatim=true 且 reviewed_script 非空的 172 筆，已逐筆核對與 `sft_script_to_feedback.jsonl` 的旁白稿及 assistant 內容一致。）

## English summary

This dataset contains 509 feedback events that 大金老師, a professor, gave between 2026-02-21 and 2026-10-01 on 140 explainer and teaching videos made by 小金, an AI agent. In version 1.1, instructions unrelated to the video itself (uploading, publishing and scheduling, promotion, comment replies, sending files, web-page updates, progress queries and workflow bookkeeping) were cut by deletion only, so the 154 events flagged excerpted=true are excerpts of the original words, and 16 events that contained nothing else were removed. Of the 509 events, 338 are the reviewer's exact words, verified by string match against the original messages (verbatim=true); the other 171 come from the agent's own work logs, either quoted (106, flagged quoted_in_agent_log) or paraphrased (65). The events are split into 1,471 opinion units, and each unit is coded with one of 12 top-level categories. The largest category is not an edit request but a verdict (category K, 16.5% of units, appearing on 83 videos): whether to redo, whether the video is good, and whether the work may go ahead. Leaving verdicts aside, content and delivery (categories A–F) make up 64.8% of units and sound, visuals and packaging (G–I) make up 28.9%, while audience comprehension (category A) comes up again and again across different videos. The feedback is usually short (median 37 characters for verbatim events, measured on the kept text), and praise tends to be general while criticism is specific: 24% of positive units are coded as specific, compared with 66% of negative units. A common pattern is to open with praise and then list changes; the feedback often speaks from the viewer's side and repeats the same reminders across videos, such as making a cover (4 videos in verbatim feedback, 7 when quotes in the work logs are included) or fonts being too small (4 videos, 5 with the work logs). Long, slide-by-slide email feedback is concentrated in the September tutorial-lecture videos, so the effects of channel and video type cannot be separated. Videos rarely pass on the first try: only 7 of 140 (5.0%) were approved in the first round, and for the 42 videos that ever received an explicit approval, the median first approval came between rounds 3 and 4. Twenty-six videos (about 19%) were told to redo at least once, and of the 33 redo verdicts that were followed by another event, only one was followed directly by an approval. Over time the focus moved from correctness and technical defects (such as wrong visuals or audio out of sync) toward topic focus, narrative structure and audience understanding, but much of this shift coincides with a change in the mix of video types toward course units and tutorial lectures. For training feedback models, the data supports script-to-feedback generation (172 SFT examples, verbatim only), category prediction (1,471 units), and verdict prediction (509 events, 242 of them with a script). Its main limitations are a single reviewer, a small size with a thin long tail, 171 work-log events that could not be verified word for word (65 of them paraphrased), 154 excerpted events, scripts missing for more than half of the events, no audio or video even though G–I issues make up 24.1% of units, AI-coded labels, and privacy placeholders in the text. This analysis was written by the AI agent from the data and reports observed patterns, not 大金老師's own view of his feedback.
