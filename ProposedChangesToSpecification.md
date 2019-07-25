# Proposed changes to BIDS specification

This document captures all of the changes that the BEP001 team are proposing to the BIDS specification.

Table of contents:

* [Indexable metadata](#indexable-metadata)
* [Acquisition metadata field](#acquisition-metadata-field)
* [Part metadata field](#part-metadata-field)
* [Repetition time](#repetition-time)
* [Symbolic links](#symbolic-links)
* [Suffix](#suffix)
* [B1plus fieldmaps](#b1plus-fieldmaps)
* [Repetition Time](#repetition-time)
* [B1plus fieldmaps](#b1plus-fieldmaps)
* [S0map](#s0map)

## Indexable metadata

Change in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md`.

There are a lot of different additional metadata fields that may be needed for structural brain imaging.
In this change we added a field called `indexable_metadata` to capture those changes.
The use is described in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#indexable_metadata-index-key-value-pair`.

The keys can be pulled from the "Sequence specifics" table (`/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#sequence-specifics`).
This table was updated to include metadata fields for MT sequences and to include metadata fields for any sequence with spoiling gradients.

## Acquisition metadata field

The `acq-<label>` key-value pair was added to capture a group of parametrically linked anatomical images.
This was updated in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#acq-label-key-value-pair`.

## Part metadata field

### Proposed change

Change in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#part-magphase-keyvalue-pair`.

The `part-<mag/phase>` key-value pair was added to distinguish the magnitude and phase parts of an acquisition.

We additionally recommend (but do not require) that phase images should be in radians and have a range of 0 (inclusive) to 2pi (not included).

## Repetition Time

### Proposed Change

Adjust the definition of `RepetitionTime` in section [4.1.x Task (including resting state) imaging data](/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) and add two new fields to section [4.1.y Anatomy imaging data](/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data).

### Justification

`RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 4.1.x and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts in 4.1.y.

## Symbolic links

### Proposed change

Files in the derived folder can be symbolically linked to the main BIDS folder.
For example, a derived quantitative map `derivatives/sub-01/anat/sub-01_R1.nii.gz` may be linked as `sub-01/anat/sub-01_T1w.nii.gz` to be used as a standard structural image for functional MRI processing pipelines.

This change was made in `/src/02-common-principles.md#symbolic-links`.

### Justification

Whether a file is "derived" or "raw" depends on where you're starting from.
To give a specific example, some researchers will be working on _creating_ quantitative maps, while others will want to _use_ the map as an input file to a structural processing pipeline.
There is a possible natural cut off by saying that files in the BIDS specification are "raw" as they come off the scanner, and "derived" if offline processing has happened.
Unfortunately, it is not possible to harmonize datasets across different scanners when following this rule.
For example MP2RAGE images are sometimes used to construct `_T1uni`-images and `_T1map`s on the scanner system, but on some systems, this only happens post-hoc/"offline".

## Suffix

The original specification uses `modality_label` at the end of each file name.
We have updated this to `suffix` as there are multiple modalities that are not captured in the proposed options, and because the boundaries between "modalities" differ by domain specification.
This change was made in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data`

## B1+ Fieldmaps

### Proposed Change

Extend the part on `Fieldmaps` in [4.1.x Task (including resting state) imaging data](/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) by also allowing
for storing B1+ fieldmaps.

### Justification

For some anatomical MRI acquisitions, especially when doing quantiative MRI (qMRI), B1+ fieldmaps can be useful to get better estimates of the underlying physical parameters (e.g., T1 in T1 maps obtained with MP2RAGE-sequence, see Marques et al., 2013).

## S0map

### Proposed Change

Add a suffix `_S0map` to store the intercept-parameter for when T2\*-decay curves are fit, for example using a multi-echo Gradien-Recalled Echo (GRE)-sequence.

Change in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#suffix`.

### Justification

Both structural and functional multi-echo sequences are becoming more and more common. By fitting an exponential T2\*-model to such data, a `S0-map` remains that contains contrast for proton density (PD) and B1+ and B1--effects. This can be useful for for example skull-stripping.
