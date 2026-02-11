# MAC-SLU: Multi-Intent Automotive Cabin Spoken Language Understanding Benchmark

> **💡 Meta Information**
>
> * **Venue:** `arXiv 2025` 
> * **Paper Type:** Benchmark
> * **Links:** [Paper (arXiv)](https://arxiv.org/abs/2512.01603) | [Code](https://github.com/Gatsby-web/MAC_SLU) | [Dataset](https://huggingface.co/datasets/Gatsby1984/MAC_SLU)
> * **Institutions:** Shanghai Jiao Tong University, AISpeech Co., Ltd., et al.
>
> **🏷️ Domains & Tasks**
> * `Spoken Language Understanding` `Multi-Intent Understanding` `LLMs` `Large Audio Language Models`
> * **Core Tasks:** Intent Classification, Multi-Intent Detection

### 📌 Citation
```text
Peng, Y., Cai, C., Liu, Z., Fan, S., Jiang, S., Xu, H., et al. (2025). 
MAC-SLU: Multi-Intent Automotive Cabin Spoken Language Understanding Benchmark. 
arXiv preprint arXiv:2512.01603.
```

## Introduction

### 📝 Spoken Language Understanding (SLU) - Core Definition
> Spoken Language Understanding (SLU) is a conventional paradigm for task-oriented spoken semantics extraction, which has been
widely applied in scenarios such as smart homes and automobiles to extract spoken commands from users for executing downstream tasks.

#### 1. Definition (定義)
SLU 是 **Task-oriented (任務導向)** 對話系統中的核心範式，負責進行 **Spoken Semantics Extraction (口語語意提取)**。
* **The Mapping Process:** `User Speech` --(SLU)--> `Structured Semantics (Commands)`
    (目標是將非結構化的語音訊號，轉化為機器可執行的結構化指令)

#### 2. Key Applications (主要應用場景)
目前廣泛應用於需要「免手持操作 (Hands-free)」的場景：
* 🏠 **Smart Homes (智慧家居):** e.g., "Turn off the lights."
* 🚗 **Automobiles (車載系統):** e.g., "Navigate to the nearest gas station."

#### 3. Purpose (目的)
為了執行 **Downstream Tasks (下游任務)**。
* SLU 不是終點，而是手段。它的準確度直接決定了系統能否正確執行使用者的意圖。

### 📝 SLU Architecture Evolution (SLU 架構演進)
> Traditional SLU employs a pipeline approach, first transcribing the user’s spoken query into text via Automatic Speech Recognition (ASR), and then extracting semantic information through Natural Language Understanding (NLU), which typically includes Intent Classification (IC) and Slot Filling (SF). Subsequently, E2E SLU systems emerged to address the issue of error propagation from the ASR transcription process and potentially incorporate pre-trained language models (e.g., RoBERTa) to enhance performance.

#### 1. Traditional Pipeline Approach (傳統流水線式)
這是最經典的處理方式，將任務拆解為兩個獨立階段：

* **Workflow (工作流程):** `Audio` --(ASR)--> `Text` --(NLU)--> `Semantics`
* **Key Components (核心組件):**
    1.  **ASR (Automatic Speech Recognition):** 將使用者的語音查詢轉錄為文字。
    2.  **NLU (Natural Language Understanding):** 從轉錄的文字中提取語意資訊，通常包含兩個子任務：
        * **IC (Intent Classification):** 意圖分類。
        * **SF (Slot Filling):** 關鍵詞槽位填充。
* **Critical Issue (關鍵問題):**
    * **Error Propagation (錯誤傳播):** 如果 ASR 在轉錄階段發生錯誤，NLU 階段就幾乎不可能正確分析語意。

#### 2. End-to-End (E2E) SLU (端對端式)
為了克服傳統流水線方法的缺陷而出現的新架構。

* **Core Objective (核心目標):**
    * 解決 ASR 轉錄過程帶來的 **Error Propagation (錯誤傳播)** 問題。
* **Methodology (方法論):**
    * 不再依賴中間的文字轉錄，嘗試直接從音訊特徵映射到語意。
    * 經常結合 **Pre-trained Language Models (PLMs)**（文中舉例：**RoBERTa**）來增強模型的語意理解效能。

### 🚩 Critical Challenges in LLM-based SLU (LLM 應用於 SLU 的當前挑戰)
> Presently, the advancement of LLMs and LALMs offers the potential for a more flexible and precise SLU. Nevertheless, research on LLM-based SLU still confronts the following two challenges: (1) Existing SLU datasets lack sufficient diversity and complexity. The widely used ATIS and SNIPS datasets contain only 16 and 7 intents, respectively, which limits the task’s difficulty, resulting in existing models already achieving over 95% accuracy in both IC and SF. SLURP dataset increases the number of intent and slot categories but remains confined to single-intent SLU tasks. (2) There is a lack of a unified benchmark for state-of-the-art (SOTA) open-source LLMs and LALMs. Although some studies have preliminarily explored the performance of LLMs like ChatGPT and Llama on certain SLU tasks, they have adopted different task formats (i.e., varying data formats, prompts, or training and alignment methods), leading to evaluation results that cannot be fairly compared. Furthermore, existing research has been limited to pipeline-based methods and LLMs, without exploring E2E approaches and the most advanced LALMs.

儘管 LLM 和 LALM 展現了極大的潛力，但目前的研究面臨兩大核心痛點：

#### 1. Dataset Limitation: Lack of Complexity (資料集的侷限性)
現有的 SLU 資料集過於簡單，無法反映真實場景的複雜度，導致模型能力看起來「虛高」。
* **Saturated Benchmarks (已飽和的基準):**
    * 常用資料集如 `ATIS` (17 intents) 和 `SNIPS` (7 intents) 分類過少。
    * **Consequence:** 現有模型在 IC (Intent Classification) 與 SF (Slot Filling) 上準確率均已超過 **95%**，難以區分新模型的優劣。
* **Complexity Gap:**
    * 雖然 `SLURP` 增加了類別數量，但仍侷限於 **Single-Intent (單一意圖)** 任務，缺乏多意圖 (Multi-Intent) 的挑戰。

#### 2. Evaluation Crisis: Lack of Unified Benchmark (缺乏統一評測標準)
目前的 SOTA 模型評測處於「各自為政」的狀態，缺乏公平的比較基準。
* **Inconsistent Protocols:**
    * 現有研究雖然測試了 ChatGPT 或 Llama，但使用了不同的 Data Formats、Prompts 或 Alignment methods。
    * **Result:** 評測結果無法進行公平的橫向對比 (Unfair comparison)。
* **Scope Limitation:**
    * 目前研究多集中在 **Pipeline-based** 方法，忽略了 **End-to-End (E2E)** 方法與最先進 **LALMs** 的潛力探索。

---

💡 Keep in Mind
這段話其實是在為作者自己的貢獻 **(Contribution)** 鋪路。讀到這裡，你應該預期這篇論文接下來會提出：
1.  一個更難、更多樣化（可能包含 Multi-Intent）的新資料集。
2.  一個統一的 Benchmark，同時評測 Pipeline vs. E2E 以及 LLM vs. LALM。

---
### 🚀 Core Contributions (核心貢獻)
> This work addresses these challenges with two steps. First, we introduce the Multi-Intent Automotive Cabin Spoken Language Understanding (MAC-SLU) dataset, a novel Chinese SLU corpus to overcome the complexity limitations of existing data. Derived from real-world automotive text commands with TTS-synthesized speech, it spans 8 domains, 81 intents, 192 slots, and includes multi-intent queries with up to 5 intents, creating a more rigorous testbed. Second, we establish a unified benchmark on MAC-SLU for SOTA
open-source LLMs and LALMs. Standardizing formats, tasks, and evaluation methods enables fair comparisons and provides a dependable reference for the community.

這篇論文提出了兩大解決方案來應對現有 SLU 研究的不足：

#### 1. The MAC-SLU Dataset (全新的資料集)
為了解決現有資料集缺乏複雜度與多樣性的問題，作者發布了 **Multi-Intent Automotive Cabin Spoken Language Understanding (MAC-SLU)**。
* **Language:** Chinese (中文語料)。
* **Data Source:** 源自真實世界的車載文本指令，並透過 TTS (Text-to-Speech) 合成語音。
* **Scale & Complexity:**
    * **8** Domains (領域)
    * **81** Intents (意圖)
    * **192** Slots (槽位)
* **Key Innovation:** 包含 **Multi-intent queries (多意圖查詢)**，單句最多包含 **5** 個意圖，建立了一個更嚴苛的測試平台 (Rigorous testbed)。

#### 2. Unified Benchmark (統一的評測基準)
針對 SOTA 開源 LLMs 和 LALMs 建立了統一的評測標準。
* **Standardization:** 統一了資料格式 (Formats)、任務定義 (Tasks) 與評估方法 (Evaluation methods)。
* **Goal:** 實現 **Fair Comparisons (公平比較)**，為社群提供可信賴的參考依據。

> Our contributions include:
> - We introduce MAC-SLU, a novel Chinese multi-intent SLU dataset including complex, multi-intent queries from a real-world automotive cabin domain. MAC-SLU enables a more chanllenging evaluation of the latest LLMs and LALMs.
> - We provide a comprehensive benchmark for SOTA open-source and closed-source LLMs and LALMs, encompassing methods based on direct inference, in-context learning, and SFT, as well as both pipeline and E2E SLU task paradigms.
> - Our experiments demonstrate that (1) existing LLMs and LALMs can complete parts of the IC or SF tasks through in-context learning, yet there remains a significant performance gap compared to in-domain SFT; (2) benefiting from the avoidance of error propagation, current LALMs can already achieve performance comparable to pipeline methods.

#### 1. Dataset Contribution: MAC-SLU
作者發布了一個全新的 **Chinese Multi-Intent SLU Dataset**。
* **Domain:** Real-world Automotive Cabin (真實車載場景)。
* **Characteristics:** 包含 **Complex, Multi-intent queries (複雜多意圖查詢)**。
* **Value:** 相比於現有的簡單資料集，它為最新的 LLMs 和 LALMs 提供了更具挑戰性的評測環境。

#### 2. Comprehensive Benchmark (全面評測基準)
建立了一個涵蓋 **SOTA Open-source & Closed-source** 模型的評測基準。
* **Models Covered:** LLMs (Text-only) & LALMs (Audio-Text).
* **Methods Evaluated:**
    * Direct Inference (Zero-shot)
    * In-Context Learning (Few-shot)
    * Supervised Fine-Tuning (SFT)
* **Paradigms:** 同時評測了 **Pipeline (ASR+NLU)** 與 **E2E (End-to-End)** 兩種 SLU 架構。

#### 3. Key Experimental Insights (關鍵實驗結論)
這部分揭示了目前技術的邊界 (Boundaries)：
* **Gap in Learning Paradigms:**
    $$\text{Performance}\_{\text{SFT}} \gg \text{Performance}\_{\text{ICL}}$$
    * 雖然 LLMs/LALMs 可以透過 In-context Learning (ICL) 完成部分 IC 或 SF 任務，但與 **In-domain SFT** 相比仍有顯著差距。
* **Rise of LALMs (E2E Potential):**
    目前的 LALMs (Large Audio Language Models) 已經能達到與 Pipeline 方法 **Comparable (相當)** 的效能。
    * **Reason:** 歸功於 **Avoidance of Error Propagation** (E2E 架構避免了 ASR 轉錄錯誤傳遞到 NLU 的問題)。
 
---
## 📊 Dataset Construction & Statistics (MAC-SLU 資料集建構)

<img width="1051" height="163" alt="image" src="https://github.com/user-attachments/assets/c7265932-c3af-453f-ab70-5152acb67f3d" />

### Text Data Collection

#### 1. Data Origin (數據來源)
* **Source:** Real-world Automotive Cabin Scenarios (真實世界的車載場景)。
* **Format:** Transcribed texts of Chinese spoken commands (中文語音指令轉錄文本)。
* **Scale:** 原始收集了超過 **20,000** 條語料及其對應的 SLU 解析結果。

#### 2. Data Partitioning & Annotation Strategy (數據劃分與標註策略)
作者採用了 **Weakly Labeled (弱標註)** 用於訓練，**Human Annotated (人工精標)** 用於測試的策略，這是工業界常見的做法。

| Split | Size (Samples) | Label Quality | Method |
| :--- | :--- | :--- | :--- |
| **Training Set** | **17,997** | Weakly Labeled | 隨機選取，直接使用現有的 SLU 解析結果作為標籤。 |
| **Validation Set**| **1,391** | Weakly Labeled | 同上。 |
| **Test Set** | **1,152** | **Clean / Gold** | 經過 3 位標註者人工審查與清洗。 |

#### 3. Rigorous Cleaning Process for Test Set (測試集清洗流程)
為了確保 Evaluation 的準確性，作者對測試集進行了嚴格的篩選：
* **Initial Pool:** 隨機抽取 1,800 樣本。
* **Process:** 3 位標註者進行審查，移除或修正錯誤 (Parsing errors) 與空意圖 (Blank intents)。
* **Retention Rate:** $1800 \to 1152$ (約保留 64%)。
    * *Insight:* 這個篩選比例暗示原始數據中存在一定比例的噪聲 (Noise)，因此人工清洗對於建立可靠的 Benchmark 至關重要。

### Speech Data Generation

為了構建語音資料集，作者並未重新錄製人聲，而是採用了先進的 TTS 技術進行合成，並透過嚴謹的策略確保了語者的多樣性與實驗的公平性。

#### 1. Core Technology (核心技術)
* **TTS Model:** 使用 **CosyVoice-2** 模型進行語音合成。
* **Target Language:** Mandarin Chinese。

#### 2. Speaker Embedding Source (語者特徵來源)
為了讓合成的語音聽起來像不同的人（而不是只有一個機器人聲音），作者使用了 **AIShell-1**（一個廣泛使用的中文 ASR 資料集）作為語者特徵的來源。

#### 3. Synthesis Strategy & Data Isolation (合成策略與資料隔離)
這部分是實驗設計的亮點，旨在避免**資料洩露 (Data Leakage)**：
* **Template Extraction:** 從 AIShell-1 的 `train`, `dev`, `test` 三個集合中，隨機選取音訊片段，構建出三組獨立的 **Speaker Templates (語者模板)**。
* **Strict Isolation:**
    * 合成 MAC-SLU 的訓練集時 $\to$ 只使用 AIShell-1 訓練集的語者模板。
    * 合成 MAC-SLU 的測試集時 $\to$ 只使用 AIShell-1 測試集的語者模板。
* **Goal:**
    1. **Ensure Diversity:** 確保語料庫中包含豐富的語者特徵。
    2. **Maintain Isolation:** 確保模型在測試時遇到的「語者」是訓練時從未聽過的 (Unseen Speakers)，符合真實場景需求。
 
## Experiments

<img width="943" height="499" alt="image" src="https://github.com/user-attachments/assets/4df26d54-d97b-49d9-9840-34f81d3cf92b" />

#### 1. In-Context Learning (ICL) Performance
作者驗證了 LLM 與 LALM 透過 Few-shot Prompting 完成 SLU 任務的能力。

* **Capabilities (能力驗證):**
    * **IC (Intent Classification):** 確認了 LLMs 具備完成此任務的潛力，並將結論延伸至最新的 LALMs。
    * **SF (Slot Filling) - The Surprise:** 透過精心設計的 Prompt ，模型在 SF 任務上的表現超出預期。
        * *Text Input F1:* **55.09%**
        * *Speech Input F1:* **47.38%**
        * *Comparison:* 顯著優於前人研究報告的 13.35%。
* **The "Reality Check" (現實的骨感):**
    儘管 F1 看起來不錯，但 **Overall Accuracy (整體準確率)** 即使是最強的模型 (Qwen3-32B, GPT-4o-Audio) 也 **未超過 15%**。
    * *Insight:* 這再次證明了 **MAC-SLU** 資料集的難度與複雜性（Multi-intent 帶來的挑戰）。

#### 2. E2E LALMs vs. Pipeline Systems (端對端 vs. 流水線)
這是本篇論文最關鍵的架構對比發現。

* **Comparable Performance (效能相當):**
    E2E LALMs 的表現已經可以媲美甚至超越同量級的 Pipeline 系統。
    * **Evidence:** `Qwen2.5-Omni-7B` (E2E) $\succ$ `Whisper + Qwen3-8B` (Pipeline)。
    * **Gains:** IC +2%, SF +1%。
* **Why E2E Wins? (核心原因):**
    * **Avoidance of ASR Error Propagation:** E2E 模型直接處理音訊，避免了 Pipeline 中 ASR 聽錯導致 NLU 接著錯的連鎖反應。

#### 3. Scaling & Model Types (模型規模與類型)
* **Size Matters (Scaling Law):**
    雖然 7B 的 E2E 贏了 8B 的 Pipeline，但仍落後於更大的 `Qwen3-32B`。
    * *Implication:* 未來將 LALM 擴大參數規模 (Scaling up)，有巨大的效能提升空間。
* **Closed vs. Open Source:**
    * `GPT-4o-Audio` & `Gemini-2.5-Flash` (閉源) 目前在整體準確率上仍優於開源 LALMs。
 
<img width="452" height="326" alt="image" src="https://github.com/user-attachments/assets/6f347a1d-3923-4bd6-a4b4-d326e7a1d36e" />

#### 1. SFT vs. ICL: The Gap is Massive (SFT 的絕對統治力)
實驗數據無情地證明，在特定領域任務（In-domain SLU）上，微調 (SFT) 仍然遠強於提示工程 (In-Context Learning)。

* **Performance Jump:** 以 `Qwen2.5-Omni-7B` 為例，SFT 帶來的提升是巨大的：
    * **IC Accuracy:** $\uparrow$ **29%**
    * **SF F1 Score:** $\uparrow$ **39%**
    * **Overall Accuracy:** $\uparrow$ **47%** (幾近翻倍的成長)
* **Conclusion:** 目前要達到 SLU 任務的最佳效能 (Optimal Performance)，**Fine-tuning 仍是唯一解**。期待 LLM Agent 僅靠 ICL 自主完成複雜 SLU 任務仍是一個巨大的挑戰。

#### 2. The "Achilles' Heel" of Pipeline Systems: Error Propagation (流水線系統的阿基里斯腱)
這段數據量化了 ASR 錯誤如何毀掉一個強大的 LLM。

* **Baseline (Ideal Case):**
    * 當輸入是完美的文字 (Gold Text, CER=0%) 時，`Qwen3-8B` 表現最好，Overall Accuracy 達到 **60.73%**。
* **Reality (ASR Errors):**
    一旦接上 ASR，效能就會因為 **Character Error Rate (CER)** 的上升而劇烈下降：
    * **Paraformer (CER=3.64%):** 準確率下降 **>13%**。
    * **Whisper (CER=10.40%):** 準確率下降 **>25%** (直接腰斬)。
* **Insight:**
    $$\text{High CER} \xrightarrow{\text{Propagates}} \text{Low NLU Accuracy}$$
    這解釋了為什麼 Pipeline 方法雖然很強，但在端對端評測中往往輸給 LALMs——因為 **LALMs 沒有 ASR 轉錄這個中間環節，天生免疫轉錄錯誤**。

### Qualitative Analysis: The "False Negative" Problem (定性分析：被誤判的錯誤)

<img width="919" height="344" alt="image" src="https://github.com/user-attachments/assets/e3c1b93e-92a1-46f0-b1ea-13c3744bba68" />

#### 1. The Phenomenon: Lexical Variation (用詞差異)
作者在分析錯誤案例時發現，大量的所謂「錯誤」其實是被冤枉的。
* **Observation:** 模型輸出的答案在 **語意上是正確的 (Semantically Correct)**，只是**措辭 (Phrasing)** 跟標準答案（Label）不一樣。
* **Example:**
    * *Label:* "Open the window."
    * *Model Output:* "Please open the window."
    * *Metric Result:* **Error (X)** (因為字串不完全匹配)。

#### 2. Functional Correctness vs. Exact Match (功能正確性 vs. 精確匹配)
從 SLU 的應用目標來看，這些輸出其實是合格的。
* **Goal:** SLU 的目的是提取語意供下游應用執行 (Execute downstream tasks)。
* **Verdict:** 只要語意對了，機器就能執行正確動作。因此，這些被判錯的樣本在真實應用中其實是 **Functionally Correct (功能上正確的)**。

#### 3. Conclusion: Underestimation of Capability (能力被低估)
這導出了一個關鍵結論：
$$\text{Standard Metrics (Exact Match)} \xrightarrow{\text{Leads to}} \text{Underestimation}$$
依賴「精確字串匹配」的傳統評測標準，嚴重低估了 LALM 真實的意圖理解能力。因為 LALM 是生成式模型，天生就比傳統分類模型更具創造性與多樣性。

---
### 🎓 Conclusion & Future Directions (結論與未來方向)

#### 1. Summary of Contributions (貢獻總結)
* **New Dataset (MAC-SLU):** 發布了一個針對車載場景 (Automotive Cabin) 的 **Chinese Multi-Intent SLU** 資料集，解決了現有資料集缺乏多樣性與複雜度的問題。
* **Unified Benchmark:** 建立了一個統一的評測基準，讓 Open-source LLMs 與 LALMs 能在同一個標準下進行比較。

#### 2. Key Takeaways (核心結論)
* **SFT is Key:** 雖然 In-context learning (ICL) 展現了潛力，但在追求 **Optimal Performance (最佳效能)** 時，**In-domain SFT (領域內微調)** 仍然是不可或缺的關鍵。
* **E2E Potential:** 端對端 (E2E) LALMs 的表現已經可以媲美傳統的 Pipeline 方法。
    * *Reason:* 有效減輕了 **ASR Error Propagation (轉錄錯誤傳播)** 的影響。

#### 3. Future Work (未來展望)
作者指出了接下來的研究重點：
* **Enhancing ICL:** 提升模型在 In-context learning 下的能力（讓模型不用微調也能表現更好）。
* **Semantically-aligned Metrics:** 開發更符合語意對齊的評估指標（不再只看字串是否完全匹配，而是看意思對不對）。
