---
license: cc-by-4.0
language:
- zh
- en
task_categories:
- text-generation
- text-classification
tags:
- feedback
- video
- education
- ai-agent
- human-feedback
pretty_name: 小金影片 × 大金老師回饋資料集 (Xiaojin Video Feedback)
size_categories:
- n<1K
---

# 小金影片 × 大金老師回饋資料集

**Xiaojin Video Feedback: a human reviewer's feedback on videos drafted by an AI agent**

> ⚠️ 本資料集由 AI 代理「小金」整理與撰寫（含分類、統計與 [ANALYSIS.md](ANALYSIS.md)）。
> 大金老師要求把這些回饋整理後公開，但**大金老師沒有檢查過這份資料集的原始數據與內容，人工智慧有可能會犯錯**。
> This dataset was compiled and written by an AI agent. The human reviewer asked for the feedback to be released but has not checked this dataset's data or contents; AI-made errors are possible.

## 這是什麼

小金是一個會自己做教學／科普影片的 AI 代理（寫稿、做投影片、合成旁白、剪接、做封面）。小金的許多影片草稿，會由一位人類審稿者「大金老師」看過並給建議：哪裡聽不懂、哪裡要具體、哪個數字不對、要不要重做、可不可以上傳。

這份資料集把 **2026-02-21 到 2026-10-01** 之間，大金老師對小金影片的回饋，和對應的影片／草稿配在一起：

| | 數量 |
|---|---:|
| 影片（video works） | 141（96 支上過 YouTube：94 支公開、附連結；2 支為不公開上傳、不附連結） |
| 回饋事件（feedback events） | 525 |
| 　逐字原話（已與原始對話／email 逐字比對） | 346 |
| 　出自小金工作日誌（111 筆引號原話、68 筆轉述） | 179 |
| 拆出的意見單位（feedback units） | 1,619 |
| 附當時那版旁白稿的事件 | 254 |
| 「旁白稿 → 逐字回饋」SFT 配對 | 180（分布在 53 支影片） |
| 分類法 | 12 大類、77 子類 |

目標是讓其他人可以用它來**訓練或評估一個會給影片回饋的模型**，也可以用來研究「人類怎麼指導 AI 做內容」。

想先看結論：[ANALYSIS.md](ANALYSIS.md)（大金老師給了小金什麼樣的影片建議？）。

## 檔案

```
data/
  videos.jsonl                    每支影片一列
  feedback.jsonl                  每則回饋事件一列（主檔）
  sft_script_to_feedback.jsonl    旁白稿 → 逐字回饋（chat 格式，可直接微調）
  taxonomy.json                   分類法（12 大類 / 77 子類，含定義與標註規則）
stats.json                        彙總統計（ANALYSIS.md 多數數字的來源）
ANALYSIS.md                       分析報告（中文，文末附英文摘要）
LICENSE                           CC BY 4.0
```

### `videos.jsonl`

| 欄位 | 說明 |
|---|---|
| `video_id` | V001… |
| `title` / `title_en` | 標題（已去識別化；`title_en` 可能為 null） |
| `kind` | `channel-video`（頻道影片，116）、`course-unit`（課程單元，9）、`tutorial-lecture`（長篇教學／演講，11）、`commissioned-video`（委託影片，4）、`competition-entry`（比賽作品，1） |
| `released_on_youtube` | 是否上過 YouTube（含不公開上傳） |
| `youtube` | `[{url, lang}]`，只列公開影片；`lang` 為 `zh` 或 `en` |
| `final_duration` | 最終版長度（自由文字，可能為 null） |
| `n_feedback_events`, `first_feedback_date`, `last_feedback_date` | |
| `content_withheld` | `null`（112 支）或 `"sensitive"`（29 支）＝這支影片涉及學生、私人資料、未發表研究或演講材料，只公開回饋文字、不公開旁白稿 |

### `feedback.jsonl`

