# cbis-ddsm-mass-classification
# Patient-level comparison of training strategies for mammographic mass classification on CBIS-DDSM

Code accompanying the manuscript "Deep Learning Strategies for Mammographic Breast Mass Classification: A Controlled Comparison of Training from Scratch, Transfer Learning, and Fine-Tuning on CBIS-DDSM" (Aray Aryn, Orken Mamyrbayev).

## What the code does

The notebook compares five models for benign-versus-malignant classification of cropped breast mass images from CBIS-DDSM under one protocol:

- two CNNs trained from scratch (Simple CNN, CNN+BN),
- ResNet50 with a frozen ImageNet backbone,
- ResNet50 with fine-tuned layer4 (FT-L4) and layer3+layer4 (FT-L34).

It includes:

- automatic selection of the lesion crop versus the ROI mask for every record (a check that detects mask/crop mix-ups),
- patient-level stratified five-fold cross-validation with an inner patient-level validation split,
- pooled out-of-fold predictions, threshold selection on validation data (Youden index), and patient-level cluster bootstrap confidence intervals,
- supplementary experiments: preprocessing ablation (E2), record-level versus patient-level splitting (E3), crop-geometry controls and repeated seeds,
- generation of all tables and figures of the manuscript.

## Data

The data are not included in this repository. CBIS-DDSM is available from The Cancer Imaging Archive:

- Collection: https://www.cancerimagingarchive.net/collection/cbis-ddsm/
- DOI: https://doi.org/10.7937/K9/TCIA.2016.7O02S9CY
- Reference: Lee RS, et al. A curated mammography data set for use in computer-aided detection and diagnosis research. Sci Data. 2017;4:170177.

Only the mass files are used: `mass_case_description_train_set.csv` and `mass_case_description_test_set.csv`, together with the JPEG images of the cropped lesions and ROI masks. Set `BASE_DIR` in the configuration cell to the folder where you stored the data.

## How to run

The notebook was run in Google Colab with a Tesla T4 GPU. Run the cells strictly in order:

1. Step 0: mount Google Drive, import libraries, set paths, and the `CONFIG` dictionary (all hyperparameters).
2. Step 1: load the CSV files and build binary labels (malignant = 1; benign and benign without callback = 0).
3. Step 2: find the images and classify each file as lesion crop or ROI mask.
4. Step 3: preprocessing (padding to a square, resizing to 224 x 224) and caching.
5. Step 4: patient-level five-fold partition and dataset tables.
6. Step 5: dataset, models, training function, and a sanity check (the network must overfit 64 images).
7. Step 6: main experiment E1 (5 models x 5 folds).
8. Step 7: metrics, bootstrap confidence intervals, paired comparisons, and figures.
9. Steps 8-9: experiments E2 (normalization and augmentation) and E3 (data leakage).
10. Steps 10-11: feature maps and the workflow diagram (optional).
11. Additional cells: crop-size control, paired E2/E3 tests, longer training of the frozen model, repeated seeds, stretch versus padding.

Results are written to `revision_v2/` (`runs`, `figures`, `tables`, `weights`). The `tables` folder in this repository contains the output files used in the manuscript.

## Software environment

- PyTorch 2.11.0
- torchvision 0.26.0
- scikit-learn 1.6.1
- GPU: Tesla T4 (mixed precision)
- Random seeds: 42 for the fold assignment; 0 for all models in E1; 0, 1, 2 for the frozen ResNet50 and FT-L4

## License

MIT License.

## Citation

If you use this code, please cite the manuscript (reference to be added after publication).
