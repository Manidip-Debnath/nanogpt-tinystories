# Build and Pre-train Your Own GPT (nanoGPT-style, TinyStories)

**Author:** Manidip Debnath (manidipdeb@oakland.edu)  
**Hardware used for the saved run:** Google Colab, NVIDIA Tesla T4, one top-to-bottom run of the whole notebook (Parts 1-9, Appendix A and Appendix B).

## Contents

| Path | What it is |
|---|---|
| `notebook/nanogpt_pretraining_final_Manidip_Debnath_v5.ipynb` | **The submission notebook**: all 10 TODOs, Q1-Q6, Part 7 search, Part 8 final evaluation, Part 9 bonus (RoPE), Appendix A (RoPE seeds), **Appendix B (follow-up runs)**, with all outputs saved. |
| `report/IEEE_Report_Manidip_Debnath.pdf` | 6-page IEEE-style report (two columns, 11 figures, 7 tables). |
| `report/report.tex` | LaTeX source of the report (`pdflatex report.tex`, run twice; figures are in `report/figures/`). |
| `figures/`, `report/figures/` | The plots exported from the notebook outputs (`cellNNN.png`, NNN = cell index of the v4 notebook), the Appendix B model-size plot, and the two diagrams drawn for the report. |

## Results (from the saved outputs)

| Item | Result |
|---|---|
| Default model (3,000 steps, 35% of budget) | 0.8585 nats/char (requirement <= 1.00) |
| Final model (Part 7): context 256, batch 8, 14,715 steps, 1.99e14 FLOPs | **0.7016** nats/char, 1.012 bits/char, perplexity 2.017 (tier <= 0.725) |
| Baseline at the full budget (context 128, batch 32) | 0.7478 |
| Final setting on fresh seeds | 0.7077, 0.7088 (all below 0.725) |
| RoPE bonus, 3 seeds, 35% of budget | 0.8476 +/- 0.0032 vs 0.8555 +/- 0.0026 (learned), Welch p = 0.031 |
| **Appendix B: window-length effect** | with equal 128-character windows the winner (0.7532) is not better than the baseline (0.7478): the window length alone explains the whole Part 8 difference |
| **Appendix B: model size** (context 256, batch 8) | width 96: 0.7292, **width 128 (winner): 0.7016**, 6 layers: 0.7129, width 192: 0.8142 |
| **Appendix B: other** | dropout 0.1: 0.7826 (worse); context 128 with batch 8: 0.7700 (worse than batch 32) |
| **Appendix B: RoPE at the full budget**, 3 seeds | 0.7212 +/- 0.0050 vs 0.7060 +/- 0.0038 for learned positions: RoPE is **worse** (Welch p = 0.016) |

## How to use

* **Read:** open the notebook (Jupyter, VS Code or Colab). Outputs are already embedded, so nothing has to be run.
* **Do not re-run for submission.** GPU runs are not bit-for-bit reproducible. A new run changes the last digits of the losses and the generated samples, and the written answers quote the saved run.
* **Re-run (optional):** Colab with a T4 GPU, *Runtime -> Run all*, about 2 hours for the whole notebook (50-70 minutes up to Appendix A, plus about 40 minutes for Appendix B).
* **Rebuild the report:** `cd report && pdflatex report.tex && pdflatex report.tex` (needs a LaTeX installation with the `times`, `booktabs`, `titlesec`, `caption` packages).

## Notes

* The report uses an IEEE-style two-column layout built on the standard `article` class (the official `IEEEtran` class was not available when it was built).
* Every number in the report is taken from the executed notebooks.
* The Part 8 evaluation scores windows of the model's own `block_size`. Appendix B shows that the whole gain from the longer context is an effect of this scoring, not of a better language model. The notebook and the report both state this.
* The baseline (seed 1337) is not exactly reproducible across sessions (0.7478, 0.7513, 0.7552); the winner reproduced 0.7016 exactly. Appendix B runs use the learning rate 3e-3 for every model size and one seed each; this is stated as a limitation.

## Detailed report
`report/IEEE_Detailed_Report_Manidip_Debnath.pdf` (source: `report/report_detailed.tex`) is the full IEEE-style write-up: method, TODO-by-TODO explanation, compute accounting, all results, RoPE and model-size studies, complete Q1-Q6 answers, and a rubric/score mapping. The short 6-page version is `report/IEEE_Report_Manidip_Debnath.pdf`.