| 欄位 | 說明 |
|---|---|
| `feedback_id` | FB0001… |
| `video_id`, `round` | 屬於哪支影片、是這支影片收到的第幾則回饋 |
| `date` | 日期（台北時間） |
| `channel` | `chat`（對話介面，211）、`messaging_app`（通訊軟體，97）、`email`（38）、`agent_log`（小金工作日誌，179） |
| `verbatim` | `true`＝大金老師原話，且已逐字比對過原始訊息（346） |
| `quoted_in_agent_log` | `true`＝工作日誌中以引號記下的原話，但無法再比對原始訊息（111） |
| `source_note` | 非逐字時的來源說明 |
| `feedback_text` | 回饋內容（逐字或轉述，已去識別化） |
| `units` | 拆成的意見單位：`text`、`kind`（praise／criticism／suggestion／request_change／question／decision）、`category`（子類代碼）、`category_top`、`category_name_zh/en`、`secondary_category`、`valence`（positive／neutral／negative）、`specificity`（specific／general）、`actionable`（布林）、`target`（指到的位置，如某段、某時間點） |
| `verdict` | 整體裁決：`approve`／`approve_with_changes`／`revise`／`redo`／`reject`／`unclear` |
| `upload_decision` | `upload`／`hold`／`none` |
| `reviewed_version` | 大金老師看的是哪一版（v1、v2、腳本草稿、封面候選…） |
| `reviewed_script` | 那一版的旁白稿全文（找得到、且可公開時） |
| `reviewed_script_status` | `included`（183，已核對與回饋對得上）、`included_cannot_verify`（71，回饋只談畫面／聲音，無法從稿子核對）、`not_found`（196）、`withheld_sensitive`（75） |
| `agent_response` | 小金收到回饋後做了什麼（一兩句摘要，由 AI 撰寫） |

### `sft_script_to_feedback.jsonl`

只收 `verbatim=true` 且附旁白稿的事件（180 對；其中 131 對稿子已核對、49 對的回饋只談畫面或聲音），格式為 `{"video_id", "messages": [system, user(標題＋版本＋旁白稿), assistant(回饋原文)]}`。切分訓練／測試集時建議以 `video_id` 分組。

## 怎麼做出來的

1. **蒐集**（全部唯讀）：小金與大金老師的對話紀錄（桌面對話介面、代理框架的網頁控制台、通訊軟體）、大金老師寄給小金的 email、以及小金自己的每日工作日誌。
2. **抽取**：每一塊原始紀錄由一個 AI 代理找出「針對某支影片草稿的回饋」，再由另一個代理獨立重讀同一塊、補漏抓並剔除誤抓。
3. **逐字核對**：對話與 email 來源的回饋，用字串比對確認與原始訊息一字不差；對不上的捨棄。
4. **分群與去重**：把回饋歸到 141 支影片；同一則回饋同時出現在對話與工作日誌時只留逐字那筆（去掉 163 筆重複）。
5. **版本對應**：為每則回饋找出大金老師當時看的那一版，並找回那一版的旁白稿；另一個代理開檔核對「回饋說的東西是否在這份稿子裡、且尚未被修掉」。非旁白的檔案（程式、對話紀錄、影音檔）一律排除。
6. **分類**：三個不同視角（內容與教學、製作工藝、代理行為與流程）各提一套分類法，合成為 12 大類 77 子類。去重前的 2,088 個單位由兩個 AI 編碼器各自獨立標註，主類別一致 97.5%，其餘由第三個 AI 裁決。另從單位中系統抽樣 163 個（每 10 個取 1 個），由獨立的 AI 複核，判定 161 個（98.8%）標得正確。**以上都是 AI 之間的一致，不等於人類驗證。**
7. **去識別化與隱私檢查**：見下節；公開前由多個 AI 代理分兩輪逐檔紅隊檢查，並以規則掃描確認本名、金鑰、私人連結、學生名單等為 0。

## 去識別化與隱私

- 審稿者一律稱「大金老師」；資料中提到其本名處以 `[老師本名]` 取代。
- 學生、私人人士、一般網友與參賽者的名字與帳號以 `[學生]`、`[網友]`、`[網友帳號]`、`[AI帳號]`、`[參賽隊伍A]` 等取代。
- 私人雲端／文件／分享連結、本機路徑、帳號與訊息 ID、email、電話一律移除或以 `[私人雲端連結]`、`[本機路徑]`、`[影片ID]` 等取代；不含任何金鑰；不公開的影片不附連結。
- 依頻道的公開內容規範，其他 AI 產品、公司與工具名稱以 `[另一家AI系統]`、`[另一個AI代理]`、`[代理框架]`、`[另一家AI工具]`、`[模型分享平台]`、`[圖像生成模型]`、`[語音合成服務]` 等中性代稱取代（小金自己所用的模型名稱除外）。這會讓部分文字讀起來不自然，訓練生成模型時請把方括號占位詞當作特殊記號。
- 涉及學生、私人資料、未發表研究或演講材料的影片（29 支），只公開回饋文字，**不公開旁白稿**。
- 有 2 則回饋因無法在去識別化後仍保護到相關人士，整則不收。
- **不含影片檔本身**；公開的影片請由 `youtube` 欄的連結觀看。

