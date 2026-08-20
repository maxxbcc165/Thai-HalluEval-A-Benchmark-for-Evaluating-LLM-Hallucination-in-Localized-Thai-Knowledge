This readme file was generated on 2026-08-19 by Nitchanun Sinchaikul

# Thai-HalluEval Benchmark Dataset

## 1. General Information
- **Title of Dataset:** Thai-HalluEval: A Benchmark for Evaluating LLM Hallucination in Localized Thai Knowledge
- **Author:** Nitchanun Sinchaikul
- **Institution:** Queen Mary University of London
- **Date of Data Collection:** August 2026
- **Context:** This dataset was created for the MSc Linguistics and AI dissertation titled *"Global Models, Local Blind Spots: Evaluating LLM Hallucination in Thai Knowledge"*.

## 2. SHARING/ACCESS INFORMATION
- **Licenses/restrictions placed on the data: No
- **Links to publications that cite or use the data: None at this time
- **Links to other publicly accessible locations of the data: https://github.com/maxxbcc165/Thai-HalluEval-A-Benchmark-for-Evaluating-LLM-Hallucination-in-Localized-Thai-Knowledge
- **Recommended citation for this dataset: Nitchanun Sinchaikul (2026). hai-HalluEval: A Benchmark for Evaluating LLM Hallucination in Localized Thai Knowledge. https://github.com/maxxbcc165/Thai-HalluEval-A-Benchmark-for-Evaluating-LLM-Hallucination-in-Localized-Thai-Knowledge.

## 3. Data and File Overview
- **File List:** 
  - `thai_cult_50.json` (The primary dataset of Thai culture domain containing 50 verified prompts and ground truths)
  - `thai_history_50.json` (The primary dataset of Thai history domain containing 50 verified prompts and ground truths)
  - `thai_law_50.json` (The primary dataset of Thai law domain containing 50 verified prompts and ground truths)
  - `evaluation_pipeline.ipynb` (Google Colab notebook used for LLM-as-a-Judge evaluation)

## 4. Methodological Information
- **Description of Methods:** The dataset was generated using Claude 5 Sonnet for initial drafting and subsequently underwent strict human expert verification against primary Thai sources (e.g., Royal Thai Government Gazette, historical records).
- **Domains Covered:** Thai History (50), Thai Domestic Law (50), and Thai Cultural Norms (50).

## 5. Data-Specific Information
- **Variables / JSON Keys:**
  - `prompt_id`: Unique identifier for the query.
  - `domain`: The localized domain of the query.
  - `prompt_th`: The exact query in the Thai language.
  - `prompt_en`: English translation of the prompt.
  - `ground_truth_th`: The expert-verified factual standard used for LLM evaluation in Thai.
  - `ground_truth_en`: The expert-verified factual standard used for LLM evaluation in English.
