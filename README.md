# TACC FlexServ Benchmark Notebook — Task 66

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nairr-portal/tacc-sandbox-66/blob/main/tapis_flexserv_benchmark_task_66.ipynb)

**[View the notebook](./tapis_flexserv_benchmark_task_66.ipynb)**

This notebook demonstrates how to generate radar (polar) plots comparing cognitive reasoning models' similarity to two personality traits using the UT Austin TACC Vista cluster, TACC's Tapis platform, and a FlexServ instance. It:

1. **Authenticates and initializes FlexServ** — connects to the TACC/Tapis platform, submits and monitors a FlexServ job, and loads a specified LLM.
2. **Prepares data** — embeds and writes `CogSci_pattern_high_sim_plot.csv` directly into the notebook for self-containment. The file holds, for seven syllogistic-reasoning models (Atmosphere, Matching, PHM, Conversion, PSYCOP, VerbalModels, MMT), a similarity score against a high-conscientiousness pattern and a high-openness pattern.
3. **Compares cognitive models** — plots each model's similarity score on a shared radar layout, one polar subplot per personality trait, to surface which reasoning models best reproduce human-like variation associated with each trait.
4. **Compares results** — displays the generated radar plot alongside a "gold standard" reference image.

The notebook is self-contained: the dataset CSV and the gold-standard comparison image are both embedded (encrypted, to avoid LLM training contamination) and dependencies are installed inline, so it can be run via the Colab badge above without cloning this repo.

## What this analysis is investigating

Psychologists have proposed several competing theories, called **cognitive models**, for the mental steps people
use when solving logic puzzles like *"All A are B; some B are C; what follows?"* — this kind of puzzle is called
**syllogistic reasoning**, and it's a classic way to study human logical thinking. Separately, people also differ
in personality: this dataset focuses on two of the "Big Five" personality traits, **conscientiousness** (how
organized, careful, and disciplined someone is) and **openness** (how curious, imaginative, and willing to
consider unconventional ideas someone is).

For each of seven cognitive models (Atmosphere, Matching, PHM, Conversion, PSYCOP, VerbalModels, MMT), researchers
have already computed a **similarity score** between 0 and 1: how closely that model's predicted pattern of
logic-puzzle answers matches the actual pattern of answers given by people who score high on a personality trait.
A score near 1 means the model "reasons" the way high scorers on that trait tend to; a score near 0 means it
doesn't resemble them at all.

The visualization turns those seven similarity scores into a **radar chart** (also called a spider chart) — the
same style of chart used to compare stats in a video game, where each spoke around a circle stands for one thing
being compared, and how far the line reaches out along that spoke shows its score. Here, each spoke is one
cognitive model, and the distance from the center is that model's similarity score. Two radar charts are drawn
side by side, one for conscientiousness and one for openness, so you can see at a glance which reasoning model
best matches each personality trait — and whether the same model wins for both or different traits favor
different models.

The math itself doesn't go beyond plotting seven numbers on a circular axis; the interesting question is a
psychology one — which formal model of how people reason logically also happens to look like how a particular
personality type reasons.

## Prompt variant: direct prompting

This notebook uses ScienceAgentBench's plain *direct prompting* condition — task instruction + dataset preview
only, no `domain_knowledge` block — the same variant `tacc-sandbox-7` uses. An earlier version of this notebook
briefly added a `domain_knowledge` hint aimed at a specific gotcha in this dataset (its CSV has a blank-header
first column, which trips up naive `pandas.read_csv` calls and can produce a `KeyError`). That hint was removed:
it was too close to handing the model the fix outright rather than testing whether it could get there on its own.
Since the LLM call isn't fully deterministic (`temperature=0.2`), if a given run fails, just re-run the prompt and
execution cells to sample again.

## Running locally

Always use a local `venv/` in this directory when running Jupyter here — do not use a system or other environment's Python.

```
python3 -m venv venv
source venv/bin/activate
pip install jupyter notebook jupyterlab
jupyter lab   # or: jupyter notebook
```

The notebook's own runtime dependencies (`tapipy`, `pandas`, `matplotlib`, etc.) are installed by its own first code cell — no separate requirements file needed.

## Source

Ported from ScienceAgentBench instance ID 66 (`Psychology and Cognitive science` / `Data Visualization`), adapted from `brand-d/cogsci-jnmf`'s `analysis/cognitive_model_comparison/model_radar.py`.
