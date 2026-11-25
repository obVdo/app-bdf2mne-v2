<h1 align="center">bdf2mne-v2</h1>

<p align="center">
  Converts a BioSemi BDF EEG recording to MNE-Python Raw (.fif) with mne.io.read_raw_bdf, detects events on the stimulus channel and produces raw-trace, PSD, sensor-layout and event QC figures.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.895"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.895-blue.svg" alt="Run on brainlife.io"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Converts a BioSemi BDF EEG recording to MNE-Python Raw (.fif) with mne.io.read_raw_bdf, detects events on the stimulus channel and produces raw-trace, PSD, sensor-layout and event QC figures.**

Reads the BDF file with MNE-Python, saves it as raw.fif and, when a stimulus channel is present, finds events on it (Status by default). Adds a raw trace of the first 20 s, a power spectral density up to 100 Hz, electrode layout and event plots to an HTML report and the brainlife product. Use it to bring BioSemi ActiveTwo recordings into brainlife MNE pipelines. The BIDS channels, electrodes and events sidecars of the input are not applied.

| | |
|---|---|
| **Inputs** | `neuro/eeg/bdf` |
| **Outputs** | `neuro/meeg/mne/raw` |
| **Container** | `brainlife/mne:1.0.2` |
| **Resources** | 1 CPUs · 16 GB memory · 1 h walltime |
| **Run it** | [doi.org/10.25663/brainlife.app.895](https://doi.org/10.25663/brainlife.app.895) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `bdf` | `neuro/eeg/bdf` | yes | BioSemi BDF recording with optional BIDS sidecars (channels, electrodes, coordsystem, events). |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `neuro/meeg/mne/raw` | Raw EEG/MEG data converted from BDF to MNE FIF format |

<details>
<summary><b>Parameters</b> (1)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `stim_channel` | string | `Status` | Name of the stimulus channel used for event detection with mne.find_events; empty means auto-detect (BioSemi default Status). |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.895](https://doi.org/10.25663/brainlife.app.895), choose `neuro/eeg/bdf` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/meeg/mne/raw`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlife/mne:1.0.2`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "bdf": "/path/to/bdf/bdf",
       "channels": "/path/to/bdf/channels",
       "electrodes": "/path/to/bdf/electrodes",
       "coordsystem": "/path/to/bdf/coordsystem",
       "events": "/path/to/bdf/events",
       "events_json": "/path/to/bdf/events_json",
       "stim_channel": "Status"
   }
   ```

3. Run `./main`.

## Configuration Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `bdf` | string | `""` | Path to input BDF file |
| `stim_channel` | string | `"Status"` | Stimulus channel name for event detection |
| `preload` | boolean | `false` | Load data into memory on open (unused, always loaded) |

## Technical Details

- Reads BDF using `mne.io.read_raw_bdf()`
- Saves to FIF using `raw.save()` with `overwrite=True`
- Event detection via `mne.find_events()` on the `Status` stim channel (BioSemi default)
- PSD computed up to 100 Hz or Nyquist − 1, whichever is lower
- Sensor layout plot generated if EEG channel positions are present

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- bdf2mne-v2. brainlife.io App. [doi:10.25663/brainlife.app.895](https://doi.org/10.25663/brainlife.app.895)
- Gramfort A. et al. MEG and EEG data analysis with MNE-Python. Frontiers in Neuroscience 7:267 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Brainlife.io Team

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>
