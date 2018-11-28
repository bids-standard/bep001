
## 8.10 Structural multispectral imaging data

## 8.10.1 Filenames

Template:
~~~~
sub-<participant_label>/[ses-<session_label>/]
	anat/
		sub-<participant_label>[_ses-<session_label>][_acq-<label>][_rec-<label>][_fa-<index>][_inv-<index>][_echo-<index>][_part-<phase|mag>][_run-<index>]_<sequence_label>.nii[.gz]
~~~~
Structural MR data can be acquired with different parameter values, such as Echo Time(TE), flip angle, inversion time *during the same scan*.  An example of this is the MP2RAGE-sequence, where there are two GRE readout blocks at different "inversion times", after the same inversion pulse. Another example is a multi-echo FLASH sequence, where multiple echoes are acquired after the same excitation pulse. In order to represent such imaging data in the BIDS standard, the following syntax and keywords can be used.  

1. The   `[_fa-<index>][_inv-<index>][_echo-<index>]` key/value pairs correspond to a numerical index label user may use to distinguish different set of multispectral imaging data. `fa` for variation in flip angle, `echo` for multi echo data and `inv` inversion time. Please note that the index label is just an identifier and is not intended to be a source of scanning parameters which should be stored in sidecar JSON files.

2. The optional key `[_part-<phase|mag>]` allows for storing phase and magnitude images. In its absence the image is assumed to be magnitude. Phase images should be in radians and have a range of (0, 2 pi] (including 0, excluding 2 pi).

3. Possible sequences label describing sequences used in imaging  include, but are not limited to:

<table>
  <tr>
    <td>Name</td>
    <td>sequence_label</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>Driven Equilibrium Single-Pulse Observation of T1</td>
    <td>DESPOT1</td>
    <td>Also known as the Variable Flip-Angle (VFA)</td>
  </tr>
  <tr>
    <td>Driven Equilibrium Single-Pulse Observation of T2</td>
    <td>DESPOT2</td>
    <td></td>
  </tr>
  <tr>
    <td>(Multi-echo) Flash </td>
    <td>FLASH</td>
    <td></td>
  </tr>
  <tr>
    <td>(Multi-echo) Magnetization Prepared (2) Rapid Acquisition Gradient-Echo(es)</td>
    <td>MPRAGE</td>
    <td></td>
  </tr>
  <tr>
    <td>Fluid Attenuation Inversion Recovery</td>
    <td>FLAIR</td>
    <td></td>
  </tr>
  <tr>
    <td>Fast Spin-Echo</td>
    <td>TSE</td>
    <td>(Includes Spin-Echo techniques like SPACE/CUBE/VISTA)</td>
  </tr>
  <tr>
    <td>Spin Echo</td>
	<td>SE</td>
	<td></td>
	</tr>
<tr>
	<td>Magnetisation transfer-weighted FLASH</td>
	<td>MTw</td>
	<td>Introduced as part of MPM sequence
</td>
</tr>
<tr>
<td>Proton density weighted FLASH</td>
<td>PDw</td>
<td>Introduced as part of MPM sequence
</td>
</tr>
<tr>
<td>T1-weighted FLASH</td>
<td>T1w</td>
<td>Introduced as part of MPM sequence
</td>
</tr>
<tr>
<td>Low resolution GRE acquisition</td>
<td>RF_sens_head / RF_sens_body</td>
<td>Used for B1 receive field mapping, once acquired with the head coil and once with the body coil (see Papp et al.: Correction of inter-scan motion artifacts in quantitative R1 mapping by accounting for receive coil sensitivity effects, Magnetic Resonance in Medicine, 2016, 76, 1478–1485)</td>
</tr>
</table>

Note that if the sequence that is being used is identical to a simpler sequence, yet with multiple readout blocks that correspond to different acquisition parameters, one should use the simpler sequence label and identify the data corresponding to different readout blocks using numerical indices. Three prominent examples are multi-echo FLASH sequences (`sub-01_echo-1_FLASH.nii.gz and sub-01_echo-2_FLASH.nii.gz`), MP2RAGE sequences (`sub-01_inv-1_MPRAGE.nii.gz and sub-01_inv-2_MPRAGE.nii.gz`) and multi-echo MP2RAGE sequences (`sub-01_inv-1_MPRAGE.nii.gz and sub-01_inv-2_echo-1_MPRAGE.nii.gz`, `sub-01_inv-2_echo-2_MPRAGE.nii.gz` and `sub-01_inv-2_echo-3_MPRAGE.nii.gz`).

4. Furthermore, (quantitative) image-derived contrasts can be labeled as follows:

