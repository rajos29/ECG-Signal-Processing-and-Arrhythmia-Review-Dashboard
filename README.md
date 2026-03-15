# ECG Signal Processing and Arrhythmia Review Dashboard
## Overview

This project implements an end-to-end electrocardiogram (ECG) signal processing and arrhythmia analysis pipeline with an interactive review dashboard. The system ingests raw ECG recordings, applies biomedical signal preprocessing, detects cardiac events, extracts physiological features, and visualizes arrhythmia predictions within a unified dashboard interface.

The goal is to bridge the gap between raw electrophysiology signals and interpretable analysis workflows. Instead of training a model in isolation, this project exposes each step of the signal pipeline—filtering, peak detection, feature extraction, and classification—so that the behavior of the system can be visually inspected and evaluated.

The dashboard enables interactive review of:

raw ECG waveforms

filtered signals

detected R-peaks

beat segmentation

extracted physiological features

machine learning predictions

This project demonstrates core competencies in biomedical signal processing, electrophysiology analysis, time-series machine learning, and interactive data visualization.

## Motivation

Electrocardiograms are one of the most widely used diagnostic signals in medicine. However, raw ECG signals are:

noisy

high-frequency time-series data

difficult to interpret programmatically without significant preprocessing

Detecting arrhythmias requires a multi-stage pipeline that typically includes:

signal cleaning

beat detection

beat segmentation

feature extraction

arrhythmia classification

Most machine learning ECG projects focus only on the final classification step. This project instead emphasizes the entire electrophysiology analysis workflow, exposing intermediate signals and decisions through a dashboard-based interface.

The result is a research and engineering tool for exploring ECG signal behavior and arrhythmia detection pipelines.

## Dataset

The project uses the MIT-BIH Arrhythmia Database, a classic benchmark dataset for ECG analysis.

Dataset characteristics:

48 ECG recordings

~30 minutes per recording

360 Hz sampling frequency

two ECG leads per record

beat-level annotations provided by cardiologists

Annotations identify beat types such as:

Normal beats (N)

Premature ventricular contractions (PVC)

Atrial premature beats (APC)

Bundle branch blocks

These annotations serve as ground truth labels for beat classification.

Data ingestion is handled using the wfdb library.

## Project Objectives

The project focuses on building a reproducible ECG analysis workflow that includes:

biomedical signal preprocessing

R-peak detection

beat segmentation

physiological feature extraction

arrhythmia classification

interactive dashboard-based signal inspection

The system is framed as research and engineering analysis software, not as a clinical or diagnostic product.

## Signal Processing Pipeline

The ECG processing pipeline includes several stages commonly used in electrophysiology signal analysis.

### Baseline Wander Removal

Low-frequency drift caused by respiration or electrode motion is removed using a high-pass filter.

Typical cutoff frequency:

0.5 Hz

### Bandpass Filtering

A bandpass filter isolates the physiologically relevant ECG frequency range.

Typical ECG band:

0.5–40 Hz

This step suppresses:

high-frequency muscle noise

low-frequency baseline drift

### R-Peak Detection

Cardiac R-peaks are detected using an algorithm inspired by the Pan–Tompkins method, which identifies the QRS complex in ECG signals.

### Output:

timestamps of detected R-peaks

### Beat Segmentation

Individual beats are extracted by windowing around detected R-peaks.

Typical beat window:

-200 ms to +400 ms around the R-peak

These segments are used for feature extraction and model input.

### RR Interval Extraction

RR intervals are computed from successive R-peak locations:

RRₙ = Rₙ₊₁ − Rₙ

These intervals form the basis of heart rate and heart rate variability metrics.

## Feature Extraction

The pipeline computes physiological features from each beat and surrounding signal.

### Time-Domain Features

RR interval

mean RR

heart rate

SDNN (standard deviation of RR intervals)

RMSSD

Beat Morphology Features

R-peak amplitude

QRS width

beat energy

These features support both exploratory analysis and machine learning classification.

## Machine Learning Task

The machine learning component performs beat-level arrhythmia classification.

Two possible classification settings:

### Binary classification:

Normal vs Abnormal beats

or

### Multi-class classification:

Normal

Premature ventricular contraction

Other abnormal beats

### Possible modeling approaches:

Random Forest using engineered features

Gradient Boosting

1D Convolutional Neural Network applied to beat waveforms

### The model outputs:

predicted beat class

prediction probability

These predictions are visualized in the dashboard.

## Dashboard

A central design goal of the project is to make the dashboard a first-class component, not an afterthought.

The dashboard provides an interface for reviewing ECG signals, signal processing steps, and model predictions.

### Key Dashboard Panels
Signal Explorer

Displays the full ECG waveform with zoom and pan capabilities.

### Features:

navigation through recordings

beat markers

waveform cursor inspection

Raw vs Filtered Signal View

Displays the raw ECG signal alongside the filtered signal to visualize the effect of preprocessing.

R-Peak and Event Overlay

Detected R-peaks are overlaid on the ECG waveform.

This allows verification of peak detection accuracy and identification of abnormal beats.

### Beat Viewer

Displays individual beat segments centered on the R-peak.

Used to examine waveform morphology.

Feature Summary Panel

Displays computed signal features such as:

heart rate

RR statistics

HRV metrics

### Model Prediction Panel

Displays model outputs for each beat:

predicted label

ground truth label

prediction probability

## Abnormal Beat Navigator

Allows navigation to:

abnormal beats

misclassified beats

selected beat indices

This supports error analysis and debugging of the pipeline.
```
    Repository Structure
    ecg-arrhythmia-dashboard
    │
    ├── data
    │   ├── raw
    │   ├── processed
    │   └── metadata
    │
    ├── preprocessing
    │   ├── filters.py
    │   ├── baseline_removal.py
    │   └── peak_detection.py
    │
    ├── segmentation
    │   └── beat_extraction.py
    │
    ├── features
    │   └── feature_extraction.py
    │
    ├── models
    │   ├── train.py
    │   ├── model.py
    │   └── inference.py
    │
    ├── evaluation
    │   ├── metrics.py
    │   └── plots.py
    │
    ├── dashboard
    │   ├── app.py
    │   ├── components
    │   └── layout
    │
    ├── results
    │   ├── figures
    │   └── metrics
    │
    ├── requirements.txt
    │
    └── README.md
```

## Evaluation

### Model performance is evaluated using standard classification metrics:

accuracy

precision

recall

F1 score

Evaluation artifacts include:

confusion matrix

ROC curves

precision-recall curves

The dashboard also allows inspection of misclassified beats, enabling qualitative model analysis.

## Technologies

### Typical technology stack:

#### Python libraries:

NumPy

SciPy

Pandas

WFDB

Scikit-learn

PyTorch or TensorFlow

#### Visualization and dashboard:

Plotly

Dash or Streamlit

#### Signal processing:

SciPy signal processing tools

#### Expected Outcomes

By the end of the project the repository should contain:

a complete ECG preprocessing pipeline

R-peak detection implementation

beat segmentation and feature extraction

a trained arrhythmia classifier

evaluation metrics and plots

an interactive dashboard for reviewing ECG signals and predictions

documentation and reproducible scripts

## Disclaimer

This project is intended for research, education, and engineering demonstration purposes only. It is not a clinical diagnostic system and should not be used for medical decision-making.

## Author

Rafael Jimenez, using data from the MIT-BIH Arrhythmia Database
