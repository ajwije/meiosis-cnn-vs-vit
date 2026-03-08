# CNN vs. Vision Transformer for Meiosis Defect Detection

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ajwije/meiosis-cnn-vs-vit/blob/main/meiosis_cnn_vs_vit.ipynb)
![Python](https://img.shields.io/badge/python-3.10-blue)
![HuggingFace](https://img.shields.io/badge/🤗-HuggingFace-yellow)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0-red)

## The Story

In 2018, I built a CNN to classify meiosis microscopy images as **wildtype** or **ptd mutant** (PARTING DANCERS — a gene I characterized during my PhD). The model reported ~93% accuracy. It was completely meaningless: I had trained and validated on the same data.

Six years later, I revisited the same 127-image dataset using a pretrained Vision Transformer (ViT) and conducted an honest evaluation. The results tell a clear story about transfer learning in data-scarce biology.

## Results

| Model | Val Accuracy | F1 Macro | Notes |
|-------|-------------|----------|-------|
| 2018 CNN (reported) | ~93% ⚠️ | N/A | Train = Val data leak |
| CNN from scratch (honest) | 77% | 0.75 | Stratified split, weighted loss |
| ViT fine-tuned (honest) | 96% | 0.96 | `google/vit-base-patch16-224` |

## Why ViT Wins on 127 Images

A CNN trained from scratch must learn edges, textures, and shapes before it can recognize chromosomes. With ~100 training images, that's nearly impossible without overfitting.

A pretrained ViT has already learned visual structure from 14 million images. Fine-tuning teaches it only the biological difference between wildtype and ptd meiosis — a much easier task.

**The same principle applies to genomics:** DNABERT and ESM do for DNA sequences what ViT does for images — pretrained representations that transfer to small biological datasets.

## What's in the Notebook

- **Section 1** — Setup and dependencies
- **Section 2** — Image visualization and class distribution
- **Section 3** — CNN baseline (modernized from 2018, with proper train/val split)
- **Section 4** — ViT fine-tuning via HuggingFace
- **Section 4b** — 5-fold cross-validation (mean ± std)
- **Section 5** — Side-by-side comparison: accuracy, F1, confusion matrices
- **Section 6** — ViT attention map visualization
- **Section 7** — Key takeaways and connection to genomics

## Dataset

- **wildtype**: 47 microscopy images of normal meiosis
- **ptd mutant**: 80 images of meiosis defects (PARTING DANCERS knockout)
- **Format**: JPG, organized in two subfolders

> Images are not included in this repo. To run the notebook, upload your images to Google Drive with this structure:
> ```
> MyDrive/meiosis_data/wildtype/
> MyDrive/meiosis_data/ptd/
> ```

## How to Run

1. Click the **Open in Colab** badge above
2. Upload your images to Google Drive (structure above)
3. Enable GPU: `Runtime → Change runtime type → T4 GPU`
4. Run all cells in order

## Key Takeaway

> Even at 96% accuracy, the ViT attention maps show the model sometimes uses background features, not just chromosomes, to classify images. High accuracy does not replace biological validation.

## Related

- 📝 [Medium post — full walkthrough](https://medium.com/@asela_1/i-built-a-meiosis-classifier-in-2018-it-was-wrong-0d99e7279476)
- 🌐 [aselacompbio.com](https://www.aselacompbio.com)
- 💼 [LinkedIn](https://www.linkedin.com/posts/aselawijeratne_bioinformatics-machinelearning-transferlearning-activity-7436448156806029312-fsse?utm_source=share&utm_medium=member_desktop&rcm=ACoAAANu34sBJgcm6UMnEErodK6D_Q5o8SD19MY)

## Citation

If you use this code, please cite:

```
Wijeratne, A.J. (2026). CNN vs. Vision Transformer for Meiosis Defect Detection.
GitHub: https://github.com/ajwije/meiosis-cnn-vs-vit
```

## License

MIT