<table>
  <tr>
    <td>Name</td>
    <td>contrast_label</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>T1-map</td>
    <td>T1map</td>
    <td>Quantitative map of the estimated T1 relaxation rate (in seconds)</td>
  </tr>
  <tr>
    <td>T2-map</td>
    <td>T2map</td>
    <td>Quantitative map of the estimated T2 relaxation rate (in seconds)</td>
  </tr>
  <tr>
    <td>T2star-map</td>
    <td>T2starmap</td>
    <td>Quantitative map of the estimated T2* relaxation rate (in seconds).</td>
  </tr>
  <tr>
    <td>Quantitative Susceptibility Map</td>
    <td>QSM</td>
    <td>Quantitative map of magnetic susceptibility (in ppm).</td>
  </tr>
  <tr>
    <td>B1-map</td>
    <td>B1plusmap</td>
    <td>Quantitative map of relative strength of B1 field (exact flip angle = 1). </td>
  </tr>
  <tr>
    <td>RF-sensitivity</td>
    <td>B1minusmap</td>
    <td>Quantitative map of the B1 receive field (calculated e.g. as the division of a head and body coil measurement).</td>
  </tr>
  <tr>
    <td>R1-map</td>
    <td>R1map</td>
    <td>Quantitative map of the estimated R1 (= 1/T1) relaxation rate (in Hertz)</td>
  </tr>
  <tr>
    <td>R2star-map</td>
    <td>R2starmap</td>
    <td>Quantitative map of the estimated R2* (= 1/T2*) relaxation rate (in Hertz)</td>
  </tr>
  <tr>
    <td>PD-map</td>
    <td>PDmap</td>
    <td>Quantitative map of the estimated PD (proton density) (in p.u.)</td>
  </tr>
  <tr>
    <td>MT-saturation</td>
    <td>MTsat</td>
    <td>Semi-quantitative map of the estimated MT (magnetization transfer saturation) (in a.u.)</td>
  </tr>
</table>


## 8.10.2 Metadata in sidecar-JSON

Especially for quantitative structural MRI, the exact scanning parameters of the images are crucial information. Therefore, the following information should be included in the sidecar-JSON files.

**Meta-information from original BIDS (1.0.2)-specification**

* `EchoTime`: The echo time (TE) of the corresponding image, specified in seconds.

* `FlipAngle`: Flip angle of the excitation pulse of the corresponding image, specified in degrees.

* `InversionTime`: Time after the middle of inverting RF pulse to middle of excitation pulse train to detect the amount of longitudinal magnetization, specified in seconds. Corresponds to DICOM Tag 0018, 0082 "Inversion Time".

* `NumberShots`: The number of RF excitations need to reconstruct a slice or volume. Please mind that this is not the same as Echo Train Length which denotes the number of lines of k-space collected after an excitation

* `PartialFourier`: The fraction of partial Fourier information collected. Corresponds to DICOM Tag 0018, 9081 "Partial Fourier”.

* `PulseSequenceType`: A general description of the pulse sequence used for the scan (i.e., MPRAGE, Gradient Echo EPI, Spin Echo EPI, Multiband gradient echo EPI).

* `PulseSequenceDetails`: Information beyond pulse sequence type that identifies the specific pulse sequence used (i.e., "Standard Siemens Sequence distributed with the VB17 software," "Siemens WIP ### version #.##,” or “Sequence written by X using a version compiled on YYYY/MM/DD ”).

**Additional meta-information fields**

The RepetitionTime-field as specified by the main BIDS specification is defined as "The time in seconds between the beginning of an acquisition of one volume and the beginning of acquisition of the volume following it". This differs from the DICOM “Repetition Time” (0018, 0080) field. Rather, it corresponds to the “Temporal Resolution” (0020, 0110) field. However, this latter definition is not very useful for structural sequences. Therefore, this extension proposal introduces two additional meta-information fields:

* `RepetitionTimeExcitation`: Time between successive excitation pulses that excite the same tissue, specified in seconds. This corresponds to the ‘Repetition Time’ (0018, 0080)-field in the DICOM specification. Note that for most structural scans this is *not* the same as the RepetitionTime-field of the main BIDS specification, which specifies the amount of time that it takes to acquire a single volume.

* `RepetitionTimeInversion`: Time between successive inversion pulses in an inversion recovery (IR) sequence, such as MP(2)RAGE, specified in seconds.

Furthermore, for quantitative structural MRI, often we use calculated contrasts that are based on a set of raw scanner images. Therefore, we introduce two more fields:

* `BasedOn`: This field indicates which raw images are used to create the contrast. For example a unified T1-weighted image or a T1 map is based on two MP2RAGE images or a T2-star map based on a set of multi-echo FLASH images.

* `EstimationMethod`: This field indicates how an image contrast was calculated (e.g., for MP2RAGE, using the original method (Marques et a., 2010), or using regularisation (e.g.,  Marques & Gruetter, 2013).
