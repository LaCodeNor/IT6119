# Sleep/Wake Classification from Apple Watch Motion and Heart Rate

Machine learning pipeline that classifies 30-second epochs as wake or sleep using
accelerometer and photoplethysmography heart rate data from an Apple Watch, with
polysomnography (PSG) as reference. Developed as a university course project.

## Dataset
PhysioNet "Motion and heart rate from a wrist-worn wearable and labeled sleep from
polysomnography" v1.0.0 (31 subjects, University of Michigan). Contents used:
- Acceleration (g), triaxial, sampling rate varies between subjects
- Heart rate (bpm) from PPG, irregularly sampled
- PSG sleep stage labels per 30-s epoch (wake, N1, N2, N3, REM)

The raw data are **not included** in this repository. Download them from
https://doi.org/10.13026/hmhs-py35 and place them in `labels/`, `motion/` and
`heart_rate/` next to the notebook.

## Analysis summary
- Labels recoded to wake (0) vs. sleep (1); unscored epochs (stage -1) removed
- Features computed per 30-s epoch: accelerometer magnitude statistics, mean and SD of heart rate
- One subject (7749105) excluded due to accelerometer coverage of 14 % of PSG epochs
- Train/test split performed at subject level to avoid data leakage

## Usage
Run `final_project_laho.ipynb` from top to bottom.

## Citations
Resource Citation
Walch, O. (2019). Motion and heart rate from a wrist-worn wearable and labeled sleep from polysomnography (version 1.0.0). PhysioNet. RRID:SCR_007345. https://doi.org/10.13026/hmhs-py35

Original Publication
Olivia Walch, Yitong Huang, Daniel Forger, Cathy Goldstein, Sleep stage prediction with raw acceleration and photoplethysmography heart rate data derived from a consumer wearable device, Sleep https://doi.org/10.1093/sleep/zsz180

Platform Citation
Pollard, T., Moody, B. E., Lehman, L., Gow, B., Fernandes, C., Xie, C., Johnson, A., Mark, R. G., & Heldt, T. (2026). PhysioNet as a global platform for biomedical research. Nature Health. https://doi.org/10.1038/s44360-026-00096-z. Available from: https://pmc.ncbi.nlm.nih.gov/articles/PMC13641116/
Data License
The underlying source data components are made available under the Open Data Commons Attribution License (ODC-By) v1.0.

## License
Source data: Open Data Commons Attribution License (ODC-By) v1.0.
Code in this repository: add a license (e.g. MIT) or state "all rights reserved".