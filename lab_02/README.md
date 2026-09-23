# Lab 02: Backpropagation in PyTorch and image classification

**BME MSc Deep Learning / VITMMA19 · Fall semester 2026/2027**  
**Instructor:** Dr. Mohammed Salah Al-Radhi

[Back to the course overview](../README.md)

This lab connects the backpropagation and hardware lectures with a complete PyTorch experiment. We first work through Fashion-MNIST together, then apply the same workflow independently to CIFAR-10.

## Learning objectives

- Connect the chain rule to `loss.backward()` and a parameter update.
- Read tensor shapes, dtypes, and devices; explain the role of mini-batches.
- Calculate normalisation statistics from training data only.
- Build and train a small CNN with raw logits and cross-entropy loss.
- Compare two learning rates under the same initialisation and training budget.
- Select a checkpoint using validation, then evaluate it on held-out test data.
- Distinguish weight storage from training-memory requirements.

## Notebooks

| Material | Notebook | Run in Google Colab |
| --- | --- | --- |
| Follow with the instructor | [Guided notebook](01_BME_DL_Lab2_Guided.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_02/01_BME_DL_Lab2_Guided.ipynb) |
| Complete independently and submit | [Assignment notebook](02_BME_DL_Lab2_Assignment.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_02/02_BME_DL_Lab2_Assignment.ipynb) |

## How the lab works

Suggested 90-minute session: **5 minutes setup, 35 minutes guided work, 45 minutes independent work, and 5 minutes submission**. The instructor may adjust the pacing.
The guided notebook's final testing and CPU/GPU benchmark sections are optional demonstrations.

| Assignment task | Topic | Points |
| --- | --- | ---: |
| T1 | Inspect and visualise the data | 3 |
| T2 | Complete the CNN; explain memory use | 5 |
| T3 | Training/evaluation loop and backpropagation | 5 |
| T4 | One controlled learning-rate experiment | 4 |
| T5 | Final test evaluation and error analysis | 3 |
| **Total** | | **20** |

## Getting started

1. Open the notebook in Colab and select **File → Save a copy in Drive**.
2. Use a Python 3 runtime. **CPU is supported; GPU is optional.** Select your device before setup.
3. Run setup/data cells first. The initial Fashion-MNIST or CIFAR-10 download requires internet access and downloads the full dataset, even though training uses a subset.
4. Work through the cells in order. The assignment has deliberate TODOs; supplied helpers are labelled.

The default guided run uses 4,000 training and 1,000 validation images. The assignment uses 6,000 training, 1,000 validation, and 1,000 held-out test images. Each experiment runs for two epochs. Keep the same configuration for both compared models.
These are classroom experiments, not full-dataset benchmark results. Marks depend on correctness and reasoning, not an exact accuracy.

## Submission

Complete T1–T5, include your name and Neptun code, and keep the required outputs, plots, and written answers visible. Restart and run the completed notebook from the beginning before downloading it.

Rename the file **`YOUR_NEPTUN_DL_Lab02.ipynb`**, download it through **File → Download → Download .ipynb**, and submit the executed file to **Moodle before the lab ends**. A Colab/Drive link alone is not sufficient.

Submit partial work on time if necessary and explain what remains incomplete. If Moodle access is unavailable, keep a downloaded copy, inform the instructor, and email the work to the instructor, following the course README.

## Academic practice and credits

Work on your own submission and be prepared to explain it. Follow the course rules for collaboration and AI tools and record any assistance used. Keep completed answers and personal student details out of the public repository.
Dataset credits, framework references, and reuse notes appear inside the notebooks.
