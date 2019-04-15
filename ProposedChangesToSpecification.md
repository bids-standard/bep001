# Proposed changes to BIDS specification

This document captures all of the changes that the BEP001 team are proposing to the BIDS specification.

Table of contents:

* [Indexable metadata](#indexable-metadata)
* [Repetition time](#repetition-time)
* [Symbolic links](#symbolic-links)
* [Suffix](#suffix)

## Indexable metadata

`src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md`

"Sequence specifics" updated to include metadata for MT sequences and to include metadata for any sequence with spoiling gradients.

In "Anatomy imaging data" added indexable-metadata

`<indexable_metadata>-<index>` key-value pair

`acq-<label>` key-value pair

`part-<label>` key/value pair

## Repetition Time

### Proposed Change

Adjust the definition of `RepetitionTime` in section [4.1.x Task (including resting state) imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) and add two new fields to section [4.1.y Anatomy imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data).

### Justification

`RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 4.1.x and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts in 4.1.y.

## Symbolic links

### Proposed change

Files in the derived folder can be symbolically linked to the main BIDS folder.
For example, a derived quantitative map `derivatives/sub-01/anat/sub-01_R1.nii.gz` may be linked as `sub-01/anat/sub-01_T1w.nii.gz` to be used as a standard structural image for functional MRI processing pipelines.

### Justification

Whether a file is "derived" or "raw" depends on where you're starting from.
To give a specific example, some researchers will be working on _creating_ quantitative maps, while others will want to _use_ the map as an input file to a structural processing pipeline.
There is a possible natural cut off by saying that files in the BIDS specification are "raw" as they come off the scanner, and "derived" if offline processing has happened.
Unfortunately, it is not possible to harmonize datasets across different scanners when following this rule.
For example MP2RAGE images are sometimes used to construct `_T1uni`-images and `_T1map`s on the scanner system, but on some systems, this only happens post-hoc/"offline".

## Suffix

Updated "modality label" to "suffix".

