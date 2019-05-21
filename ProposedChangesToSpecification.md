# Proposed changes to BIDS specification

This document captures all of the changes that the BEP001 team are proposing to the BIDS specification.

Table of contents:

* [Repetition Time](#repetition-time)
* [B1plus fieldmaps](#b1plus-fieldmaps)
* [Suffix](#suffix)
* [Indexable metadata](#indexable-metadata)

## Repetition Time

### Proposed Change

Adjust the definition of `RepetitionTime` in section [4.1.x Task (including resting state) imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) and add two new fields to section [4.1.y Anatomy imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data).

### Justification

`RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 4.1.x and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts in 4.1.y.

## B1plus fieldmaps

### Proposed Change

Extend the part on `Fieldmaps` in [4.1.x Task (including resting state) imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) by also allowing
for storing B1+ fielmaps.

### Justification

For some anatomical MRI acquisitions, especially when doing quantiative MRI (qMRI), B1+ fieldmaps can be useful to get better estimates of the underlying physical parameters (e.g., T1 in T1 maps obtained with MP2RAGE-sequence, see Marques et al., 2013).

## Suffix 

### Proposed Change


### Justification 

Unlike other key labels that exist in the main specification (e.g. `task`), a designated key label (e.g. `modality_name` or `sequence_name`) can not semantically hold true for all the values that it may list for multiple quantitative MRI (qMRI) techniques. This is because qMRI methods can be addressed with respect to the following:
 * sequence names (MP2RAGE)
 * generated outputs (MTR, MTsat) 
 * developer-defined acronyms (DESPOT1, hMRI,etc). 
  
As a result, semantic relevance of any specific key label can get easily overloaded. We therefore suggest the use of the `_suffix`, as it is free from a designated key label. Nevertheless, this approach calls for a systematically created definition list of the `_suffix` entries. Otherwise, the approach becomes prone to the violation of one-to-one referral principle of the `_suffix` field. On the other hand, unorganized profileration of `_suffix` entries would eventually yield a noisy and unadaptable list of entries. 

Based on the reasons given above, to maintain a viable and well-balanced list of suffixes, BEP001 classify suffixes in three categories:

1. `Agglomerating suffixes` 
     * **Role:** Groups together files that belong to parametrically linked multiple scans intended for a well-defined qMRI application (e.g. `MPM`, `VFA`).

2. `Designation suffixes for qMRI maps` 
     * **Role:** Denotes the parameter contained within a single file of a quantitative map (e.g. `T1map`,`MTsat`).  

3. `Suffixes for conventional MRI contrasts`
     * **Role:** Denotes the type of the predominant contrast conveyed by a single file of a conventional anatomical image (e.g. `T1w`, `T2w`, `PDw`, `T2starw`). 

#### An important note on minimizing the use of weight tags for grouping qMRI inputs

Altering some acquisition parameters while keeping the remaining constant across multiple repetitions of the same sequence is a mainstream approach to data collection for quantitative mapping. As a matter of course, such intentional modifications on the acquisition parameters yield multiple images with "relatively different contrasts". Within the scope of a single qMRI application,distinguishing these images using conventional weight tags (i.e. `T1w`, `PDw` and `T2w`) emerges as an intuitive solution. However, outside the scope of that qMRI application, interchangeability of the images which are designated by conventional weight tags becomes questionable. 

Assume that `PDw` and `T1w` labels were assigned to the volumes acquired with a GRE sequence at long TR and different flip angles for a hypothetical qMRI application of `qFoo`. Also assume that the TR is long enough, so that both images have a quite similar T1 contrast. Even though these labels would be somewhat acceptible for the sake of distinguishing them from each other due to the difference in flip angles, the `PDw` label would no longer be tenable for the interchangeability of `PDw` images outside the scope of the `qFoo` protocol. 

Similarly, the description of the magnetization transfer saturation index map (`MTsat`) inputs as grouped by the `MTS` suffix constitutes a good real-world example from the curent proposal (please see the list of available suffixes). The method involves acquisition of three volumes, where files are named by `MTw`, `PDw` and `T1w` tags _by tradition_. The `MTw` refers to the volume acquired by playing magnetization transfer pulse as a preperation step, all the remaining parameters of `MTw` ideally match that of `PDw`. When compared side by side, the contrast distinction implied by the `PDw` and `T1w` labels is apprehensible. Given that these two volumes are acquired using a GRE sequence and with the same TR, the one with the higher flip angle is expected to attain a higher T1 contribution. However, outside the scope of the `MTsat` protocol, demarcation of `PDw` vs `T1w` for the identical set of sequence and parameters can easily become a subjective matter. Therefore, to avoid potentially unrepresentative use of the weight tag of `PDw`, following solution is provided: 

* Group input files needed to compute an `MTsat` map by the `MTS` agglomerating suffix.
* Change `MTw` tag to `MTon` and `PDw` tag to `MToff` to better denotate the relationship between these images.  
* Use `MTon`, `MToff` and `T1w` tags as values in the `acq-<label>` key-value pair to distinguish `MTS` files from each other.

```
sub-01_MTS.json [*]
sub-01_acq-MTon_MTS.nii.gz 
sub-01_acq-MTon_MTS.json [**]
sub-01_acq-MToff_MTS.nii.gz 
sub-01_acq-MToff_MTS.json [**]
sub-01_acq-T1w_MTS.nii.gz 
sub-01_acq-T1w_MTS.json [**]

* JSON file that contains non-varying metedata fields betweeen different `MTS` files. 
** JSON file that contains all the varying metadata fields, corresponding to the companion nifti file. 
```

In conclusion, weight tags denote the type of predominant contrast conveyed by a single file of a conventional anatomical image ONLY when used as a suffix. In all other cases,  the `acq-<label>` key-value pair can be still used to accodomate legacy naming conventions for the sake of accustomed familiarity, yet without interfering with the interchangeability principle. Nevertheless, minimizing the use of weight tags is highly encouraged to avoid confusion.  

For the `MTS` example given above, one would question why two `<indexable_metadata>-<index>` key-value pairs of `fa` and (a new key tag of) `mt` are not used to distinguish `MTS` images from each other? Answers to this question are:
1. `<indexable_metadata>-<index>` is applicable when the variable entries are _enumerable_. Whereas `mt` would attain  _categorical_ entries. Therefore, `mt` cannot be defined as a new key tag in the list of allowed key tags.
2. The list of `acq-<label>` key-value pairs would only define `MTon` and `MToff` for the `MTS` and could be used in junction with the `<indexable_metadata>-<index>` key-value pair of `fa`. Although this is a legitimate solution, it was _not preferred_ to keep convention somewhat closer to the accustomed format of `MTw`-`PDw`-`T1w` and to collapse two varying parameters (flip angle and MT) into one variable, so that the file list becomes easier to read for the multi-echo derivatives of the `MTS` method (e.g. `MPM`).              

#### BEP001 adopts following principles when a new `_suffix` entry is to be listed:

* List of available suffixes is a list of unique entries. Thus, more than one description for a single `_suffix` is not allowed. 
* Every suffix MUST belong to one and only one of the three suffix classes given above. 
* `Agglomerating suffixes` MUST attain a clear description of the qMRI application that they relate to. If available, hyperlinks to example applications and/or more detailed descriptions are encouraged.
* If there exist an `agglomerating suffix` which relates to one or many `designation suffix for qMRI map`, all the relevances MUST be indicated in the description of the `agglomerating suffix` by the "Associated output suffixes" expression.
* Unless the pulse sequence name is directly linked to a qMRI application (e.g. `MP2RAGE`),          




## Indexable Metadata 

### Proposed Change 

### Justification 

