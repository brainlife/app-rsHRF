[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.1-blue.svg)](https://doi.org/10.25663/bl.app.1)

# app-rsHRF

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

**Estimates the voxel-wise resting-state hemodynamic response function (HRF) and deconvolves BOLD fMRI within a brain mask with the rsHRF toolbox (BIDS-Apps rsHRF 1.0.8).**

Runs rsHRF on the BOLD image with the mask passed as --atlas and the chosen estimation basis (canonical with 2 derivatives, smoothed or plain FIR, Gamma, Fourier or Hanning). rsHRF retrieves the onsets of pseudo-events from resting-state BOLD to estimate the HRF and deconvolve the signal. Results and QC plots are written to the results directory.

Run it on brainlife.io: [https://doi.org/10.25663/brainlife.app.208](https://doi.org/10.25663/brainlife.app.208)

| Input | Datatype | Required | Description |
|---|---|---|---|
| `bold` | `neuro/func/task` | yes | Resting-state BOLD fMRI time series. |
| `mask` | `neuro/mask` | yes | brain mask (passes as --atlas) |

| Output | Datatype | Description |
|---|---|---|
| `results` | `raw` | rsHRF outputs: estimated HRF parameter maps, deconvolved BOLD and QC plots. |

| Parameter | Type | Default | Description |
|---|---|---|---|
| `estimation` | enum | `canon2dd` | estimation procedure Options: `canon2dd`, `sFIR`, `FIR`, `Gamma`, `Fourier`, `Hanning`. |
| `tempmask` | string |  | Enter the slice number that you like to scrub (skip). Enter string like "1,2,4,56,555" |

Runtime: container `bids/rshrf:1.0.8`; 8 CPUs; 6 h walltime.

<!-- brainlife-app:end -->

This is a brainlife wrapper for the resting state HRF estimation and deconvolution method. 
https://github.com/BIDS-Apps/rsHRF 

# input files

This App requires an fMRI (bold) and anatomical brain mask. 

```
{
	"bold": "testdata/bold.nii.gz",
	"mask": "testdata/mask.nii.gz",
	"estimation": "canon2dd"
}
```

The `estimation` parameter (analysis method) can be set to one of the following

```
	 'canon2dd (canonical shape with 2 derivatives), '
	 'sFIR (smoothed Finite Impulse Response) , '
	 'FIR (Finite Impulse Response)')
```

# Resting state HRF estimation and deconvolution.

[![PyPI version](https://badge.fury.io/py/rsHRF.png)](https://badge.fury.io/py/rsHRF)

Please refer to https://github.com/compneuro-da/rsHRF for MATLAB version

![BOLD HRF](https://github.com/guorongwu/rsHRF/raw/master/docs/BOLD_HRF.png)


# The basic idea 

This toolbox is aimed to retrieve the onsets of pseudo-events triggering an hemodynamic response from resting state fMRI BOLD voxel-wise signal.
It is based on point process theory, and fits a model to retrieve the optimal lag between the events and the HRF onset, as well as the HRF shape, using either the canonical shape with two derivatives, or a (smoothed) Finite Impulse Response.

![BOLD HRF](http://users.ugent.be/~dmarinaz/example_hrf.png)

Once that the HRF has been retrieved for each voxel, it can be deconvolved from the time series (for example to improve lag-based connectivity estimates), or one can map the shape parameters everywhere in the brain (including white matter), and use the shape as a pathophysiological indicator.

![HRF map](http://users.ugent.be/~dmarinaz/FIR_Height_full_layout.png)

# How to use the original toolbox 

The input is voxelwise BOLD signal, already preprocessed according to your favorite recipe. Important thing are:

* bandpass filter in the 0.01-0.08 Hz interval (or something like that)
* z-score the voxel BOLD time series

To be on the safe side, these steps are performed again in the code.

The input can be images (3D or 4D), or directly matrices of [observation x voxels].

It is possible to use a temporal mask to exclude some time points (for example after scrubbing).

The demos allow you to run the analyses on several formats of input data.

# Python Package and BIDS-app 

A BIDS-App has been made for easy and reproducible analysis. Its documentation can be accessed at:

http://bids-apps.neuroimaging.io/rsHRF/

# Collaborators 

* Guorong Wu
* Nigel Colenbier
* Sofie Van Den Bossche
* Daniele Marinazzo

* Madhur Tandon (Python - BIDS)
* Asier Erramuzpe (Python - BIDS)

**References**

1. Guo-Rong Wu, Wei Liao, Sebastiano Stramaglia, Ju-Rong Ding, Huafu Chen, Daniele Marinazzo*. "A blind deconvolution approach to recover effective connectivity brain networks from resting state fMRI data." Medical Image Analysis, 2013, 17:365-374. [PDF](https://github.com/guorongwu/rsHRF/raw/master/docs/2013_MIA.pdf)

2. Guo-Rong Wu, Daniele Marinazzo. "Sensitivity of the resting state hemodynamic response function estimation to autonomic nervous system fluctuations." Philosophical Transactions of the Royal Society A, 2016, 374: 20150190. [PDF](https://github.com/guorongwu/rsHRF/raw/master/docs/2016_PTA.pdf)

3. Guo-Rong Wu, Daniele Marinazzo. "Retrieving the Hemodynamic Response Function in resting state fMRI: methodology and applications." PeerJ PrePrints, 2015. [PDF](https://github.com/guorongwu/rsHRF/raw/master/docs/poster_OHBM2016_HRF.pdf)

# Funding 

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)

# TODO 
More input parameters cloud be exposed (https://github.com/BIDS-Apps/rsHRF/blob/4546c38feeb196cce5cfc60563398d32906891e6/rsHRF/CLI.py)



