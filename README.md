# 基於多版本YOLO 與投票策略之Trichophyton 菌種顯微影像鑑別研究 
**Microscopic Image Classification of Trichophyton Species Based on Multiple YOLO Versions and Ensemble Voting Methods**

## 📌 Project Overview

本研究旨在結合多版本 YOLO（You Only Look Once）物件偵測模型與投票策略，針對皮癬菌（*Trichophyton*）顯微影像進行菌種鑑別，研究重點為 *Trichophyton interdigitale* 與 *Trichophyton rubrum*。

由於不同菌種及其形態特徵可能存在相似性，單一模型在顯微影像辨識上可能面臨特徵混淆與分類不穩定等問題。因此，本研究比較不同 YOLO 版本、模型規模及任務架構的辨識表現，並進一步探討物件偵測結果與多模型投票策略在影像層級菌種判定上的應用。

本專案涵蓋顯微影像資料整理、形態標註、模型訓練、效能評估與投票決策，期望建立一套可供比較與延伸的深度學習影像辨識研究流程。

## 🎯 Research Objectives

- **Multi-Version YOLO Comparison**：比較 YOLOv8、YOLOv9、YOLOv10 與 YOLOv11 在皮癬菌顯微影像辨識任務上的表現。
- **Morphological Feature Detection**：辨識不同菌種所對應的顯微形態特徵，評估模型對局部影像特徵的辨識能力。
- **Detection Task Comparison**：探討一般物件偵測（Detection）、旋轉框物件偵測（OBB）及影像分割（Segmentation）等方法的應用。
- **Voting Strategy**：整合多模型的預測資訊，研究不同投票與分數整合策略對影像層級菌種判定的影響。
- **Image-Level Identification**：將模型對局部形態的辨識結果整合為整張顯微影像的菌種判定。

## 🦠 Target Species

| Species | Scientific name |
|---|---|
| *T. interdigitale* | *Trichophyton interdigitale* |
| *T. rubrum* | *Trichophyton rubrum* |

## 🗂️ Dataset

本研究使用高雄榮民總醫院（Kaohsiung Veterans General Hospital）於 2021 至 2023 年間取得的皮癬菌顯微影像作為研究資料。

### Dataset Statistics

| Dataset category | Initial images | Additional images | Total before cleaning |
|---|---:|---:|---:|
| *T. interdigitale* | 97 | 122 | 219 |
| *T. rubrum* | 451 | 154 | 605 |
| **Total** | **548** | **276** | **824** |

資料經過清理後，研究紀錄中的有效影像數量分別為：

- *T. interdigitale*：84 張
- *T. rubrum*：350 張

清理後的影像數量合計為 434 張。實際訓練、驗證與測試所使用的影像數量，應以各實驗版本的最終資料清單為準。

### Morphological Labels

本研究依據菌種及形態特徵建立五個標註類別：

| Class ID | Label | Description |
|---:|---|---|
| 0 | `inter_mac` | *T. interdigitale* — macroconidia-related morphology |
| 1 | `inter_small` | *T. interdigitale* — small morphological features |
| 2 | `inter_spiral` | *T. interdigitale* — spiral morphology |
| 3 | `rubrum_big` | *T. rubrum* — large morphological features |
| 4 | `rubrum_small` | *T. rubrum* — small morphological features |

上述英文描述為類別名稱的概括性說明；正式形態定義與標註判準應以論文中的標註規範為準。

### Data Splitting

研究採用以工作編號（work ID）為單位的資料切分方式，比例為：

- Training set：70%
- Validation set：15%
- Test set：15%

以工作編號進行切分，可降低同一工作來源的影像同時出現在不同資料子集的風險，讓模型評估更貼近對不同來源樣本的泛化能力。

> **Data privacy:** 本專案 README 不包含原始臨床顯微影像。資料的公開、分享及再利用應遵循資料來源機構的授權與隱私規範。

## 🧠 Models and Methods

### YOLO Model Versions

本研究比較下列 YOLO 模型系列：

- YOLOv8
- YOLOv9
- YOLOv10
- YOLOv11

研究中亦比較不同模型規模，例如 YOLOv9s 與 YOLOv11l，分析模型容量與辨識表現之間的關係。

