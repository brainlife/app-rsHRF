# app-rsHRF

This is a brainlife wrapper for https://github.com/BIDS-Apps/rsHRF 

According to rsHRF site,

> This toolbox is aimed to retrieve the onsets of pseudo-events triggering an hemodynamic response from resting state fMRI BOLD voxel-wise signal. It is based on point process theory, and fits a model to retrieve the optimal lag between the events and the HRF onset, as well as the HRF shape, using either the canonical shape with two derivatives, or a (smoothed) Finite Impulse Response.

# input files

This App requires bold and brain mask. 

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


