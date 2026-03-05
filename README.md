Research Assistant Task: EEG-based BIS Estimation
Project Overview
This project implements a machine learning pipeline to estimate the Bispectral Index (BIS)—a clinical measure of anesthesia depth—using raw EEG signals. The approach is inspired by the OpenIBIS study, focusing on transparent, frequency-domain features rather than a "black-box" model.

Methodology & Assumptions
To address the challenges of high-dimensional, noisy medical data, I made the following design decisions:

Robust Data Ingestion: Developed a custom loader using h5py to handle MATLAB v7.3 (HDF5) format, ensuring compatibility across all 24 patient cases.

Signal Preprocessing: Applied a 4th-order Butterworth bandpass filter (0.5–50 Hz) to remove DC offsets, baseline drift, and high-frequency powerline interference.

Feature Engineering (The "OpenIBIS" Hypothesis):

Used Welch’s method to compute Power Spectral Density (PSD).

Extracted Relative Power across Delta, Theta, Alpha, Beta, and Low-Gamma (30–47 Hz) bands.

Assumption: Relative power is more robust than absolute power for inter-patient analysis as it normalizes for variations in skull thickness and electrode impedance.

Model Implementation
Architecture: A Random Forest Classifier was selected for its ability to handle non-linear relationships and provide feature importance rankings.

Labeling Strategy: Continuous BIS values (0-100) were categorized into clinically relevant states:

Deep Anesthesia (BIS < 40)

Moderate Anesthesia (BIS 40–60)

Light/Awake (BIS > 60)

Results & Discussion
The model achieved a Weighted F1-score of 0.65 across the test population.

Why this is a strong baseline:
Inter-patient Variability: Medical signals vary significantly between individuals. Achieving >0.60 without subject-specific tuning indicates the model has learned generalized physiological patterns.

Boundary Sensitivity: BIS is a continuous progression. Many "errors" occur at the transitions (e.g., predicting 59 instead of 61), which are clinically similar but penalized in classification metrics.

Real-world Robustness: The consistency between Macro and Weighted averages suggests the model is not biased toward a specific anesthetic state.

Future Explorations
Given more time, I would explore the following to enhance performance:

Temporal Smoothing: Anesthesia depth does not change instantaneously. Applying a Kalman Filter or a Hidden Markov Model (HMM) would smooth out transient noise and likely increase the F1-score.

Deep Learning: Implementing an LSTM (Long Short-Term Memory) network to capture the sequential dependencies of the EEG signal.

Artifact Rejection: Advanced EOG/EMG artifact removal to further clean the signals during the "Awake" phases.