### Model Tasks

| Task | Abbreviation | Purpose |
|---|---|---|
| Object Detection | Detection | 定位並辨識影像中的形態特徵 |
| Oriented Bounding Box | OBB | 以具方向性的旋轉框描述目標位置與角度 |
| Instance Segmentation | Segmentation | 以像素層級輪廓描述目標形態 |

並非所有模型版本都必然採用完全相同的任務架構；實際比較組合以研究中的實驗設定為準。

### Voting Strategies

本研究進一步探討如何整合模型預測，將局部目標的辨識結果轉換為影像層級的菌種判定。

主要研究方向包括：

1. **Model Prediction Integration**：整合多個模型的預測結果。
2. **Confidence Score Aggregation**：利用預測信心分數進行整合。
3. **Image-Level Voting**：根據單張影像中的辨識結果進行菌種判定。
4. **Soft-Sum Strategy**：加總或整合模型輸出的分數，作為影像層級決策的依據。

實際投票規則、分數正規化方式、各模型權重及平手處理方式，應以論文實作為準。

## 🏆 Experimental Results

根據本研究目前整理的實驗結果，以下列出兩項具有代表性的結果：

| Experiment | Model / Strategy | Reported result |
|---|---|---:|
| Work-ID evaluation | YOLOv9s | 0.875 |
| Image-level Soft-Sum evaluation | YOLOv11l-OBB | 0.903 |

其中，YOLOv9s 在 work-ID 評估設定下的紀錄為 0.875，而 YOLOv11l-OBB 搭配 image-level Soft-Sum 策略的紀錄為 0.903。

**Interpretation:** 這些結果顯示，不同模型架構與影像層級的分數整合策略，可能對最終菌種判定產生不同影響。然而，兩項數值來自不同的評估設定，不應直接視為同一基準下的公平比較。

正式發表或重現實驗時，應明確標示指標定義、資料子集、模型設定與評估方式。若數值代表 Accuracy，應以論文中的實際計算公式與實驗表格為準，不應僅由數值推定。

## 🔬 Research Workflow

```text
Microscopic Image Collection
            |
            v
       Data Cleaning
            |
            v
 Morphological Annotation
            |
            v
  Work-ID-Based Data Split
       70 / 15 / 15
            |
            v
  YOLO Model Training
  v8 / v9 / v10 / v11
            |
            v
 Detection / OBB / Segmentation
            |
            v
   Model Evaluation
            |
            v
 Voting and Score Aggregation
            |
            v
  Image-Level Species Decision
            |
            v
 Comparative Experimental Analysis
```

## 🛠️ Technology Stack

| Category | Technology |
|---|---|
| Object Detection | YOLOv8, YOLOv9, YOLOv10, YOLOv11 |
| Computer Vision | Microscopic Image Analysis |
| Model Tasks | Detection, OBB, Segmentation |
| Deep Learning | Python-based deep learning workflows |
| Experiment Design | Work-ID-based data splitting |
| Decision Strategy | Voting and Soft-Sum aggregation |
| Thesis Writing | Overleaf, XeLaTeX |
| Version Control | Git, GitHub |

實際使用的 Python、PyTorch、Ultralytics 及其他套件版本，請以各模型實驗環境中的設定檔為準。

## 📁 Repository Structure

以下為建議的專案目錄配置，可依實際程式碼與實驗檔案調整。

```text
.
├── README.md
├── configs/
│   ├── dataset.yaml
│   └── model_configs/
├── datasets/
│   └── README.md
├── scripts/
│   ├── train.py
│   ├── validate.py
│   └── predict.py
├── voting/
│   └── README.md
├── experiments/
│   └── README.md
├── results/
│   └── README.md
├── requirements.txt
└── .gitignore
```

