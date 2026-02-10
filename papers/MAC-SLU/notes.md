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
* **The Mapping Process:**
    $$\text{User Speech} \xrightarrow{\text{SLU}} \text{Structured Semantics (Commands)}$$
    (目標是將非結構化的語音訊號，轉化為機器可執行的結構化指令)

#### 2. Key Applications (主要應用場景)
目前廣泛應用於需要「免手持操作 (Hands-free)」的場景：
* 🏠 **Smart Homes (智慧家居):** e.g., "Turn off the lights."
* 🚗 **Automobiles (車載系統):** e.g., "Navigate to the nearest gas station."

#### 3. Purpose (目的)
為了執行 **Downstream Tasks (下游任務)**。
* SLU 不是終點，而是手段。它的準確度直接決定了系統能否正確執行使用者的意圖 (Execute spoken commands)。

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
    * **Error Propagation (錯誤傳播):** 如果 ASR 在轉錄階段發生錯誤（聽錯），NLU 階段就幾乎不可能正確分析語意。

#### 2. End-to-End (E2E) SLU (端對端式)
為了克服傳統流水線方法的缺陷而出現的新架構。

* **Core Objective (核心目標):**
    * 解決 ASR 轉錄過程帶來的 **Error Propagation (錯誤傳播)** 問題。
* **Methodology (方法論):**
    * 不再依賴中間的文字轉錄，嘗試直接從音訊特徵映射到語意。
    * 經常結合 **Pre-trained Language Models (PLMs)**（文中舉例：**RoBERTa**）來增強模型的語意理解效能。

### 🚩 Critical Challenges in LLM-based SLU (LLM 應用於 SLU 的當前挑戰)
> Presently, the advancement of LLMs and LALMs offers the potential for a more flexible and precise SLU. Nevertheless, research on LLM-based SLU still confronts the following two challenges: (1) Existing SLU datasets lack sufficient diversity and complexity. The widely used ATIS and SNIPS datasets contain only 16 and 7 intents, respectively, which limits the task’s difficulty, resulting in existing models already achieving over 95% accuracy in both IC and SF. SLURP dataset increases the number of intent and slot categories but remains confined to single-intent SLU tasks. (2) There is a lack of a unified benchmark for state-of-the-art (SOTA) open-source LLMs and LALMs. Although some studies have preliminarily explored the performance of LLMs like ChatGPT and Llama on certain SLU tasks, they have adopted different task formats (i.e., varying data formats, prompts, or training and alignment methods), leading to evaluation
results that cannot be fairly compared. Furthermore, existing research has been limited to pipeline-based methods and LLMs, without exploring E2E approaches and the most advanced LALMs.

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
