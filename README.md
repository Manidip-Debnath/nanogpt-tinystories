# Build and Pre-train Your Own GPT (nanoGPT-style, TinyStories)

**Author:** Manidip Debnath (manidipdeb@oakland.edu)  
**Hardware used for the saved run:** Google Colab, NVIDIA Tesla T4, one top-to-bottom run

## Contents

| Path | What it is |
|---|---|
| `notebook/nanogpt_pretraining_final_Manidip_Debnath_v4.ipynb` | The submission notebook: all 10 TODOs, Q1-Q6, Part 7 search, Part 8 final evaluation, Part 9 bonus (RoPE), with **all outputs saved**. |
| `report/IEEE_Report_Manidip_Debnath.pdf` | 5-page IEEE-style report (two columns, 10 figures, 5 tables). |
| `report/report.tex` | LaTeX source of the report (`pdflatex report.tex`, run twice; figures are in `report/figures/`). |
| `report/figures/` | The 14 plots exported from the notebook outputs (`cellNNN.png`, NNN = cell index) plus the two diagrams drawn for the report. |

## Results (from the saved outputs)

| Item | Result |
|---|---|
| Default model (3,000 steps, 35% of budget) | 0.8570 nats/char (requirement <= 1.00) |
| Final model (Part 7): context 256, batch 8, 14,715 steps, 1.99e14 FLOPs | **0.7016** nats/char, 1.012 bits/char, perplexity 2.017 |
| Baseline at the full budget (context 128, batch 32) | 0.7513 |
| Final setting on fresh seeds | 0.7077, 0.7088 (all below 0.725) |
| RoPE bonus, 3 seeds, 35% of budget | 0.8473 +/- 0.0036 vs 0.8561 +/- 0.0015 (learned), Welch p about 0.04 |

## How to use

* **Read:** open the notebook (Jupyter, VS Code or Colab). Outputs are already embedded, so nothing has to be run.
* **Do not re-run for submission.** GPU runs are not bit-for-bit reproducible. A new run changes the last digits of the losses and the generated samples, and the written answers quote the saved run.
* **Re-run (optional):** Colab with a T4 GPU, *Runtime -> Run all*, about 50-70 minutes. On a laptop CPU it takes several hours.
* **Rebuild the report:** `cd report && pdflatex report.tex && pdflatex report.tex` (needs a LaTeX installation with the `times`, `booktabs`, `titlesec`, `caption` packages).

## Notes

* The report uses an IEEE-style two-column layout built on the standard `article` class (the official `IEEEtran` class was not available when it was built). To submit to a venue that requires `IEEEtran`, change the preamble.
* Every number in the report is taken from the executed notebook.
* The Part 8 evaluation scores windows of the model's own `block_size`, so part of the gain from the longer context is an effect of the scoring. The notebook and the report both state this.
