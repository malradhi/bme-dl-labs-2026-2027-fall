# Lab 02 — PyTorch and Convolutional Neural Networks

**Course:** Deep Learning / VITMMA19  
**Budapest University of Technology and Economics (BME)**  
**Department of Telecommunications and Artificial Intelligence**  
**Academic year:** 2026/2027 — Fall semester  
**Instructor:** Dr. Mohammed Salah Al-Radhi,

Lab 02 introduces the standard **PyTorch deep-learning workflow** and applies it to image classification with convolutional neural networks (CNNs).

The lab has two parts:

1. **Guided notebook** — concepts and code are explained step by step together in class.
2. **Assignment notebook** — students work independently and submit the completed notebook to Moodle before the end of the lab.

---

## Learning objectives

By the end of this lab, students should be able to:

- create and manipulate PyTorch tensors,
- move tensors and models between CPU and GPU,
- explain the role of automatic differentiation,
- use `Dataset` and `DataLoader` for mini-batch training,
- explain the basic role of convolution, ReLU and pooling layers,
- build a small CNN using `torch.nn`,
- implement the standard PyTorch training and evaluation loops,
- use training/validation curves to inspect model behaviour,
- evaluate a classifier with a confusion matrix and error examples,
- perform one controlled model-improvement experiment.

---

## Lab files

| File | Purpose | Open in Colab |
|---|---|---|
| `01_BME_DL_Lab2_Guided.ipynb` | Guided PyTorch + CNN walkthrough using Fashion-MNIST | [Open in Colab](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_02/01_BME_DL_Lab2_Guided.ipynb) |
| `02_BME_DL_Lab2_Assignment.ipynb` | Independent CIFAR-10 assignment | [Open in Colab](https://colab.research.google.com/github/malradhi/bme-dl-labs-2026-2027-fall/blob/main/lab_02/02_BME_DL_Lab2_Assignment.ipynb) |

---

## Suggested lab structure

### Part 1 — Guided work

The guided notebook covers:

1. PyTorch tensors and device handling,
2. automatic differentiation (`autograd`),
3. `Dataset` and `DataLoader`,
4. image batch shapes (`N × C × H × W`),
5. CNN building blocks,
6. `CrossEntropyLoss` and Adam,
7. a full PyTorch training loop,
8. validation, learning curves and confusion matrices.

The guided example uses **Fashion-MNIST** so that the class can focus on the workflow rather than long training times.

### Part 2 — Independent assignment

The assignment transfers the same ideas to **CIFAR-10**, which is more difficult than Fashion-MNIST and therefore requires more independent reasoning.

The assignment contains **5 tasks / 20 points**:

| Task | Topic | Points |
|---|---|---:|
| T1 | Inspect, visualize and explain CIFAR-10 data | 3 |
| T2 | Complete a CNN architecture | 5 |
| T3 | Complete the PyTorch training/evaluation loop | 5 |
| T4 | Run one controlled improvement experiment | 4 |
| T5 | Confusion matrix and error analysis | 3 |
| **Total** |  | **20** |

---

## Google Colab setup

1. Open the required notebook using the **Open in Colab** link above.
2. In Colab, save your own copy if needed: **File → Save a copy in Drive**.
3. Use a standard Python 3 runtime.
4. A CPU is sufficient for most of the guided notebook.
5. For the assignment, a **T4 GPU is recommended**:  
   **Runtime → Change runtime type → T4 GPU**.
6. Run the setup cells first and then follow the notebook in order.

No local installation is required when using Google Colab.

---

## Assignment submission

Students must:

1. complete **T1–T5**,
2. include their **name and Neptun code**,
3. keep code outputs, plots and written answers visible,
4. restart the runtime/session and run the completed notebook from top to bottom before submission,
5. rename the notebook to:

```text
YOUR_NEPTUN_DL_Lab02.ipynb
```

6. download the executed `.ipynb` file,
7. submit that file to **Moodle before the end of the lab**.

Submitting only a Colab or Google Drive link is not sufficient.

If a student cannot finish every task during the lab, they should still submit their completed work on time unless the instructor gives different instructions.

---

## Main technical ideas

The central workflow of Lab 02 is:

```text
Dataset
   ↓
DataLoader / mini-batches
   ↓
CNN
   ↓
logits
   ↓
CrossEntropyLoss
   ↓
loss.backward()
   ↓
optimizer.step()
   ↓
validation and error analysis
```

A key goal is that students understand this workflow directly in PyTorch before using higher-level training frameworks.

---

## Repository

Course repository: https://github.com/malradhi/bme-dl-labs-2026-2027-fall
