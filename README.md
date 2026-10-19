# Thai-HalluEval: A Benchmark for Evaluating LLM Hallucination in Localized Thai Knowledge

Thai-HalluEval is a 150-question benchmark that tests whether large language models answer factual questions about Thailand correctly. It covers three domains (Thai history, Thai domestic law and Thai cultural norms), each with 50 questions and a reference answer verified against primary Thai sources.

The benchmark was built for the MSc Linguistics and AI dissertation *"Global Models, Local Blind Spots: Evaluating LLM Hallucination in Thai Knowledge"* (Queen Mary University of London, 2026; supervisor: Linnaea Stockall).

## Results

Three models were tested zero-shot, and their answers were classified by an LLM judge (Gemini) against the verified reference answers.

| Model | History (n=50) | Law (n=50) | Culture (n=50) | Overall accuracy (n=150) |
| --- | --- | --- | --- | --- |
| GPT-4o | 16.0% | 40.0% | 30.0% | **28.7%** |
| SeaLLM-7B-v2.5 | 2.0% | 12.0% | 4.0% | **6.0%** |
| Typhoon v2.5 (30B) | 0.0% | 0.0% | 2.0% | **0.7%** |

Distribution of errors by type (share of each model's incorrect answers):

| Model | Intrinsic hallucination | Extrinsic hallucination | Cultural context confusion |
| --- | --- | --- | --- |
| GPT-4o (107 errors) | 34.6% | 65.4% | 0.0% |
| SeaLLM (141 errors) | 67.4% | 32.6% | 0.0% |
| Typhoon (149 errors) | 59.7% | 40.3% | 0.0% |

## Repository contents

| File | Description |
| --- | --- |
| `thai_history_50.json` | 50 Thai history questions with verified reference answers |
| `thai_law_50.json` | 50 Thai domestic law questions with verified reference answers |
| `thai_cult_50.json` | 50 Thai cultural norms questions with verified reference answers |
| `evaluation_pipeline.ipynb` | Step 1: sends each question to GPT-4o, Typhoon and SeaLLM and saves their answers |
| `gemini_verdict.ipynb` | Step 2: LLM-as-a-judge evaluation with Gemini, then summary of results with pandas |

## Dataset format

Each JSON file is a list of objects with these keys:

| Key | Description |
| --- | --- |
| `id` | Unique question ID, e.g. `TH-HIS-014` (`HIS` = history, `LAW` = law, `CUL` = culture) |
| `domain` | Domain of the question |
| `prompt_th` | The question in Thai (this is what the models received) |
| `prompt_en` | English translation of the question |
| `ground_truth_th` | Verified reference answer in Thai (this is what the judge used) |
| `ground_truth_en` | English translation of the reference answer |

## How the dataset was built

Questions and draft reference answers were first drafted with Claude Sonnet 5. Every question and reference answer was then checked and revised by the author against primary Thai sources, including the Royal Thai Government Gazette (for legal questions), the Royal Institute Dictionary and historical records. The questions target specific, well-documented facts that are unlikely to appear often in English-language web data.

## Evaluation pipeline

**Step 1: answer generation** (`evaluation_pipeline.ipynb`)

| Model | Version | Access |
| --- | --- | --- |
| GPT-4o | `gpt-4o` | OpenAI API |
| Typhoon | `typhoon-v2.5-30b-a3b-instruct` | Typhoon API (`api.opentyphoon.ai`) |
| SeaLLM | `SeaLLMs/SeaLLM-7B-v2.5` | Hugging Face, run in Google Colab with 4-bit quantization, max 512 new tokens |

All models received the same English system prompt and the Thai question only (zero-shot, no context or examples), with temperature 0 / greedy decoding.

**Step 2: LLM-as-a-judge** (`gemini_verdict.ipynb`)

The judge (`gemini-3.6-flash`, temperature 0) receives the question, the verified reference answer and the model's answer. It is told to judge only against the reference answer, not its own knowledge, to reason step by step, and to assign exactly one category:

1. **Factually Accurate**: correctly states the core facts in the reference answer without contradicting it
2. **Intrinsic Hallucination**: directly contradicts facts, dates or clauses in the reference answer
3. **Extrinsic Hallucination**: adds plausible but unverified entities or events not found in the reference answer
4. **Cultural Context Confusion**: wrongly applies Western social, historical or legal principles to a Thai question

The second cell of the notebook counts the categories for each model and domain with pandas, calculates accuracy and exports the table to Excel.

## Reproducing the results

1. Open the notebooks in Google Colab and add these keys under **Secrets**: `OPENAI_API_KEY`, `TYPHOON_API_KEY`, `HF_TOKEN`, `GOOGLE_API_KEY`.
2. Run `evaluation_pipeline.ipynb` once per domain file. SeaLLM needs a GPU runtime.
3. Rename the answer files to the names expected by `gemini_verdict.ipynb` (e.g. `history_gpt4o_answers_COMPLETE.json`, `law_typhoon_answers.json`, `culture_seallm_answers_LOCAL.json`; the full list is in the notebook).
4. Run `gemini_verdict.ipynb`. It saves progress after every item, so it can be re-run safely if interrupted.

Model outputs can change as API models are updated, so exact numbers may differ on a new run.

## Limitations

- **Small dataset**: 150 questions, chosen to allow full manual verification.
- **Single-label rubric**: each answer gets one category. Answers that are both wrong and framed in a Western way are likely to be labeled intrinsic or extrinsic, so the 0% for Cultural Context Confusion should not be read as proof that models never show this error. Judging this category also requires outside knowledge, which conflicts with the instruction to use only the reference answer.
- **Strict extrinsic category**: details not present in the reference answer count as hallucination even if they are true, which may lower scores for models that give long answers.
- **Judge not yet validated against human labels**: agreement between Gemini and a human rater has not been measured.
- **Different run conditions**: SeaLLM was run locally with 4-bit quantization and a 512-token limit, while GPT-4o and Typhoon were called through APIs.

## Citation

> Sinchaikul, Nitchanun (2026). *Thai-HalluEval: A Benchmark for Evaluating LLM Hallucination in Localized Thai Knowledge.* https://github.com/maxxbcc165/Thai-HalluEval-A-Benchmark-for-Evaluating-LLM-Hallucination-in-Localized-Thai-Knowledge

## License and access

No restrictions are placed on the data.

## Author

Nitchanun Sinchaikul, Queen Mary University of London. README first generated on 2026-08-19, updated on 2026-10-19.
