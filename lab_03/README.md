# Lab 03: Hyperparameter optimisation under a fixed budget

**BME MSc Deep Learning / VITMMA19 · Fall semester 2026/2027**  
**Instructor:** Dr. Mohammed Salah Al-Radhi

[Back to the course overview](../README.md)

Lab 3 connects the hyperparameter-optimisation lecture with a complete experimental workflow in PyTorch. We first explore a small handwritten-digit classifier together. You then tune a classifier on a fixed noisy version of the same data, justify two follow-up experiments, and confirm two finalists across several seeds.

The training engine is supplied so that the main work is designing and interpreting experiments. The assignment builds on the PyTorch training loop and validation procedure from Lab 2.

## Materials

| Material | Notebook | Run in Google Colab |
| --- | --- | --- |
| Follow with the instructor | [Guided notebook](01_BME_DL_Lab3_Guided.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_03/01_BME_DL_Lab3_Guided.ipynb) |
| Complete independently and submit | [Assignment notebook](02_BME_DL_Lab3_Assignment.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_03/02_BME_DL_Lab3_Assignment.ipynb) |

## Learning objectives

- Diagnose weak optimisation and possible overfitting from learning curves.
- Compare learning rates while holding other settings fixed.
- Design logarithmic and categorical hyperparameter search spaces.
- Use Optuna TPE, intermediate reporting, and pruning.
- Distinguish early stopping within one run from pruning across trials.
- Restore the checkpoint associated with the reported validation score.
- Compare two configurations using three prespecified training seeds.
- Freeze the choice before a single final test evaluation.
- Keep an experiment log, including unsuccessful attempts, and explain limitations.

## Session structure

**30 minutes guided work + 60 minutes independent work, including submission.**

| Assignment task | Minutes | Points |
| --- | ---: | ---: |
| T1 · Diagnose the supplied baseline | 7 | 2 |
| T2 · Three controlled learning-rate experiments | 10 | 4 |
| T3 · Eight automated trials and two informed refinements | 18 | 6 |
| T4 · Two finalists, three new seeds each | 12 | 5 |
| T5 · Freeze, test once, and interpret | 8 | 3 |
| Check and submit | 5 | — |
| **Assignment total** | **60** | **20** |

## Before you start

1. Open the notebook in Colab and select **File → Save a copy in Drive**.
2. Use a **Python 3 CPU runtime**. No GPU, paid compute, W&B account, or API key is required.
3. Run setup first. It installs the tested Optuna version, 4.7.0, if necessary. Keep Colab's existing PyTorch and NumPy packages.
4. Work in order. Supplied helper cells can be expanded for inspection; the assignment contains deliberate TODOs.

The scikit-learn digits dataset contains **1,797 images of size 8×8** and is packaged with scikit-learn. It is not MNIST. Both notebooks use the same fixed stratified IDs: **1,077 training, 360 validation, and 360 held-out test examples**. The guided notebook never evaluates test data.

The assignment adds fixed Gaussian pixel noise (standard deviation 0.35 after scaling pixels to 0–1), then clips to 0–1. The noise is generated independently for each split and does not change between experiments. This is an artificial classroom problem, not a full handwriting-recognition benchmark.

## Experiment rules

The assignment permits **20 fits maximum**: one baseline, three manual runs, eight Optuna trials, two refinements, and six confirmation runs. Each fit has at most 25 epochs, with batch size 64 and a fixed early-stopping rule. Pruned and failed trials still consume their allocated slots.

Tune only learning rate, hidden width, dropout, and weight decay as instructed. Keep the data, noise, architecture family, optimizer, epoch limit, and seed lists fixed. The largest allowed model has 9,610 trainable parameters.

Select by **validation cross-entropy**, with accuracy as a secondary metric. Aim for at least **85% mean validation accuracy across the three confirmation seeds**, but there is no accuracy-based pass threshold. Marks reward valid experiments, evidence, and interpretation. An unsuccessful but justified refinement can receive full credit.

The final test uses the seed-7 checkpoint of the configuration selected by mean validation loss. Do not choose the best seed or tune after viewing test results. Three training seeds on one split are not cross-validation.

The required log is displayed in the notebook and also saved locally as `lab3_experiments.csv`. W&B appears only as an optional after-class example in the guided notebook.

## Submission

Submit **one executed notebook** named **`YOUR_NEPTUN_DL_Lab03.ipynb`** to Moodle **before the lab ends**. Include your name, Neptun code, assistance used, completed code, written predictions, all required tables/plots, and the final decision. Keep outputs when saving. A Colab sharing link alone is insufficient; the CSV is an optional backup.

If you restart and rerun, reproduce the same frozen plan; do not change it in response to test results. Submit partial work on time if necessary and explain the blocker. If you do not yet have Moodle access, keep a downloaded copy, inform the instructor before the deadline, and email the work as stated in the main course README.

Work on your own submission, follow the course rules for collaboration and AI assistance, and be ready to explain your results. Keep completed answers and personal student details out of the public repository. Dataset and framework credits are provided in both notebooks.
