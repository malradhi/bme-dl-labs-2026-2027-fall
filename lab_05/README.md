# Lab 05: Computer vision and semantic segmentation

**BME MSc Deep Learning / VITMMA19 · Fall semester 2026/2027**  
**Instructor:** Dr. Mohammed Salah Al-Radhi

[Back to the course overview](../README.md)

Lab 5 connects the computer vision lecture with a complete semantic segmentation workflow in PyTorch: instead of one label per image, the network labels every pixel. We first work through per-pixel losses and metrics, the step from sliding windows to fully convolutional networks, down- and upsampling, BatchNorm, residual paths, augmentation for segmentation, and a self-supervised pretext task together. You then investigate a practical question: a digit segmenter trained under good lighting must keep working when forms are lit from one side and covered in pencil marks.

The training engine is supplied, and each run takes seconds on a CPU. The main work is predicting results before running, planning code before writing it, debugging, and explaining what you observe with validation evidence. The assignment builds on the metrics and controlled experiments of Lab 4 and is more demanding: it includes two debugging tasks and an audit of AI-written code.

## Materials

| Material | Notebook | Run in Google Colab |
| --- | --- | --- |
| Follow with the instructor | [Guided notebook](01_BME_DL_Lab5_Guided.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_05/01_BME_DL_Lab5_Guided.ipynb) |
| Complete independently and submit | [Assignment notebook](02_BME_DL_Lab5_Assignment.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_05/02_BME_DL_Lab5_Assignment.ipynb) |

## Learning objectives

- Produce and train per-pixel predictions: `N × C × H × W` scores with a pixel-wise cross-entropy loss.
- Compute pixel accuracy, IoU, mIoU and Dice, and explain what each rewards and hides.
- Explain why a sliding-window classifier is inefficient, and convert it into an equivalent fully convolutional network.
- Use downsampling, upsampling and skip connections, and separate *where* an object is from *what* it is.
- Explain BatchNorm's training and evaluation modes, and diagnose an evaluation that depends on batch size.
- Implement a residual block and relate residual paths to gradient flow in deep networks.
- Design augmentation that keeps images and label maps consistent, and audit augmentation code before trusting it.
- Explain the idea of a self-supervised pretext task.
- Test a claim experimentally, including suggestions from AI tools, and freeze a decision before a single test evaluation.

## Session structure

**30 minutes guided work + 60 minutes independent work, including submission.**

| Assignment task | Minutes | Points |
| --- | ---: | ---: |
| T1 · Pixels, classes and the metric trap | 8 | 3 |
| T2 · From sliding window to fully convolutional | 10 | 4 |
| T3 · Where versus what: encoder–decoder and skip connections | 14 | 5 |
| T4 · BatchNorm and residual connections | 12 | 4 |
| T5 · Augmentation you can trust, then test once | 12 | 4 |
| Check and submit | 4 | — |
| **Assignment total** | **60** | **20** |

Each task has parts a, b and c. Optional deeper investigations follow T5 for students who want to go further. They are not part of the 20 points.

## Before you start

1. Open the notebook in Colab and select **File → Save a copy in Drive**.
2. Use a Python 3 runtime. **A CPU runtime is sufficient; a GPU is optional.** No paid compute, account, or API key is required.
3. Run the setup cells first. No download is needed: the data ship with scikit-learn. Keep Colab's existing PyTorch, NumPy, and scikit-learn packages.
4. Work in order. Supplied helper cells can be expanded for inspection; the assignment contains deliberate TODOs that stop the notebook until you complete them.

The data are built from the **1,797 scikit-learn 8×8 digits**, using the same fixed stratified split IDs as Labs 3–4. Each **24×24 scene** contains two digits, 11–14 pixels tall. They usually touch, and in about two thirds of the scenes one digit partly covers the other. Every pixel is labelled **background** or **digit 0–9**, so there are 11 classes. Training, validation and test scenes are built from separate digit images: **1,200 training, 400 validation and 400 test scenes**. This is not MNIST.

- **Training** images use *standard* conditions: mild uneven lighting, a few pencil strokes and slight noise.
- **Validation** contains two views of the same 400 scenes: *standard*, and *harsh* (strong lighting from one side and more pencil strokes). The labels are identical, so the two views form a paired comparison.
- **Test** scenes have the same two views and are built only when you open them, once, at the end of the assignment. The guided notebook never evaluates test data.

Normalisation statistics are calculated from the standard training images only. The scenes, lighting and clutter are an artificial classroom problem, not a benchmark result.

## Experiment rules

Every run uses the same fixed settings: **12 epochs, batch size 32, Adam with a one-cycle learning-rate schedule (peak 1e-2), seed 42, and pixel-wise cross-entropy**. The engine keeps the final-epoch model, so differences come from the model or the data, not from when training stopped.

Before each task's training runs, record numerical **predictions** with `predict(...)`; the engine refuses to train until you have. Some code TODOs start with a short written **plan** (shapes, a formula, or which check catches what). Predictions and plans are marked for being specific and reasoned, not for being right.

You change only what each task allows: the decoder of a small U-Net in T3, three 16-layer networks in T4, and at most two augmentation runs in T5b. Every run is recorded in the experiment log. A name cannot be reused for different settings, so unsuccessful runs remain visible as evidence.

**Select by mean IoU (mIoU) on the harsh validation view**, which resembles deployment. Foreground IoU, digit accuracy and pixel accuracy help you explain *why* a model wins. Marks reward valid experiments, evidence, and interpretation, not a particular score. An unsuccessful experiment that is well explained can receive full credit.

The final test evaluates the run selected by the stated validation rule. Write your decision first, then open the test set once. Do not change the model after seeing test results.

## AI assistance

You may use AI tools such as ChatGPT or Claude. Declare what you used and how you checked it at the top of the notebook. In T5a, everyone audits an augmentation function written by an AI assistant before trusting it. In T5b, AI users test one suggestion from their assistant experimentally; students who did not use AI test a supplied claim instead. Both paths are assessed on the quality of the test and the conclusion, not on whether the claim turns out to be true.

## Submission

Submit **one executed notebook** named **`YOUR_NEPTUN_DL_Lab05.ipynb`** to Moodle **before the lab ends**. Include your name, Neptun code, assistance declaration, completed code, written plans and predictions, all required tables and pictures, and your final decision. Keep outputs when saving. A Colab sharing link alone is insufficient.

If you restart and rerun, reproduce the same frozen plan and the same predictions; do not change them in response to results. Submit partial work on time if necessary and explain what remains incomplete. If you do not yet have Moodle access, keep a downloaded copy, inform the instructor before the deadline, and email the work as stated in the main course README.

Work on your own submission, follow the course rules for collaboration and AI assistance, and be ready to explain your results. Keep completed answers and personal student details out of the public repository. Dataset and framework credits are provided in both notebooks.
