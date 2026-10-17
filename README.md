# BioVista OCSR Benchmark

## Project Overview
This project benchmarks BioMiner's pretrained MolGlyph recognition
component on 100 cropped chemical structure images from BioVista.
It measures conversion to SMILES, not full-PDF structure detection.

## Repository Contents
- `week 5.ipynb`: complete benchmarking pipeline with explanations.
- `requirements.txt`: required packages and recorded versions.
- `results/benchmark_sample.csv`: selected images and reference labels.
- `results/excluded_references.csv`: unreadable reference records.
- `results/molglyph_predictions.csv`: raw predictions, converted SMILES,
  evaluation scores, processing times, and errors.
- `results/benchmark_summary.csv`: overall performance.

## Required Downloads
1. Download the BioMiner source ZIP:
   https://github.com/jiaxianyan/BioMiner
   Extract it into `BioMiner-main` beside the notebook.
2. Use the OCSR download link in the repository's BioVista dataset table.
   Extract it so these paths exist:
   - `ocsr/data/`
   - `ocsr/labels/gd_SMIELS.csv`
3. Download `molglyph_large.pt`:
   https://huggingface.co/jiaxianustc/MolGlyph
   Place it directly inside `BioMiner-main/`.

The source, dataset, and model weights are downloaded separately.

## Environment Setup
Run these commands in Anaconda Prompt from the project folder:

```bash
conda create -n biovista-benchmark python=3.10 pip ipykernel -y
conda activate biovista-benchmark
python -m pip install -r requirements.txt
python -m ipykernel install --user --name biovista-benchmark --display-name "Python (BioVista Benchmark)"
```

Extract BioMiner before installing requirements because the dependency
file includes its local MolScribe package.

## How to Run
Open `week 5.ipynb` in VS Code and select the BioVista Benchmark
Python environment. Run all cells from top to bottom.

The pipeline loads labels, matches images, validates reference SMILES,
selects a sample, runs MolGlyph, calculates metrics, and saves CSV files.
After setup, no manual intervention is required during execution.
Running again replaces the result files.

## Evaluation
- Sample: 100 images selected from readable references using seed 42.
- Exact match: canonical isomeric SMILES comparison.
- Tanimoto: Morgan fingerprints, radius 2, 2,048 bits, chirality enabled.
- Invalid or failed predictions receive zero credit and remain
  in the denominator.
- Unreadable references are excluded before sampling and recorded.
- Confidence scores are disabled because requesting them caused
  an error in the supplied implementation.

## Results
- Readable references: 8,860 of 8,861.
- Valid predictions: 96 of 100.
- Exact matches: 81 of 100.
- Exact-match accuracy: 81%.
- Mean Tanimoto similarity: 0.910436.
- Three invalid predictions and one conversion failure.
- The initial run took approximately 5.3 minutes on CPU.
  See the summary CSV for the latest measured processing times.

## Limitations
These results apply to the selected cropped-image sample.
They do not measure full-PDF detection or the entire BioVista dataset.
The sample does not enforce the official validation/test split,
and model training overlap was not independently checked.
Wildcard-containing reference structures are retained.

The notebook was rerun successfully from top to bottom in the
configured Windows/Python 3.10 environment. Installation on a
separate fresh machine has not been tested.
