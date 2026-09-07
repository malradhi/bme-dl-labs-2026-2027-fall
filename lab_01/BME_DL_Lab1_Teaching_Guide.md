# BME Deep Learning — Lab 1 teaching guide

**For Dr. Mohammed Salah Al-Radhi · Wednesday, 9 September 2026**

This pack assumes a **90-minute lab** and individual submissions. It keeps TensorFlow from the existing materials. The main change is the teaching structure: explain a short sequence of connected ideas, let students apply them, and finish with a notebook they can submit and explain.

The exercises use small synthetic data and a one-parameter linear model. This is deliberately a first lab on the mechanics of learning, not a demonstration of a deep network or a claim of real-world accuracy.

## Files and who receives them

| File | Purpose | Distribute to students? |
|---|---|---|
| `01_BME_DL_Lab1_Guided.ipynb` | Guided explanation, worked examples, and optional reference sections | Yes |
| `02_BME_DL_Lab1_Assignment.ipynb` | T1–T5, incomplete code, feedback checks, and submission instructions | Yes |
| `03_BME_DL_Lab1_Instructor_Solutions.ipynb` | Completed code, observed outputs, model answers, and marking notes | **No** |
| `BME_DL_Lab1_Teaching_Guide.md` | Pacing, review findings, Moodle text, and preparation notes | Instructor only |

