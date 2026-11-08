# Face verification in the wild

A group project for a biometrics course at Wrocław University of Science and Technology. It is a face login system
built on ArcFace, tested on the kind of images a real system gets: blurry, low resolution, or partly covered.

## What it does

1. Enrolls 100 identities from CelebA (and, in the original project, our own group) by storing ArcFace embeddings.
2. Verifies a claimed identity by comparing a new photo to the stored embeddings with cosine similarity.
3. Runs three experiments, each scored by Equal Error Rate (EER).

## Results

| Condition | No fix | With fix |
|---|---|---|
| Clean photos (1,000 trials) | 5.00% | n/a |
| Low resolution, 2x downscale | 5.64% | 5.52% |
| Low resolution, 4x downscale | 8.87% | 8.39% |
| Low resolution, 8x downscale | 47.36% | 46.39% |
| Glasses | 7.36% | 7.36% |
| Face mask | 49.12% | 42.56% |

The fixes are small on purpose. For low resolution it is contrast enhancement (CLAHE) and sharpening before detection.
For occlusion it is a cascade of four face detectors with a fallback when none finds a face.

The system holds up against glasses and moderate blur. A mask, or a face only about 22 pixels wide, takes it close to
a coin flip, and these fixes do not rescue it.

## Files

| Path | Contents |
|---|---|
| `verify.py`, `metrics.py` | Verification and EER / ROC metrics |
| `augment.py` | Synthetic low resolution, masks and glasses |
| `prepare_data.py`, `enroll.py`, `enroll_member.py` | Data splits and enrollment |
| `experiments/` | The three experiments |
| `results/` | Scores, metrics and ROC curves |
| `demo.py` | Side-by-side demo on a file or webcam |
| `TODO.md` | Step-by-step setup and run notes |

## Not in this repository

- Face photos and the embeddings database. They are biometric data, so they stay private. `database/embeddings.pkl` is
  created when you run `enroll.py`.
- The CelebA images. The CSV files only list image names and identity numbers, so you need your own copy of CelebA.
- `results/exp1/scores_clean.pkl`, which `experiments/exp1_clean.py` writes and the other two experiments read.
