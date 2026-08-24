# CU Boulder AI

Personal study workspace for CU Boulder AI coursework on Coursera, with a practical focus on computer vision, deep learning, ML systems, and MLOps.

## Current Track

**Computer Vision Specialization**

1. Introduction to Computer Vision
2. Deep Learning for Computer Vision
3. Modern AI Models for Vision and Multimodal Understanding

The initial goal is to complete the non-credit coursework first, then selectively upgrade courses to for-credit when it makes sense.

## Repository Layout

```text
.
├── courses/                 # Course-by-course notes and labs
│   └── computer-vision/
├── experiments/             # Independent hands-on experiments
├── notes/                   # Cross-course concepts and references
├── docs/                    # Study plan and progress tracking
└── pyproject.toml           # Local Python/Jupyter environment
```

## Study Loop

```text
lecture / concept
      ↓
explain it in my own words
      ↓
work through the math by hand
      ↓
implement a minimal version
      ↓
compare with PyTorch / torchvision
      ↓
connect it to GPU inference and MLOps
```

The repository is intentionally lightweight. Structure will evolve with the coursework rather than being designed up front.

## Quick Start

```bash
uv sync
uv run python -m ipykernel install --user --name cu-boulder-ai
uv run jupyter lab
```

## Links

- CU Boulder Computer Vision Specialization: https://www.coursera.org/specializations/computer-vision-cu
- CU Boulder Online Programs: https://www.colorado.edu/cs/academics/online-programs