Upload the first two notebooks as Moodle resources. Keep the solution notebook in an instructor-only location.
Each notebook opens through **Upload notebook** in [Google Colab](https://colab.research.google.com/).
The files have Python 3 notebook metadata and Colab section navigation. Supplied check cells use Colab form view to keep their implementation compact; students can reveal the code if needed.

## Suggested 90-minute delivery

| Time | Activity | What students should understand |
|---|---|---|
| 0–5 min | Open Colab, save a copy, run setup | A notebook needs a runtime and an execution order |
| 5–20 min | Guided sections 1–5 | Samples/features, dtype, indexing, axis, broadcasting, and `*` versus `@` |
| 20–30 min | Guided sections 6–7 | Tensors, a trainable variable, a loss, a gradient, and one update |
| 30–40 min | Guided section 8 and TensorBoard | A fair comparison uses the same data and starting conditions; logs record actual measurements |
| 40–85 min | Individual assignment T1–T5 | Apply the ideas and explain the result |
| 85–90 min | Download and submit | One executed `.ipynb`, with answers, outputs, and a labelled plot |

Ask for predictions before running three selected cells: list multiplication, the per-feature mean, and the sign of the gradient update. Do not pause at every line of the checking code.

For **120 minutes**, allow 5 minutes for setup, 50 for explanation, 55 for the assignment, and 10 for submission. Use the extra explanation time for copies/views and a learning rate that diverges. If Python is new to much of the class, use that longer route; the 90-minute plan assumes a basic familiarity with variables, functions, and loops.

The optional reference sections are not required for the marked assignment.

## What changed from the original materials

All cells in the three supplied notebooks were reviewed. The original NumPy notebook has 158 cells, the TensorBoard notebook 20, and the TensorFlow notebook 25.

| Original area | Finding | Revision |
|---|---|---|
| NumPy dtype description | `float_` was described as 32-bit, and `float16_` was used as a type name in the explanation | Use explicit `float32`, `float64`, `int32`, and `uint8` examples. The old `np.float_` alias was removed in NumPy 2. |
| NumPy indexing and transpose | The descriptions restricted indices to positive integers and transpose to one or two dimensions | Explain zero/negative indexing and demonstrate a three-axis transpose. |
| NumPy wrong-example cell | `np.array(1,2,3,4)` stops a top-to-bottom run | The guided shape-error demonstration catches its intentional exception. |
| NumPy teaching load | Many repetitive examples, wildcard imports, a variable called `list`, classes, and garbage collection compete for attention | Keep a short core. Retain selected shape/copy/randomness material as optional reference. Avoid wildcard imports and built-in name shadowing. |
| Garbage collection | Manual collection can be mistaken for a general memory-management fix | Explain that it does not reliably release a framework's GPU allocations; omit it from the first-lab workflow. |
| TensorBoard writer | The explanation names `tensorflow.summary.SummaryWriter`; the implementation installs `tensorboardX` while also using TensorFlow | Use the native `tf.summary.create_file_writer` and `tf.summary.scalar` APIs. This removes an unnecessary dependency; it is not a claim that tensorboardX is deprecated. |
| TensorBoard measurements | The HParams example uses arbitrary values labelled “accuracy”, and its `session_num` is not incremented | Compare actual MSE values from two small optimisation runs, with unique run folders and explicit step numbering. |
| TensorBoard size and setup | The original file is approximately 18 MB and downloads MNIST for one image example | Use a small synthetic image only in optional material; no dataset download is needed. |
| TensorFlow | The basic tensor and GradientTape examples remain useful, but stop before a connected learning workflow | Link tensor operations to prediction, loss, gradient, update, and experiment logging. |
| Assessment | The source notebooks mainly show completed examples | Add a separate assignment, feedback checks, interpretation questions, a proposed rubric, and Moodle instructions. |

Technical references: [NumPy 2 migration](https://numpy.org/doc/stable/numpy_2_0_migration_guide.html), [indexing](https://numpy.org/doc/stable/user/basics.indexing.html), [transpose](https://numpy.org/doc/stable/reference/generated/numpy.transpose.html), [TensorFlow autodiff](https://www.tensorflow.org/guide/autodiff), and [TensorBoard](https://www.tensorflow.org/tensorboard/get_started). Further sources and the original authors' attribution are included in each notebook.

Source snapshot: `malradhi/bme_dl_lab`, latest commit affecting `1st_week_lab` at review time: `d45562da04d7fb29c8ee68f08adf0cd7b4118e69` (9 September 2025). Sources and documentation were reviewed on 7 September 2026.

## Explain these points clearly

- **“Is this deep learning?”** The small line model is not deep. It exposes the same basic training steps that will later be applied to networks with many parameters.
- **“Why use TensorFlow when the lecture mentioned PyTorch?”** These labs retain the course's existing TensorFlow path. Arrays, losses, gradients, and experiment records are concepts that transfer between frameworks.
- **“Why does axis 0 give feature means?”** In this notebook, rows are samples and columns are features. Reducing the sample axis leaves one value for each feature. Axis numbers have no universal semantic meaning outside that layout.
- **“Why protect a constant feature?”** Its standard deviation is zero, so direct division is undefined. The specified lower bound makes this exercise numerically well-defined; dropping constant features is another modelling option.
- **“Why do I get a missing gradient?”** Check that the loss was computed inside the tape, depends on the variable, and uses TensorFlow operations. Do not convert to NumPy in the differentiable calculation.
- **“Does the larger learning rate always win?”** No. It wins here for this data scale and update budget. An overly large rate can diverge.
- **“Is this validation accuracy?”** No. It is training MSE on synthetic pairs. There is no held-out evaluation in this small optimisation exercise.
- **“Why is TensorBoard empty?”** Confirm T4 passed, the writer was flushed/closed, and the dashboard points to the current log directory. Refresh the panel. The saved plot and event readback provide an alternative while the display is being fixed.

## Proposed marking: 20 points

| Task | Points | Main evidence |
|---|---:|---|
| T1: arrays and selection | 4 | General function, correct dtype, slices, and per-feature means |
| T2: standardisation | 4 | Training-only statistics, correct axes, constant-feature handling, no input mutation |
| T3: gradient update | 5 | TensorFlow tensors/variable, MSE, GradientTape, update direction, returned values |
| T4: logging and plotting | 4 | Actual event values and steps (2), correctly labelled two-run plot (2) |
| T5: interpretation | 3 | Correct explanations of preprocessing, learning-rate comparison, and evaluation limits |
| **Total** | **20** | Adjust the weighting to your course arrangements |

These are proposed lab points, not a change to the course's overall assessment rules.
Review the code as well as the outputs. Public numerical checks cannot prove that a student used GradientTape, wrote a general solution, or understands the result.

Give partial credit. If T3 fails, inspect T4's implementation rather than automatically assigning zero to the dependent task. The written-answer check only checks that text is present.

Expected instructor results, rounded:

- T1 feature means: `[140.0, 0.5, 1.0]`.
- T2 new sample `[220, 1.3, 1]`: approximately `[3.5777, 3.5777, 0]`.
- T3 first example: loss `4.166667`, gradient `-3.333333`, updated weight `0.333333`.
- T4 initial MSE for both runs: `2.291667`. After 25 updates: approximately `0.753519` for rate `0.03` and `0.000009` for rate `0.3`.

Accept suitable floating-point tolerances and equivalent correct implementations.

## Moodle setup and text to paste

Use a file-submission assignment accepting **one `.ipynb` file**. A 5 MB file limit is ample for this pack's expected outputs. Set the due time to the actual end of your Wednesday lab in Moodle's configured time zone. If using the proposed rubric, set the maximum to 20 points.

Suggested title: **Deep Learning — Lab 1: Arrays, Gradients, and Experiment Logs**

Suggested assignment description:

> Complete tasks T1–T5 in `02_BME_DL_Lab1_Assignment.ipynb`.
>
> Submit your own executed notebook by the end of the lab. Name the file `YOUR_NEPTUN_DL_Lab01.ipynb`, replacing `YOUR_NEPTUN` with your code. Include your name, Neptun code, completed code, written answers, printed results, and the labelled loss plot.
>
> Before downloading, restart the runtime and run all cells in order. Read the check messages and fix unexpected errors. Download the `.ipynb` file from Colab and upload it here; a Colab sharing link alone is not a submission. Confirm that your submission has been saved.
>
> You may refer to the guided notebook and official documentation. Follow the course rules on collaboration and AI tools, and state any assistance used. You should be able to explain your submitted code.
>
> The assignment has 20 points. Correct code, clear reasoning, and interpretation are assessed. Public checks provide feedback but do not determine the final mark. Submit partial work on time if you cannot finish every task.

The embedded TensorBoard panel does not need to be preserved. The required notebook contains a standard Matplotlib plot and printed verification of the actual event-file values. No separate screenshots, log archive, or report are required.

## Preparation and validation

The computational cells in all three notebooks were executed in separate fresh CPU Python processes using IPython cell execution and rich output capture. The guide's plots and the completed assignment's checks passed. TensorBoard event files were read back and compared against both actual loss histories. The blank student notebook was also run: it gives intended incomplete-task feedback without unexpected execution errors, and its distributed copy has no outputs or filled answers.

Tested environment:

| Component | Version |
|---|---|
| Python | 3.12.13 |
| TensorFlow CPU | 2.20.0 |
| NumPy | 2.5.3 |
| TensorBoard | 2.20.0 |
| Matplotlib | 3.11.1 |

This was a code execution check, not a hosted Colab browser test. The TensorBoard display cells were excluded from that execution. Before class, open the guide in Colab and verify the setup, the core examples, and the live TensorBoard panel. Retain the ordinary plot as the display fallback.

Use Colab's existing packages if setup succeeds. If TensorFlow or TensorBoard is missing, this optional fallback can be run in a new code cell:

```python
%pip install tensorflow==2.20.0 tensorboard==2.20.0
```

Restart the runtime after installing and run from the top. The table above records the full environment actually tested; the fallback relies on the runtime's other compatible packages. It is not a recommendation to upgrade a working course environment during class. If the selected runtime is incompatible, use the instructor's tested environment.

The notebook files are ready for upload to Colab or Moodle. The GitHub repository has not been modified.
