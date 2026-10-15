# putEMG Gesture Recordings

Group 01, CSE 475, Fall 2026, Department of Computer Science & Engineering, East West University.
Supervisor: Dr. Raihan Ul Islam, Associate Professor.

This repository holds our exploratory data analysis (EDA) of the **putEMG** dataset: surface EMG (muscle) signals recorded from the forearm while people make hand gestures. The EDA checks whether the data is complete and clean, how the gestures are distributed, and whether the gestures can be told apart from the signal.

## Team

| Name |
|---|
| Mashrur Bin Morshed |
| Munem Islam Prionto |
| Kazi Marufin Rahman |

## Repository structure

| Folder | Contents |
|---|---|
| `code/` | The EDA notebook `test_4_emg.ipynb` |
| `report/` | The EDA report: `EMG_EDA_Report_latex.pdf`, and its LaTeX source with the figures |
| `related_work/` | Papers and notes on related work, including the putEMG paper |
| `models/` | Reserved for trained models (none yet) |

## Dataset

putEMG is published by the Poznan University of Technology: https://biolab.put.poznan.pl/putemg-dataset/

| Item | Value |
|---|---|
| Subjects | 44 (36 men, 8 women), aged 19 to 37 |
| Recording days | 2 per subject, at least one week apart |
| Files used | 264 gesture files (`emg_gestures-*.hdf5`), 6 per subject |
| Files not used | 448 force files (`emg_force-*`), which have no gesture labels |
| Electrodes | 24 on the right forearm, in 3 bands of 8: `EMG_1` to `EMG_8` elbow, `EMG_9` to `EMG_16` middle, `EMG_17` to `EMG_24` wrist |
| Recording | 5120 Hz, 12-bit, raw ADC counts |

Labels: `TRAJ_GT` is the gesture the subject really made and is the label used here. `TRAJ_1` is the gesture they were asked to make. Codes: -1 relax, 0 idle, 1 fist, 2 flexion, 3 extension, 6 pinch thumb-index, 7 pinch thumb-middle, 8 pinch thumb-ring, 9 pinch thumb-small.

**The data is not stored in this repository.** Download it from the dataset page (the downloader scripts are at https://github.com/biolab-put/putemg-downloader) and keep it outside the repository.

The dataset is licensed CC BY-NC 4.0 (non-commercial use only). Please cite:

> Kaczmarek, P., Mankowski, T., Tomczynski, J. (2019). putEMG - A Surface Electromyography Hand Gesture Recognition Dataset. *Sensors*, 19, 3548. https://doi.org/10.3390/s19163548

## Main findings of the EDA

- The data is complete: 264 files, 44 subjects, 2 days each, 21.4 hours in total, no missing values.
- The classes are strongly imbalanced: idle is 52.0% of the time, relax 14.7%, and each of the seven gestures only 4.6 to 5.0%.
- In the example recording the real gesture equals the requested gesture in 87.7% of the samples, so `TRAJ_GT` should be used as the label.
- The raw signal has interference spikes (at 30, 50 and 60 Hz and their multiples), and channels 1 and 9 are the noisiest.
- Each gesture raises a different set of channels. The pinch gestures are the weakest, only 1.6 to 2.7 times the idle level in the example recording.
- Comparing a subject on two days with another subject (four recordings) is inconclusive.

The full details are in the report in `report/`.

## How to run the notebook

1. Install Python 3 with the packages:
   ```
   pip install pandas tables matplotlib scipy torch
   ```
   The notebook uses a GPU if one is available and otherwise the CPU, and prints which one in the first cell.
2. Download the dataset, and put `putEMG-participants.csv` from the dataset page next to the data.
3. Open `code/test_4_emg.ipynb` and set `DATA_DIR` and `PARTICIPANTS_FILE` in the Settings cell to your own paths.
4. Run all cells. Section 11 reads all 264 files and takes about 10 minutes.

## Notes for modelling

- Use `TRAJ_GT` as the label and drop the relax label (-1).
- Filter the signal before modelling (the EDA used the raw signal).
- Split training and test data by subject, never by random rows or windows.
- Judge a model by per-gesture results, because idle is about half of the data.

## Report

The report is `report/EMG_EDA_Report_latex.pdf`. Its LaTeX source is `EMG_EDA_Report_latex.tex` with a `figures/` folder, and can be compiled with any LaTeX installation (for example `tectonic EMG_EDA_Report_latex.tex`).
