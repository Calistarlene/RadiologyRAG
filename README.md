# RadiologyRAG

Thesis project: comparing hybrid search and graph traversal retrieval in a knowledge-graph RAG system
for simplifying radiology report text (MedLayBench-V chest captions, UMLS 2026AA knowledge base).

## Test run (two machines, one model each)

| Person | Model | Expected time |
|---|---|---|
| Calista | `qwen2.5:1.5b` | ~2.5 h generation + ~8-12 h SummaC |
| Teammate | `llama3.2:3b` | ~7 h generation + ~8-12 h SummaC |

All settings are frozen. Do not change any cell except **S0** in `notebooks/test_run.ipynb`.

### 1. Setup (once)

1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html) and [Ollama](https://ollama.com/download).
2. Clone this repo, then in the repo folder:
   ```
   conda env create -f environment.yml
   conda activate kg-rag
   python -m ipykernel install --user --name kg-rag
   ```
3. Pull your model, e.g. `ollama pull llama3.2:3b`. S0 checks that its ID matches the one used in the pilot.
4. Unzip the data package you received privately into the repo root, so that `data/subtask_kb_build_v2/`
   and `data/MedLayBench-V/chest/` exist. The data is **not** in this repo: the knowledge base contains
   UMLS content, which may only be shared with UMLS licensees (free account at
   [UTS](https://uts.nlm.nih.gov/uts/signup-login)).

### 2. Smoke check (~5 min)

1. Open `notebooks/test_run.ipynb` with the `kg-rag` kernel.
2. In S0, set `MODEL` to your model and keep `TEST_SMOKE = True`.
3. Kernel -> Restart & Run All. It should finish with 60 answers and list the result files.

### 3. Full run

1. In S0, set `TEST_SMOKE = False`.
2. Set Windows to never sleep while plugged in, and close other heavy programs.
3. Kernel -> Restart & Run All.
4. If anything crashes or the PC restarts: Restart & Run All again. Generation (G1) and SummaC (M1)
   continue from the last saved answer.

### 4. Send back

The last cell lists these files in `results/test/`:
`test_kb_v2_<model>_generations.jsonl`, `test_kb_v2_<model>_summac.jsonl`,
`test_kb_v2_<model>_scored.csv`, `test_kb_v2_<model>_summary.csv`, `machine_<model>.json`.
