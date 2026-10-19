# Transcriptomics and machine learning

This project uses the processed GEO expression matrix for GSE42568 to compare two classifiers on a clearly defined tissue-class task: breast cancer biopsy versus normal breast tissue. The aim is to demonstrate a reproducible expression-data workflow, not to build a clinical diagnostic tool.

## Dataset

- GEO: GSE42568
- Platform: GPL570 Affymetrix Human Genome U133 Plus 2.0
- 121 samples in the project matrix: 104 cancer and 17 normal
- Input matrix is downloaded from GEO when the workflow runs; raw CEL files are not included.

See [metadata/DATASET.md](metadata/DATASET.md) for provenance.

## Analysis

The Python workflow parses the GEO series matrix, constructs the binary tissue label from sample titles, and evaluates logistic regression and random forest with stratified five-fold cross-validation. Variance filtering and feature selection are fit inside each training fold to avoid leaking information from held-out samples. Metrics include ROC-AUC, average precision, accuracy, and balanced accuracy.

## Run locally

```bash
python -m pip install numpy pandas scikit-learn matplotlib
mkdir -p data
curl -fL 'https://ftp.ncbi.nlm.nih.gov/geo/series/GSE42nnn/GSE42568/matrix/GSE42568_series_matrix.txt.gz' -o data/GSE42568_series_matrix.txt.gz
gzip -t data/GSE42568_series_matrix.txt.gz
python scripts/run_ml.py
```

Results are written to `results/`, including the metrics table, feature ranking, report, and model-performance figures.

## Results and limits

The committed tables are a reference snapshot from a previous run, not proof of the latest standalone workflow result. Current outputs are uploaded by GitHub Actions after a successful run. The feature ranking is exploratory, and the tissue-class task within this dataset does not establish clinical diagnostic performance or external generalisation.

## Files

- `scripts/run_ml.py` — data parsing, cross-validation, model fitting, and plots
- `metadata/DATASET.md` — source and sample definition
- `results/` — reference outputs
- `figures/` — workflow diagrams
- `.github/workflows/transcriptomics-ml.yml` — automated analysis
