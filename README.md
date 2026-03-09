# semantic-drift-guard
Token-level monitoring framework for detecting hallucination risk in large language models.
# Semantic Drift Guard
Token-Level Monitoring for LLM Hallucination Risk

## Overview

Semantic Drift Guard is an experimental monitoring framework for detecting and mitigating hallucination-prone behavior in large language models (LLMs).

Instead of constantly constraining the model, the system analyzes generation in real time and applies stabilization only when risk signals rise.

The goal is to preserve normal generative freedom while reducing hallucination-prone outputs.

---

## Why This Project Exists

Large language models occasionally produce confident but incorrect information, commonly referred to as **hallucinations**.

This project explores whether hallucination-prone states can be detected during token generation by monitoring signals such as:

- token uncertainty
- semantic drift from the prompt
- divergence between competing continuations

When these signals exceed a threshold, the system applies stabilization anchors to guide generation back toward the intended meaning.

---

## Core Idea

Hallucinations often occur when models enter regions of **semantic uncertainty or ambiguity**.

Prompt → Generation → Risk Monitoring → Stabilization (if needed) → Continue Generation


The system allows free generation under low risk and selectively intervenes only when hallucination probability increases.

---

## System Architecture

The Semantic Drift Guard system monitors generation during inference and applies stabilization only when hallucination risk rises.

Prompt
  ↓
LLM Generation
  ↓
Risk Monitor
  ↓
Risk Score Calculation
  ↓
Decision Controller
  ↓
Anchor Trigger (if needed)
  ↓
Continue Generation

## Risk Signals

The system monitors three signals during generation.

### 1. Token Uncertainty

Measures how confident the model is about the next token.

Flat probability distributions indicate lower confidence and higher hallucination risk.

---

### 2. Semantic Drift

Measures how far the generated response has moved away from the original prompt meaning.

This is estimated using embedding similarity between the prompt and the generated response.

---

### 3. Branch Divergence

Examines competing next-token candidates to estimate whether they imply different semantic paths.

Large divergence between branches can indicate ambiguous interpretation.

---

## Risk Score

The system combines the three signals into a single risk score:

R = w₁·U + w₂·D + w₃·B


Where:

- **U** = token uncertainty  
- **D** = semantic drift  
- **B** = branch divergence  

When the risk score crosses a threshold, the system activates stabilization anchors.

---

## Stabilization Anchors

Anchors are lightweight interventions designed to prevent hallucination escalation.

Two types are used:

### Soft Anchor

Used when risk begins to rise.

Example behavior:

- reinforce prompt meaning
- discourage unsupported assumptions

---

### Strong Anchor

Used when risk becomes high.

Example behavior:

- request clarification
- restrict speculative output
- re-ground the response to the prompt context

---

## Stress Test Suite

The project includes an adversarial prompt suite designed to challenge models under common hallucination scenarios.

Test categories include:

- ambiguous entities  
- fabricated facts  
- long-context drift  
- speculation pressure  
- entity blending  

This allows guarded generation to be compared against baseline generation.

---

## Evaluation Framework

The project includes tools to measure:

- baseline hallucination rate
- guarded hallucination rate
- reliability improvement
- anchor activation frequency
- latency overhead

Results are stored as CSV files for analysis.

---

## Example Benchmark

Example experimental result:

Baseline hallucination rate: **50%**  
Guarded hallucination rate: **20%**

Hallucination reduction: **60%**

These results demonstrate that token-level monitoring can reduce hallucination-prone outputs while preserving useful responses.

---

## Visualization Tools

The project includes visualization utilities that show:

- uncertainty over time
- semantic drift curves
- risk score evolution
- anchor trigger points

These tools help analyze when and why hallucinations occur.

---

## Project Structure

semantic-drift-guard/

src/
drift_guard.py
scoring.py
anchor_engine.py
evaluation.py
analysis.py
visualize.py

prompts/
stress_test_prompts.json

results/
evaluation.csv
stress_test_results.csv

docs/
architecture.md
benchmark_notes.md


---

## Installation

pip install -r requirements.txt


---

## Running the Project

Example:

python src/evaluation.py


To analyze results:

python src/analysis.py


---

## Example Behavior

Example prompt:

Tell me about Mercury.

Baseline model response may mix multiple meanings (planet, element, mythology).

Semantic Drift Guard monitors token uncertainty and semantic drift during generation.  
If the system detects rising ambiguity between meanings, a stabilization anchor may trigger to guide the response toward the intended context.

This allows the model to maintain useful output while reducing hallucination-prone behavior.

## Future Work

Possible improvements include:

- retrieval-based grounding
- adaptive risk thresholds
- improved branch divergence estimation
- automated hallucination labeling
- larger adversarial prompt datasets

---

## Author

Steven Duffey

Independent AI reliability research project exploring hallucination detection and mitigation strategies in large language models.

---
