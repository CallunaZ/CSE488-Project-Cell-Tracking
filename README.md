# CSE488 Project – Cell Segmentation & Tracking Starter

A reproducible Python starter repository for the Cell Tracking Challenge inspired assignment.
Students clone this repo, keep it private, and add the instructional staff as collaborators.

**Goals:**

- Provide scripts for downloading challenge data, evaluation tools, and helper assets.
- Offer reusable modules for feature extraction, classical ML baselines, and evaluation (IoU + SEGMeasure).
- Encourage clean, version-controlled experiments rather than ad-hoc notebook sessions.

## Getting Started

### 1. Use this template

On GitHub, click **Use this template → Create a new repository**.
- Keep the repo **private**.
- Name it something like `<yourname>-cell-tracking`.
- Add John Femiani and other TAs as collaborators so they can clone and grade your work.

### 2. Set up your environment

```bash
conda env create -f environment.yml
conda activate cse488-cell-tracking
pip install -e .
```

### 3. Download the data and evaluation tools

```bash
python scripts/setup_data.py Fluo-N2DH-GOWT1 --splits training test
```

This downloads the dataset into `artifacts/`.

### 4. Train the SVM baseline
To test out what you will be doing for the project, some demo code has been provided to you for a SVM model. However:

- `train_svm.py` only trains on 3 frames
- `eval_seg.py` only generates predictions for 1 frame
- `MySEGMeasure.py` will silently skip any frame without a corresponding mask, so your score will look great but be meaningless if you haven't generated all 92 masks

It is your job to expand upon this code for your project to train on all frames, generate predictions for every frame, and produce a meaningful evaluation score across the full dataset. A submission that only evaluates one frame will not be accepted.

To run the training script (feel free to run before modifying to get a feel for what you're doing): 

```bash
python scripts/train_svm.py Fluo-N2DH-GOWT1 \
    --track 01 --window 5 --samples 500 \
    --model-path artifacts/models/svm_rbf.pkl
```

### 5. Evaluate
Again, it is your job to expand on `eval_seg.py` in order to evaluate across all frames.

To run the evaluation script:

```bash
python scripts/eval_seg.py Fluo-N2DH-GOWT1 \
    --track 01 --model-path artifacts/models/svm_rbf.pkl --verbose
```

## Project Layout

```
CSE488-Project-Cell-Tracking/
├── README.md
├── pyproject.toml          ← package metadata and dependencies
├── environment.yml         ← conda environment
├── scripts/
│   ├── setup_data.py       ← download datasets + evaluation tools
│   ├── train_svm.py        ← train the SVM baseline
│   └── eval_seg.py         ← run predictions and evaluate
├── src/cell_tracking/
│   ├── config.py           ← paths and URLs
│   ├── data.py             ← download helpers
│   ├── evaluation.py       ← IoU computation + SEGMeasure wrapper
│   ├── features.py         ← sliding-window feature extraction
│   ├── models/
│   │   └── svm.py          ← SVM train/predict/save/load
│   └── cli.py              ← optional Typer CLI (same commands as scripts/)
├── notebooks/
│   └── 00_reference_colab.ipynb   ← REFERENCE ONLY – do not submit this
└── tests/
    └── test_features.py    ← tests for the feature extraction module
```

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `CELL_TRACKING_BASE` | `<repo>/artifacts` | Root folder for datasets, models, and results |
| `CELL_TRACKING_DATASETS` | same as above | Optional override for the dataset cache |

## What You Need to Submit

- A private GitHub repo containing your code, a working README with setup instructions, and an environment file.
- A final PDF report (not a notebook). See the assignment instructions for required sections.
- The repo URL and PDF submitted on Canvas.

## License

MIT License