**Note:** 上述為建議的目錄結構，並非表示所有檔案均已實作或包含於目前的 Repository。`datasets/` 不應直接放入未獲授權公開的原始臨床影像。

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_NAME>
```

### 2. Set Up the Environment

建議使用獨立的 Python 虛擬環境，並依據實驗所使用的套件版本進行安裝。

```bash
python -m venv .venv
```

啟用虛擬環境：

**Windows**

```bash
.venv\Scripts\activate
```

**Linux / macOS**

```bash
source .venv/bin/activate
```

若 Repository 已提供 `requirements.txt`，可執行：

```bash
pip install -r requirements.txt
```

### 3. Prepare the Dataset

請依據資料集設定檔，準備具有相對應標註格式的影像資料。

對於 YOLO Detection、OBB 與 Segmentation 任務，標註格式及資料夾結構可能不同，必須與所使用的模型及訓練程式相符。

### 4. Train and Evaluate Models

模型訓練、驗證、推論與投票策略的實際執行指令，請依照 Repository 中的程式碼與設定檔補充。

本專案的重點是比較不同模型及決策策略，而不是假設所有模型都能使用完全相同的訓練參數。

## 📊 Evaluation and Reproducibility

為提升實驗的可重現性，建議保存以下資訊：

- Model version and model size
- Detection task configuration
- Dataset version and split identifiers
- Training hyperparameters and random seeds
- Evaluation metrics and confusion matrices
- Voting rules and score aggregation settings
- Model checkpoints and inference outputs
- Python, PyTorch, CUDA and dependency versions

若需要進行嚴謹的模型比較，應盡量固定資料切分、評估指標與測試條件，並確保不同實驗的比較基準一致。

## ⚠️ Limitations

1. **Dataset distribution:** 研究資料來自特定機構及期間，資料分布可能與其他醫療機構或顯微鏡成像條件不同。
2. **Class imbalance:** 清理後兩種菌種的影像數量仍不相同，評估時應同時考慮各類別表現。
3. **Generalization:** 單一資料來源的實驗結果不等同於跨機構的外部驗證。
4. **Morphological ambiguity:** 顯微形態可能存在相似性，最終辨識結果仍應由合格專業人員依適當診斷流程判讀。
5. **Reproducibility:** 若未公開原始資料、完整標註、訓練設定及模型權重，其他研究者可能無法完整重現所有實驗。

本研究為學術研究用途，模型輸出不應直接作為獨立的臨床診斷依據。

## 🔭 Future Work

後續可從以下方向延伸：

- **External Validation:** 使用其他機構或不同成像條件的資料評估模型泛化能力。
- **Robustness Analysis:** 分析影像品質、模糊、光照與染色差異對模型的影響。
- **Voting Optimization:** 比較不同投票策略、分數整合方法及模型權重。
- **Explainability:** 透過視覺化方法分析模型關注的顯微形態區域。
- **Model Efficiency:** 評估推論速度、模型大小與部署成本。
- **Reproducible Research:** 整合資料版本、訓練設定、實驗指標及自動化評估流程。

## 📚 Academic Information

**Thesis Title:** 基於多版本 YOLO 與投票策略之 *Trichophyton* 菌種顯微影像鑑別研究

**Research Domain:** Computer Vision, Object Detection, Deep Learning, Medical Image Analysis

**Writing Environment:** Overleaf with XeLaTeX

**Dataset Source:** Kaohsiung Veterans General Hospital, 2021–2023

## 📄 Citation

若此專案已有正式學位論文或公開論文資訊，請在此補上正式書目資料。

```bibtex
@mastersthesis{trichophyton_yolo_voting,
  title  = {基於多版本 YOLO 與投票策略之 Trichophyton 菌種顯微影像鑑別研究},
  author = {Author Name},
  school = {National Sun Yat-sen University},
  year   = {2025}
}
```

上述 BibTeX 為待補完整資料的範本；請依學校正式論文紀錄確認作者姓名、正式英文題名、系所及出版資訊。

## 📜 License

本專案的程式碼、模型權重、標註資料及研究影像可能具有不同的授權條件。請依實際權利歸屬及資料使用許可設定 License。

在確認授權前，不應將第三方資料、臨床影像或受限制的模型權重視為可自由散布的內容。

---

**Keywords:** `YOLO` `YOLOv8` `YOLOv9` `YOLOv10` `YOLOv11` `Object Detection` `OBB` `Segmentation` `Voting Strategy` `Soft-Sum` `Medical Image Analysis` `Trichophyton interdigitale` `Trichophyton rubrum`

