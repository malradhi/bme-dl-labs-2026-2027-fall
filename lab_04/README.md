# Lab 04: Classification metrics and CNN design

**BME MSc Deep Learning / VITMMA19 · Fall semester 2026/2027**  
**Instructor:** Dr. Mohammed Salah Al-Radhi

[Back to the course overview](../README.md)

Lab 4 connects the lecture on classification and convolutional neural networks with a set of short, controlled experiments in PyTorch. We first work through cross-entropy, convolution arithmetic, equivariance, and classification metrics together. You then investigate a practical question: a digit reader trained only on centred digits must work wherever the digit appears in the image.

The training engine is supplied, and each run takes seconds on a CPU. The main work is predicting results before running, explaining what you observe, and justifying a final model with validation evidence. The assignment builds on the training loop from Lab 2 and the controlled-experiment practice from Lab 3.

## Materials

| Material | Notebook | Run in Google Colab |
| --- | --- | --- |
| Follow with the instructor | [Guided notebook](01_BME_DL_Lab4_Guided.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_04/01_BME_DL_Lab4_Guided.ipynb) |
| Complete independently and submit | [Assignment notebook](02_BME_DL_Lab4_Assignment.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_04/02_BME_DL_Lab4_Assignment.ipynb) |

## Learning objectives

- Explain why cross-entropy carries information that accuracy hides, and why PyTorch losses take logits.
- Debug a classification loss using a training curve and a short derivation.
- Predict output sizes, parameter counts, and receptive fields from kernel size, stride, padding, and dilation.
- Distinguish equivariance (what convolution provides) from invariance (what a classifier needs).
- Compare architectures and training-time augmentation in controlled, one-variable experiments.
- Compute precision, recall, specificity, F1, and false-positive rate from a confusion matrix.
- Use ROC/AUC and choose a decision threshold on validation data under a recall requirement.
- Test a claim experimentally, including suggestions from AI tools, and explain unsuccessful results.
- Freeze a decision before a single final test evaluation.

## Session structure

**30 minutes guided work + 60 minutes independent work, including submission.**

| Assignment task | Minutes | Points |
| --- | ---: | ---: |
| T1 · Debug a classification loss | 8 | 3 |
| T2 · Predict shapes, parameters and receptive fields | 10 | 4 |
| T3 · Does a CNN handle digits it has only seen centred? | 17 | 6 |
| T4 · Evaluate beyond accuracy | 12 | 4 |
| T5 · Test a claim, freeze the decision, test once | 8 | 3 |
| Check and submit | 5 | — |
| **Assignment total** | **60** | **20** |

Optional deeper investigations follow T5 for students who finish early. They are not part of the 20 points.

## Before you start

1. Open the notebook in Colab and select **File → Save a copy in Drive**.
2. Use a Python 3 runtime. **A CPU runtime is sufficient; a GPU is optional.** No paid compute, account, or API key is required.
3. Run the setup cells first. No download is needed: the data ship with scikit-learn. Keep Colab's existing PyTorch, NumPy, and scikit-learn packages.
4. Work in order. Supplied helper cells can be expanded for inspection; the assignment contains deliberate TODOs that stop the notebook until you complete them.

The data are the **1,797 scikit-learn 8×8 digits** with the same fixed stratified split IDs as Lab 3: **1,077 training, 360 validation, and 360 held-out test examples**. Each digit is enlarged to 16×16 and placed on an empty 32×32 field. This is not MNIST.

- **Training** digits are always **centred**, unless you switch on training-time augmentation.
- **Validation** contains two views of the same 360 images: *centred*, and *shifted* to one fixed random position per image. The two views form a paired comparison.
- **Test** data have the same two views and are opened once, at the end of the assignment. The guided notebook never evaluates test data.

Normalisation statistics are calculated from the centred training images only. This is an artificial classroom problem, not a benchmark result.

## Experiment rules

Every run uses the same fixed settings: **15 epochs, batch size 64, Adam with learning rate 3e-3, and seed 42**. The engine keeps the final-epoch model, so differences come from the model or the data, not from when training stopped.

You change only what each task allows: the model head in T3, training-time augmentation settings in T3c, and at most two claim-test runs in T5a. Every run is recorded in the experiment log. A name cannot be reused for different settings, so unsuccessful runs remain visible as evidence.

**Select by cross-entropy on the shifted validation view**, which resembles deployment; accuracy is a secondary metric. Write each prediction *before* running the corresponding cell. Marks reward valid experiments, evidence, and interpretation, not a particular accuracy. An unsuccessful experiment that is well explained can receive full credit.

The final test evaluates the configuration selected by the stated validation rule, unless you override it with a written, validation-based reason. Write your decision first, then open the test set once. Do not change the model or the decision threshold after seeing test results.

## AI assistance

You may use AI tools such as ChatGPT or Claude. Declare what you used and how you checked it at the top of the notebook. In T5a, AI users test one suggestion from their assistant experimentally; students who did not use AI test a supplied claim instead. Both paths are assessed on the quality of the test and the conclusion, not on whether the claim turns out to be true.

## Submission

Submit **one executed notebook** named **`YOUR_NEPTUN_DL_Lab04.ipynb`** to Moodle **before the lab ends**. Include your name, Neptun code, assistance declaration, completed code, written predictions, all required tables and plots, and your final decision. Keep outputs when saving. A Colab sharing link alone is insufficient.

If you restart and rerun, reproduce the same frozen plan; do not change it in response to test results. Submit partial work on time if necessary and explain what remains incomplete. If you do not yet have Moodle access, keep a downloaded copy, inform the instructor before the deadline, and email the work as stated in the main course README.

Work on your own submission, follow the course rules for collaboration and AI assistance, and be ready to explain your results. Keep completed answers and personal student details out of the public repository. Dataset and framework credits are provided in both notebooks.
