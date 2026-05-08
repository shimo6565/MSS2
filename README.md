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

## 実行の様子 Task: MSS_TO_MOLECULE

system　プロンプト
```bash
You are an assistant that performs three types of tasks:
1. MSS_TO_MOLECULE:
   - Input: MSS sensor data and receptor type. - Output: The predicted molecule information, such as SMILES and Functional groups.
2. MOLECULE_TO_ODOR:
   - Input: Molecular information. - Output: Describe the odor of the molecule in three words or fewer.
3. MSS_TO_ODOR:
   This task is a combination of Task 1 and Task 2.
   - Input: MSS sensor data and receptor type. - Output: Describe the odor of the molecule in three words or fewer.
The user will *always* specify the task with:
"Task: MSS_TO_MOLECULE" or "Task: MOLECULE_TO_ODOR". or "Task: MSS_TO_ODOR"
Follow these rules:
- Always output ONLY the required fields for the task. - Do not include explanations. - Do not add extra text.
```
user プロンプト

```bash
Task:MSS_TO_MOLECULE {'concentration': '10%', 'receptor material': 'Z propyl STNPs: Silica-titania nanoparticles with Z propyl chains introduced on the surface', 'signal data': ['0.100', '0.200', '0.300', '0.400', '0.500', '0.600', '0.700', '0.800', '0.900', '1.000', '0.933', '0.867', '0.800', '0.733', '0.667', '0.600', '0.533', '0.467', '0.400', '0.333', '0.311', '0.289', '0.267', '0.244', '0.222', '0.200', '0.178', '0.156', '0.133', '0.111', '0.104', '0.097', '0.090', '0.083', '0.076', '0.069', '0.062', '0.055', '0.048', '0.041']}
```
assitant (出力結果)
```bash
{'SMILES': '+OM\310C(=O)OC1=CC=CC=C1N', 'Molecular weight': '160.20', 'Number of oxygen atoms': '2', 'Number of nitrogen atoms': '1', 'Contains ring': True, 'Contains double bond': True, 'Functional groups': ['Ketone']}
```
