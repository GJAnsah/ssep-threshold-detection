# EEG-Based Detection of Somatosensory Perceptual Thresholds

EEG + machine learning for objective detection of perceptual thresholds during transcutaneous electrical stimulation. Toward automated calibration of somatosensory neuroprostheses.

![Experimental Setup](figures/experimental_setup.png)

---

## Motivation

Calibrating somatosensory neuroprostheses currently requires lengthy psychophysical procedures where participants verbally report whether they perceive stimulation. This process is:
- Time-consuming (30–60 minutes per session)
- Subjective and prone to response bias
- Fatiguing for participants

An objective, EEG-based method could detect the perceptual threshold by identifying the presence or absence of somatosensory evoked potentials (SEPs), enabling faster calibration and real-time adaptation of stimulation parameters.

---

## Approach

1. Deliver transcutaneous electrical stimulation at varying amplitudes (subthreshold → suprathreshold)
2. Record EEG time-locked to stimulation
3. Extract features using Common Spatial Patterns (CSP)
4. Classify epochs as threshold vs subthreshold using Linear Discriminant Analysis (LDA)
5. Validate with cross-validation and permutation testing

---

## Experimental Setup

### Equipment
| Component | Model |
|-----------|-------|
| EEG Amplifier | g.USBamp (g.tec) |
| Electrode Driver | g.GAMMAbox |
| Stimulator | DS8R (Digitimer) |
| Stimulation Electrodes | Transcutaneous, foot |

### Stimulation Protocol
| Parameter | Value |
|-----------|-------|
| Pulse width | 200 µs |
| Inter-pulse interval | 680 ms |
| Amplitudes | 7 levels (subthreshold → suprathreshold) |
| Trials per amplitude | 5 (baseline); 30–50 (planned) |
| Threshold determination | 2AFC psychophysics task (79% criterion) |

### EEG Montage

16-channel montage optimized for tibial somatosensory evoked potentials:

| Amp Input | Electrode | Region |
|-----------|-----------|--------|
| CH 1 | Fz | Frontal |
| CH 2 | FCz | Frontal-central |
| CH 3 | Cz | Central |
| CH 4 | C1 | Central |
| CH 5 | C2 | Central |
| CH 6 | C3 | Central |
| CH 7 | C4 | Central |
| CH 8 | CPz | Centro-parietal |
| CH 9 | CP1 | Centro-parietal |
| CH 10 | CP2 | Centro-parietal |
| CH 11 | CP3 | Centro-parietal |
| CH 12 | CP4 | Centro-parietal |
| CH 13 | Pz | Parietal |
| CH 14 | POz | Parietal-occipital |
| CH 15 | T7 | Temporal (control) |
| CH 16 | T8 | Temporal (control) |
| REF | A1/A2 | Earlobe |
| GND | AFz | Forehead |

![EEG Montage](figures/eeg_montage.png)

Montage designed based on tibial SEP literature emphasizing vertex (Cz/CPz) coverage where P37 component is maximal.

---

## Analysis Pipeline

```
Raw EEG
    │
    ▼
Preprocessing
    ├── Bandpass filter (0.5–50 Hz)
    ├── Common average reference
    └── ICA artifact rejection
    │
    ▼
Epoch extraction (-100 to 500 ms)
    │
    ▼
Feature extraction (CSP)
    │
    ▼
Classification (LDA)
    │
    ▼
Validation
    ├── Stratified k-fold CV
    └── Permutation test
```

---

## Repository Structure

```
eeg-perceptual-threshold/
├── README.md
├── data/
│   └── README.md              # Data access information
├── notebooks/
│   └── 01_baseline_analysis.ipynb
├── results/
│   └── 01_baseline_results.md
├── figures/
│   ├── experimental_setup.png
│   ├── eeg_montage.png
│   ├── permutation_histogram.png
│   └── csp_patterns.png
├── docs/
│   └── montage_references.md  # Literature review for electrode selection
└── .gitignore
```

---

## Results

See [results/01_baseline_results.md](results/01_baseline_results.md) for baseline analysis.

---

## References

1. **ACNS (2008)** Guideline 9D: Guidelines on short-latency somatosensory evoked potentials. [PDF](https://www.acns.org/pdf/guidelines/Guideline-9D.pdf)

2. **Gupta et al. (2025)** Extracting robust single-trial SEPs for non-invasive BCIs. *J Neural Eng* 22:056004.

3. **D'Anna et al. (2017)** A somatotopic bidirectional hand prosthesis with transcutaneous electrical nerve stimulation based sensory feedback. *Sci Rep* 7:10930. [Link](https://www.nature.com/articles/s41598-017-11306-w)

4. **Gozzi et al. (2024)** Unraveling the physiological and psychosocial signatures of pain by machine learning. *Med* 5(12):1495-1509. [Link](https://www.cell.com/med/fulltext/S2666-6340(24)00298-8)

5. **MacDonald et al. (2019)** Best practices in EEG/SEP recording. *Clin Neurophysiol* 130:161-179.

---

---

## License

MIT
