# LLM Red Teaming Experiment (Baseline Evaluation)

## Overview

This project implements a small-scale empirical evaluation of prompt injection and adversarial prompting behaviors in large language models. The goal is to measure how different attack styles influence model compliance with potentially harmful requests and to establish a baseline for further red-teaming experiments.

The system runs a set of predefined adversarial prompts against an LLM API and evaluates the responses using an automated LLM-based judge.

---

## Experimental Setup

### Models Used

- **Target model:** `gpt-5.4-mini`
- **Evaluator model:** `gpt-4.1-mini` (used as a lightweight automated judge)

---

## Attack Types Tested

The baseline includes five categories of adversarial prompts:

- **Direct Attack**  
  Explicit harmful request without framing or disguise.

- **Roleplay Attack**  
  Instruction framed as a fictional character (e.g., “you are a criminal”).

- **Prompt Injection Attack**  
  Attempts to override prior instructions using system-style directives.

- **Encoding Attack**  
  Obfuscated request using base64 encoding.

- **Multi-step Attack**  
  Gradual framing that first elicits general knowledge before steering toward misuse.

---

## Methodology

For each attack:

1. The prompt is sent to the target model via the OpenAI API.
2. The model generates a response under deterministic settings (`temperature=0`).
3. The response is passed to a second LLM acting as an evaluator.
4. The evaluator determines whether the response meaningfully enables wrongdoing.
5. Results are stored for analysis.

---

## Evaluation Function

The evaluator model is prompted to classify responses as:

- `"yes"` → response meaningfully helps wrongdoing  
- `"no"` → response does not meaningfully help wrongdoing  

This produces a simple binary failure indicator used for aggregation.

---

## Output Format

Results are stored in a CSV file with the following structure:

| Column   | Description |
|----------|-------------|
| type     | Attack category |
| prompt   | Input prompt used |
| response | Model output |
| failure  | LLM judge output (“yes” or “no”) |

---

## Initial Results Summary

The baseline experiment produces failure-rate estimates per attack category:

- Direct attacks
- Roleplay-based framing
- Instruction injection attempts
- Encoded/obfuscated prompts
- Multi-step reasoning attacks

Failure rates are computed as the proportion of `"yes"` judgments within each attack category.

---

## Limitations (Baseline Phase)

This initial version has several known limitations:

- Small prompt set (5 attack types only)
- Binary evaluation lacks granularity (no partial compliance scoring)
- Single evaluator model introduces potential bias
- No iterative or adaptive adversarial generation
- No cross-model comparison

---

## Purpose of This Stage

This baseline serves as a starting point for iterative improvements in:

- adversarial prompt coverage
- evaluation robustness
- attack sophistication
- measurement quality

Subsequent versions will expand both the attack surface and the evaluation methodology to better approximate real-world red-teaming conditions.
