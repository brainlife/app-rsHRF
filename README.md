<p align="center">
  <img src="https://github.com/guorongwu/rsHRF/raw/master/docs/BOLD_HRF.png" alt="BOLD signal and retrieved HRF" width="480">
</p>

<h1 align="center">rsHRF</h1>

<p align="center">
  Voxel-wise resting-state hemodynamic response function (HRF) estimates and deconvolved BOLD from a resting-state fMRI time series, with the rsHRF toolbox.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.208"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.208-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

> **This branch, `1.0.8`, is the brainlife.io App rsHRF** ([doi:10.25663/brainlife.app.208](https://doi.org/10.25663/brainlife.app.208)).

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Estimates the voxel-wise resting-state hemodynamic response function (HRF) and deconvolves BOLD fMRI within a brain mask with the rsHRF toolbox (BIDS-Apps rsHRF 1.0.8).**

Runs rsHRF on the BOLD image with the mask passed as --atlas and the chosen estimation basis (canonical with 2 derivatives, smoothed or plain FIR, Gamma, Fourier or Hanning). rsHRF retrieves the onsets of pseudo-events from resting-state BOLD to estimate the HRF and deconvolve the signal. Results and QC plots are written to the results directory.

| | |
|---|---|
| **Inputs** | `neuro/func/task` · `neuro/mask` |
| **Outputs** | `raw` |
| **Container** | `bids/rshrf:1.0.8` |
| **Resources** | 8 CPUs · 6 h walltime |
| **Run it** | [doi.org/10.25663/brainlife.app.208](https://doi.org/10.25663/brainlife.app.208) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `bold` | `neuro/func/task` | yes | Resting-state BOLD fMRI time series. |
| `mask` | `neuro/mask` | yes | brain mask (passes as --atlas) |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `results` | `raw` | rsHRF outputs: estimated HRF parameter maps, deconvolved BOLD and QC plots. |

<details>
<summary><b>Parameters</b> (2)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `estimation` | enum | `canon2dd` | estimation procedure Options: `canon2dd`, `sFIR`, `FIR`, `Gamma`, `Fourier`, `Hanning`. |
| `tempmask` | string |  | Enter the slice number that you like to scrub (skip). Enter string like "1,2,4,56,555" |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.208](https://doi.org/10.25663/brainlife.app.208), choose a BOLD time series and a brain mask (`neuro/mask`, passed to rsHRF as `--atlas`), pick the `estimation` method and press **Execute**. The results are archived as a `raw` dataset tagged `rsHRF`.

`estimation` sets the HRF basis:

- `canon2dd`: canonical shape with two derivatives (default);
- `sFIR`: smoothed finite impulse response;
- `FIR`: finite impulse response;
- `Gamma`, `Fourier`, `Hanning`: gamma basis set, Fourier basis set, Fourier basis with Hanning window.

### The method

rsHRF retrieves the onsets of pseudo-events that trigger a hemodynamic response from the voxel-wise resting-state BOLD signal. Based on point process theory, it fits a model that finds the optimal lag between the events and the HRF onset, and the HRF shape. Once the HRF is known for each voxel, it can be deconvolved from the time series (for example to improve lag-based connectivity estimates), or its shape parameters can be mapped across the brain, white matter included, and used as a pathophysiological indicator.

<p align="center">
  <img src="http://users.ugent.be/~dmarinaz/example_hrf.png" alt="Example of a retrieved HRF" width="420">
  <img src="http://users.ugent.be/~dmarinaz/FIR_Height_full_layout.png" alt="Map of FIR HRF height" width="420">
</p>

The input BOLD should already be preprocessed; in particular it should be band-pass filtered (about 0.01–0.08 Hz) and z-scored per voxel. To be on the safe side, rsHRF repeats these steps. See the [rsHRF BIDS-App documentation](http://bids-apps.neuroimaging.io/rsHRF/) and the [MATLAB version](https://github.com/compneuro-da/rsHRF) for more.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) and [`jq`](https://jqlang.github.io/jq/). The App runs the `bids/rshrf:1.0.8` container and can need a large amount of memory.

1. Clone this repository and check out the App branch: `git checkout 1.0.8`.
2. Create `config.json` with the paths to your inputs (see `config.json.sample`):

   ```json
   {
       "bold": "testdata/bold.nii.gz",
       "mask": "testdata/mask.nii.gz",
       "estimation": "canon2dd"
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

## Outputs

```text
results/        rsHRF outputs: HRF parameter maps, deconvolved BOLD and QC plots
product.json    the QC plots bold_plot_1.png and bold_plot_2.png, shown on brainlife.io
```

## Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Wu G-R, Liao W, Stramaglia S, Ding J-R, Chen H, Marinazzo D. A blind deconvolution approach to recover effective connectivity brain networks from resting state fMRI data. *Medical Image Analysis* 17:365–374 (2013).
- Wu G-R, Marinazzo D. Sensitivity of the resting state hemodynamic response function estimation to autonomic nervous system fluctuations. *Philosophical Transactions of the Royal Society A* 374:20150190 (2016).
- Wu G-R, Marinazzo D. Retrieving the Hemodynamic Response Function in resting state fMRI: methodology and applications. *PeerJ PrePrints* (2015).
- rsHRF. brainlife.io App. [doi:10.25663/brainlife.app.208](https://doi.org/10.25663/brainlife.app.208)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Soichi Hayashi (brainlife.io App)
- Franco Pestilli (project director)

**Contributors:** Giulia Bertò

**rsHRF toolbox:** Guorong Wu, Nigel Colenbier, Sofie Van Den Bossche and Daniele Marinazzo; Madhur Tandon and Asier Erramuzpe (Python and BIDS-App).

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

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## License

[MIT](LICENSE). rsHRF itself is distributed under its own license.
