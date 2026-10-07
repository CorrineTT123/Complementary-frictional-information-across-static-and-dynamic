**Complementary frictional information across static and dynamic touch supports human material recognition**  
Yi Tang, Zhirui Liu, Wenhao Xue, et al.

The study examines how information available during static contact and dynamic sliding supports material recognition. Recognition advantages vary across material pairs and favor the task associated with the larger frictional contrast. The manuscript also investigates electroadhesive rendering of static-to-dynamic friction transitions and coordinated friction feedback in HaptiShop.

The files provided here cover material-friction measurements, the controlled material-sequence recognition experiment, and selected analysis routines. See [Coverage of the manuscript](#coverage-of-the-manuscript) for the scope of this release.

## Repository contents

```text
.
├── README.md
├── Table_S1_raw_data_static_dynamic_friction_material_recognition.csv
├── raw data for static friction/       # 13 Excel workbooks
├── raw data for dynamic friction/      # 13 Excel workbooks
└── code/
    ├── calculate_relative_behavioral_cue_contribution.txt
    ├── Within-participant comparison of mean sequence-identification accuracy in the smaller- and larger-contrast groupings.txt
    ├── finger motion.txt
    └── finger speed.txt
```

**File-format note:** `Table_S1_raw_data_static_dynamic_friction_material_recognition.csv` is internally an Excel `.xlsx` workbook, despite its `.csv` extension. It contains 20 participant worksheets. Read it with an Excel reader, or make an `.xlsx` copy as described below. Do not load it with `read_csv`.

The four `.txt` files in `code/` contain Python source code.

## Material identifiers

| Identifier | Material |
| --- | --- |
| NP | Normal paper |
| CP | Coated paper |
| GP | Gloss paper |
| Glass / glass | Glass |
| Acrylic | Acrylic |
| MSS | Microfiber synthetic suede |
| Alc | Alcantara |
| Al | Aluminum |
| Cu | Copper |
| PE | Polyethylene |
| PET | Polyethylene terephthalate |
| AAO | Anodic aluminum oxide / anodized aluminum interface |
| F-AAO | Fluorinated silane-modified AAO interface |

The recognition workbook retains the paper labels `GCWF`, `LWC`, and `UWF`. These correspond to **GP, CP, and NP**, respectively. Preserve the original identifiers when importing the data, and use this mapping when comparing results with the manuscript.

## Friction measurements

The [static-friction](raw%20data%20for%20static%20friction/) and [dynamic-friction](raw%20data%20for%20dynamic%20friction/) directories each contain measurements for the 13 materials or interfaces listed above. The AAO and F-AAO workbooks include separate voltage-condition sheets for measurements without applied voltage and at 250 V DC.

### Experimental context

As described in the manuscript, friction was measured using a custom tribometer with a three-axis force transducer and a motorized translation stage. The fingertip was held in place while the sample moved tangentially. The nominal normal load was 0.5 ± 0.1 N, unless otherwise specified, at approximately 25 °C and 50 ± 5% relative humidity.

Static-friction characterization used gradual tangential loading with a spring-loaded stage. The maximum tangential force immediately before sliding defined the maximum static-friction force. Dynamic-friction characterization used the tangential-to-normal force ratio during steady sliding. Friction coefficients are dimensionless.

### Columns and interpretation

Most workbooks have two labeled columns:

| Column | Meaning | Unit |
| --- | --- | --- |
| `Ft` | Tangential force, retaining the recorded sign | N |
| `Fn` | Normal force | N |

Rows contain sequential samples. Most files do not include timestamps, a sampling rate, participant identifiers, trial boundaries, or annotations marking slip onset and steady sliding. Consequently, the row index alone should not be converted to elapsed time without the acquisition metadata. Negative tangential-force values occur in the recordings; establish the force direction and baseline before calculating friction magnitudes.

Extract the pre-slip peak and steady-sliding intervals according to the experimental protocol. Taking a maximum or mean over an entire unsegmented workbook does not necessarily reproduce the manuscript's friction coefficients.

### Workbook-specific notes

| File or condition | Reading notes |
| --- | --- |
| Dynamic directory: `AAO_dynamic friction with 0 and 250V DC.xls` | A genuine legacy `.xls` workbook. Sheets are `Off` and `250V`, with columns `Time`, `X`, and `Z`. The recorded `Time` increment is 0.01; its unit is not stated in the header. The `X` and `Z` values correspond to the `Ft` and `Fn` channels in the static AAO workbook. |
| Static directory: `AAO_static friction with 0 and 250V DC.xlsx` | Sheets are `250V` and `0V`, with labeled `Ft` and `Fn` columns. |
| Dynamic directory: `F-AAO_dynamic friction with 0 and 250V DC.xlsx` | Sheets are `250V DC ` and `0 `, both with `Ft` and `Fn` headers. Sheet names contain trailing spaces. |
| Static directory: `F-AAO_static friction with 0 and 250V DC.xlsx`, sheet `250 v DC ` | No column headers. Column A is tangential force (N); column B is normal force (N). There are 632 paired samples. Read with `header=None` to retain the first numeric row. |
| Same F-AAO static workbook, sheet `0V ` | No column headers. Column A is time; column B is the friction coefficient (dimensionless), already calculated rather than a force channel. The time unit is not specified. Column A contains 38,143 values beginning at 0 with increments of approximately 0.001; column B contains 7,360 values followed by blank cells. Read with `header=None` and retain rows with a measured coefficient. Do not divide column B by column A. |
| Static directory: `CP_Static friction.xlsx` | Data are in `Trial 1`; `Sheet1` is empty. |
| Static directory: `NP_static friction.xlsx` | Data are in `Trail 1` (original spelling); `Sheet1` is empty. |
| Dynamic directory: `GP_static friction.xlsx` | The filename says `static` although it is stored in the dynamic-friction directory. Retain this filename when locating the supplied file; the acquisition regime should be confirmed before reuse. |

The NP workbooks in the static and dynamic directories contain identical numeric force records. The AAO dynamic workbook also matches the force records in the corresponding static AAO voltage conditions to numerical precision (maximum absolute difference below 10⁻¹⁵ N), with an additional `Time` column. These files should not be counted as independent recordings solely because they are stored in different directories.

## Material-sequence recognition data

File: [Table_S1_raw_data_static_dynamic_friction_material_recognition.csv](Table_S1_raw_data_static_dynamic_friction_material_recognition.csv)

The workbook contains **320 analyzed trial records from 20 participants**. Worksheets are named `S1` through `S20`, while participant IDs are `S01` through `S20`. Each participant contributes 16 records: four material pairs × two tasks × two presented orders (`AB` and `BA`). Each participant–pair–task combination therefore has two analyzed trials.

Participants chose among four sequence responses: `AA`, `AB`, `BA`, and `BB`. Only trials presenting `AB` or `BA` are included in this workbook. Same-material presentations were interleaved during the experiment but are not part of this discrimination dataset. Exact-match accuracy is 1 only when the complete response matches the presented order. The nominal uniform-guessing baseline is 25%.

### Data dictionary

| Column | Meaning |
| --- | --- |
| `subj` | Participant identifier, `S01`–`S20`. |
| `pair` | Material-pair identifier, `P1`–`P4`. |
| `matA`, `matB` | Material labels defining A and B for the sequence. |
| `order` | Presented sequence, `AB` or `BA`. |
| `task` | `Dcue`: dynamic-sliding task; `Scue`: stationary-contact task. |
| `context` | `S_equal`: smaller static-friction contrast; `D_equal`: smaller dynamic-friction contrast. These labels indicate relatively similar friction, not exact equality. |
| `delta_mu_s` | Between-material static-friction coefficient difference, dimensionless. |
| `delta_mu_d` | Between-material dynamic-friction coefficient difference, dimensionless. |
| `correct` | Recorded exact-match correctness: 1 for correct and 0 for incorrect. All 320 values agree with `response == order`. |
| `trial_idx` | Within-participant record index, 1–16. |
| `response` | Reported sequence: `AA`, `AB`, `BA`, or `BB`. |
| `note` | Optional annotation; blank in the supplied records. |

### Material pairs and contrast groups

| Pair | `matA` | `matB` | Context | Δμs | Δμd | Larger-contrast task |
| --- | --- | --- | --- | --- | --- | --- |
| P1 | GCWF | LWC | S_equal | 0.08 | 0.43 | Dcue |
| P2 | UWF | LWC | D_equal | 0.52 | 0.06 | Scue |
| P3 | Glass | Acrylic | D_equal | 0.28 | 0.05 | Scue |
| P4 | MSS | Alc | D_equal | 0.25 | 0.07 | Scue |

The alternative task for each pair forms the smaller-contrast group. `delta_mu_s` and `delta_mu_d` describe differences **between materials**; they are distinct from the within-material static-to-dynamic transition difference, Δμs-d = μs − μd, studied in the electroadhesion experiment.

The stationary-contact task included rapid translations between materials, followed by stationary comparison intervals. It emphasizes stationary contact but does not isolate purely static-friction information.

## Analysis code

| File in `code/` | Purpose | Inputs and outputs |
| --- | --- | --- |
| `Within-participant comparison of mean sequence-identification accuracy in the smaller- and larger-contrast groupings.txt` | Recomputes exact-match accuracy, groups trials by frictional contrast, calculates participant-level summaries and paired statistics, and summarizes accuracy for each material pair and task. | Expects `subject trail.xlsx`; writes `Fig2_new_GH_plot_data.xlsx`. |
| `calculate_relative_behavioral_cue_contribution.txt` | Calculates chance-referenced scores and participant-level normalized task-performance indices. | Expects `subject trail.xlsx`; writes `relative_behavioral_cue_contribution.xlsx`. |
| `finger motion.txt` | Tracks a manually selected hand landmark using MediaPipe and OpenCV, estimates speed in pixels/s, and counts low-speed events. | Expects `2.mp4`; writes `tracking_output.mp4` and `finger_speed_data.csv`. |
| `finger speed.txt` | Tracks the index fingertip, applies spatial calibration, and exports raw and smoothed speed in cm/s with per-sample stop flags. | Expects `2.mp4` and `pixel_to_cm_ratio.txt` containing `cm_per_pixel=...`; writes `output_with_tracking.mp4` and `finger_speed_cm_with_stop.csv`. |

The comparison script retains older `Fig2g`/`Fig2h` variable, worksheet, and output names. These analyses correspond to **Fig. 3g,h** in the accompanying manuscript. The normalized task-performance analysis corresponds to **Fig. 3i** and Supplementary Note 1/Table S1.

The source code uses terms such as “cue contribution.” In the manuscript, these quantities are interpreted as **descriptive normalized task-performance indices**, not estimates of perceptual cue weights or optimal sensory integration.

### Requirements and execution

The scripts are written in Python. Their imports and file readers require:

| Task | Packages |
| --- | --- |
| Recognition analyses | `numpy`, `pandas`, `scipy`, `openpyxl`, `xlsxwriter` |
| Reading the legacy AAO `.xls` file | `xlrd` |
| Video tracking | `numpy`, `opencv-python`, `mediapipe`; `finger speed.txt` also imports `matplotlib` |

Exact package versions are not recorded in the supplied files. Both tracking scripts use `mediapipe.solutions.hands`, so their environment must expose that API. Video tracking also requires the source video, and the click-selection script requires a graphical display.

To run the recognition scripts with their existing default input filename, create a byte-for-byte `.xlsx` copy of the mislabeled workbook. Run this from the repository root:

```python
from pathlib import Path
from shutil import copyfile

source = Path("Table_S1_raw_data_static_dynamic_friction_material_recognition.csv")
destination = Path("subject trail.xlsx")
if not destination.exists():
    copyfile(source, destination)
```

This preserves all 20 worksheets. Then run either Python source file directly, or save a working copy with a `.py` extension:

```bash
python "code/calculate_relative_behavioral_cue_contribution.txt"
python "code/Within-participant comparison of mean sequence-identification accuracy in the smaller- and larger-contrast groupings.txt"
```

The scripts resolve input and output paths relative to the working directory and write their named Excel outputs there. Alternatively, set `INPUT_FILE` or `input_file` in a working copy of the relevant script to an `.xlsx` copy at another location.

### Read the supplied workbook without renaming it

```python
from pathlib import Path
import pandas as pd

path = Path("Table_S1_raw_data_static_dynamic_friction_material_recognition.csv")
sheets = pd.read_excel(path, sheet_name=None, engine="openpyxl")
trials = pd.concat(sheets.values(), ignore_index=True).dropna(how="all")
trials["exact_correct"] = (trials["response"] == trials["order"]).astype(int)

print(len(sheets), trials["subj"].nunique(), len(trials))  # 20, 20, 320
print((trials["correct"] != trials["exact_correct"]).sum())  # 0
```

### Analysis definitions and reference results

For the contrast comparison, calculate each participant's exact-match accuracy for the smaller- and larger-contrast groupings. The supplied design is balanced across pairs, so averaging the eight trials in each grouping is equivalent to averaging the four pair-specific accuracies.

For the normalized task-performance indices, the supplied script defines:

```text
A_d = accuracy for Dcue trials with context S_equal
A_s = accuracy for Scue trials with context D_equal
c   = 0.25

I_d = max((A_d - c) / (1 - c), 0)
I_s = max((A_s - c) / (1 - c), 0)

C_d = I_d / (I_d + I_s)
C_s = I_s / (I_d + I_s)
```

These calculations are performed within each participant before taking group means. Undefined values are retained as missing if both chance-referenced scores are zero. The indices use the selected diagnostic conditions: P1 for `A_d`, and P2–P4 for `A_s`.

Recalculation from the included 320 records gives:

| Quantity | Result |
| --- | --- |
| Mean smaller-contrast sequence-identification accuracy | 38.125% |
| Mean larger-contrast sequence-identification accuracy | 78.125% |
| Mean within-participant accuracy gain | 40.0 percentage points |
| Participants with higher / unchanged accuracy in the larger-contrast grouping | 19 / 1 |
| Mean dynamic-task performance index, C_d | 57.7946% |
| Mean static-task performance index, C_s | 42.2054% |

The tracking scripts use different low-speed criteria: `finger motion.txt` uses 120 pixels/s, while `finger speed.txt` uses 4 pixels/frame. Both use approximately 0.15 s to count a stop event. These speed criteria coincide only at 30 frames/s. Low-speed time and per-sample stop flags are accumulated separately from the minimum-duration criterion for counting events. Review these settings, spatial calibration, frame rate, and tracking gaps before comparing their outputs.

## Coverage of the manuscript

| Manuscript component | Content supplied here |
| --- | --- |
| Material-friction characterization, including comparisons discussed in Fig. 3 and Figs. S4, S5, S7, and S18 | Static and dynamic force workbooks, subject to the format and metadata notes above. Exact plotting intervals and a complete processing pipeline are not included. |
| Controlled material-sequence recognition, Fig. 3 and Table S1 | Trial records and scripts for the contrast comparison and normalized task-performance indices. |
| Free-exploration motion analysis, Fig. 2 and Figs. S1–S3 | Tracking code. Source videos, spatial-calibration files, and participant-level exploration results are not included. |
| Static-to-dynamic transition thresholds, Fig. 4 | AAO/F-AAO baseline and 250 V DC force measurements are included. Participant staircase records, the full voltage–friction calibration, and threshold tables are not included. |
| HaptiShop and virtual-switch experiments, Fig. 5 and related supplementary figures | Recognition-response tables, device firmware, waveform libraries, and switch evaluation data are not included. |

Figure numbers refer to the accompanying manuscript version. The manuscript and Supplementary Information are not included in this directory.

## Citation and contact

When using these data or code, cite the associated manuscript:

> Tang, Y., Liu, Z., Xue, W., et al. *Complementary frictional information across static and dynamic touch supports human material recognition.*

Corresponding authors:

- Zuankai Wang: [zk.wang@polyu.edu.hk](mailto:zk.wang@polyu.edu.hk)
- Yuan Ma: [y.ma@polyu.edu.hk](mailto:y.ma@polyu.edu.hk)

The manuscript reports informed consent and institutional approval from The Hong Kong Polytechnic University (HSEARS20250102004). Participant records in the recognition workbook use coded identifiers.
