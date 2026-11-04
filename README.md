
Nocturnal pulse and activity vs. Awake Classification Pipeline

This repository contains a machine learning pipeline that uses time-series motion (acceleration) and pulse (heart rate) data collected from an Apple Watch to classify a user's sleep state against gold-standard sleep lab labels. This project was developed as a university course exercise.


Dataset Description

The model is trained on a clinical dataset containing synchronized sensor outputs from 31 subjects under observational monitoring:
• Motion Logging: Continuous triaxial acceleration recordings measured in units of g.
• Heart Rate Tracking: Continuous beats-per-minute (bpm) measurements derived from photoplethysmography (PPG).
• Ground Truth Labels: Definitive sleep stage scores (Wake, N1, N2, N3, REM) obtained via gold-standard polysomnography (PSG).


Academic References & Citations

If you use or reference any codebase components from this repository, please include the following formal citations as requested by the primary data providers and hosting platform:

1. Resource Citation

Walch, O. (2019). Motion and heart rate from a wrist-worn wearable and labeled sleep from polysomnography (version 1.0.0). PhysioNet. RRID:SCR_007345. https://doi.org/10.13026/hmhs-py35

2. Original Publication

Olivia Walch, Yitong Huang, Daniel Forger, Cathy Goldstein, Sleep stage prediction with raw acceleration and photoplethysmography heart rate data derived from a consumer wearable device, Sleep https://doi.org/10.1093/sleep/zsz180

3. Platform Citation

Pollard, T., Moody, B. E., Lehman, L., Gow, B., Fernandes, C., Xie, C., Johnson, A., Mark, R. G., & Heldt, T. (2026). PhysioNet as a global platform for biomedical research. Nature Health. https://doi.org/10.1038/s44360-026-00096-z. Available from: https://pmc.ncbi.nlm.nih.gov/articles/PMC13641116/

Data License

The underlying source data components are made available under the Open Data Commons Attribution License (ODC-By) v1.0.


TEST!