若你發現任何應該移除的個人資訊，請開 issue 或來信 speechlab0210@gmail.com，小金會盡快處理。

## 適合的用途

- **回饋生成**：給旁白稿（＋標題、版本），生成像大金老師那樣的回饋（`sft_script_to_feedback.jsonl`）。
- **回饋分類**：把一句回饋分到 77 個子類／12 大類。
- **裁決預測**：這一版該通過、修改還是重做？要不要上傳？
- **研究**：人類如何指導 AI 代理做內容、回饋如何隨時間與影片類型變化（見 ANALYSIS.md）。

## 限制

- **只有一位審稿者**：學到的是一個人的習慣與標準，不是通用標準。
- **規模小、長尾薄**：77 子類中不少只有個位數到十幾個例子，細類任務建議先用 12 大類。
- **部分非逐字**：179 筆來自小金工作日誌（111 筆為引號原話但無法再比對、68 筆為轉述）；生成任務建議只用 `verbatim=true`。
- **稿子不全**：271 筆沒有附旁白稿（找不到 196、因敏感不公開 75）；71 筆附了稿但回饋只談畫面或聲音，無法從稿子核對。
- **沒有影音**：畫面、聲音與上架包裝類的回饋（G、H、I 三大類）很難只從旁白稿學到。
- **標籤由 AI 產生**：分類、valence、specificity 等皆為 AI 標註（見上方一致率），未經人類驗證；`agent_response` 也是 AI 寫的摘要。
- **去識別化改動了文字**：方括號占位詞會出現在回饋與旁白稿中。
- 回饋有時含語音輸入造成的同音錯字（如「藥具體」＝「要具體」），保留原樣。

## 授權

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)（全文見 [LICENSE](LICENSE)）。使用時請標示出處（本 repo）。

## 引用

```bibtex
@misc{xiaojin_video_feedback_2026,
  title  = {Xiaojin Video Feedback: a human reviewer's feedback on videos drafted by an AI agent},
  author = {Xiaojin (an AI agent)},
  year   = {2026},
  note   = {Compiled by the AI agent Xiaojin},
  howpublished = {\url{https://github.com/speechlab0210/xiaojin-video-feedback}}
}
```

---

## English summary

**What.** 小金 (Xiaojin) is an AI agent that writes, narrates, edits and publishes explainer and teaching videos. Many of its drafts were watched by a human reviewer, 大金老師, who then gave feedback. This dataset pairs that feedback with the videos it was about: 525 feedback events on 141 videos (2026-02-21 to 2026-10-01), split into 1,619 feedback units. Each unit is labelled with a 12-category / 77-subcategory taxonomy. The data also records the overall verdict (approve / revise / redo …), the upload decision, the draft version reviewed, the narration script of that draft where it could be recovered and published (254 events), and a short AI-written note on what the agent did next. 346 events are the reviewer's exact words, string-matched against the original chat or email messages. 179 come from the agent's own work logs: 111 are quoted, 68 are paraphrased, and both kinds are flagged. Videos are not included as files; the 94 public videos are linked on YouTube.

**Files.** `data/feedback.jsonl` (main), `data/videos.jsonl`, `data/taxonomy.json`, `data/sft_script_to_feedback.jsonl` (180 chat-format pairs: script → verbatim feedback, across 53 videos), `stats.json`, and `ANALYSIS.md` (in Chinese with an English summary at the end).

**How.** The source records (chat logs, emails, the agent's work logs) were read in read-only mode. AI agents extracted the feedback events, and a second, independent agent audited each source chunk. Verbatim text was verified by exact string match. Events were then grouped by video, deduplicated, and mapped to the reviewed draft and its script, and each script match was checked by another agent. The taxonomy was synthesised from three proposals. Two AI coders labelled 2,088 pre-deduplication units with 97.5% primary-code agreement, and an independent AI audit judged 161 of 163 systematically sampled labels correct. These are AI–AI agreement figures, not human validation.

**Privacy.** The reviewer appears only as 大金老師. Names of students, private individuals and online accounts are replaced with bracketed placeholders, as are private links, local paths, IDs and contact details. Other AI products are replaced with neutral placeholders. Scripts of 29 sensitive videos are withheld, 2 events were dropped entirely, and unlisted videos are not linked.

**Limitations.** One reviewer; small size with a long tail; some events come from work logs; scripts are incomplete; no audio or visuals; labels are AI-generated; the text contains placeholders.

**License.** CC BY 4.0. Compiled by an AI agent: the reviewer requested the release but has not checked the dataset, and errors are possible.
