# MSSデータ解析のためのFine-tuned LLM

- MSS(Membrane-type Surface Stress Sensor, 膜型表面応力センサ)の測定データを解析するために、
LLM(Gemma3 4B)をfine-tuningしたモデルです。
---

## Model

- Gemma 3 (4B)
- LoRAを用いたFine-tuning 
- Trained for MSS sensor data analysis

---

## Code
- `make_dataset.py`

  生データから、学習のためのデータセットを整形する
- `fine_open.py`
  
  事前学習済のモデルをfine-tuningする
- `gene_open.py`
  
  fine-tuning済のモデルを用いて、匂い記述の生成、分子の分類などを行う 

---
## データセットの取り扱い

- データセットは NIMS Materials Data Repository (MDR) で公開されています。
- データ取得先（DOI）: 10.48505/nims.5556
- 本リポジトリには、当該データセットを同梱しません。
- データは必ず公式リポジトリからダウンロードしてください。
- 利用時は MDR 側の最新利用規約（商用利用可否・再配布条件など）を確認してください。

## Sample Data

- `data/Sample_data.txt`  
  以下の情報を含む:
  - 分子の濃度 
  - 受容体膜を覆う材料 
  - MSSのシグナルデータ

  このファイルは、デモ用の合成データです。

---

## Requirements

- torch  
- unsloth  
- peft  
- transformers  
- datasets  

以下を実行してください。

```bash
pip install torch unsloth peft transformers datasets
```
## How to Run
1. Prepare input data
Prepare a text file that contains molecule concentration, receptor material, and MSS measurement data.

2. Run inference

Run the following command to perform odor prediction:

```bash
python sample_code/gene_open.py --adapter_path ./adapter_weight \
                    --data_path ./data/sample_data.txt

```
The --adapter_path argument specifies the directory containing the fine-tuned LoRA adapter.
