# LoRA dialogue summarization

A small project that fine-tunes **SmolLM2-135M-Instruct** to summarize dialogues using LoRA.

The notebook follows: **load data → add LoRA → train → generate → evaluate**.

## Run

1. Download [`lora_with_evaluation.ipynb`](lora_with_evaluation.ipynb).
2. Upload it to [Google Colab](https://colab.research.google.com/).
3. Select a GPU and run the cells in order.

No paid API or Hugging Face login is required. If installation requests a restart, restart and continue
with the imports. To rerun in the same session, choose a new `RUN_DIR`.

## Experiment

- 1,000 training examples, 500 validation examples, three epochs, seed 42.
- LoRA rank 8 on query/value projections: **460,800 trainable parameters (0.34%)**.
- Select the checkpoint with the lowest validation loss.
- Test on 500 dialogues, retaining all three references per dialogue.
- Both model versions use the first 320 dialogue tokens and generate at most 128 tokens.

## Recorded results

These scores come from the completed Colab run: [ROUGE scores](test_rouge.csv),
[epoch losses](epoch_results.csv), and [run settings](experiment.json).

| ROUGE F1 (0–100) | Base | LoRA |
| --- | ---: | ---: |
| ROUGE-1 | 27.18 | 45.51 |
| ROUGE-2 | 9.29 | 18.71 |
| ROUGE-L | 20.42 | 36.71 |

Validation loss fell from **2.17 to 1.35**.
Epoch 3 was selected. ROUGE uses stemming, the best-F1 reference for each metric,
and the mean across dialogues.

## Limits

ROUGE measures overlap, not factual accuracy. Some summaries confuse speakers or repeat information.
69 test dialogues (13.8%) were shortened. Training/validation uses a 512-token limit, so long targets
can be partly truncated. This is one experiment with a small model and one seed.

The notebook is saved with clean outputs. The original executed notebook and training archives stay
local; generated checkpoints and model files are excluded from Git.

## Sources

- [SmolLM2 model](https://huggingface.co/HuggingFaceTB/SmolLM2-135M-Instruct)
- [DialogSum dataset](https://huggingface.co/datasets/knkarthick/dialogsum)
- [LoRA documentation](https://huggingface.co/docs/peft/package_reference/lora)
- [ROUGE implementation](https://github.com/google-research/google-research/tree/master/rouge)

DialogSum's dataset card lists CC BY-NC-SA 4.0; SmolLM2's model card lists Apache 2.0.
