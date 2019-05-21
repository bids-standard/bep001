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

Based on the reasons given above, to maintan a viable and well-balanced list of suffixes, BEP001 classify suffixes in three categories:

1. `Agglomerating suffixes` 
     * **Role:** Groups together files that belong to parametrically linked multiple scans intended for a well-defined qMRI application (e.g. `MPM`, `VFA`).

2. `Designation suffixes for qMRI maps` 
     * **Role:** Denotes the parameter contained within a single file of a quantitative map (e.g. `T1map`,`MTsat`).  

3. `Suffixes for conventional MRI contrasts`
     * **Role:** Denotes the type of the predominant contrast conveyed by a single file of a conventional anatomical image (e.g. `T1w`, `T2w`, `PDw`). 

BEP001 adopts following principles when a new `_suffix` entry is to be listed:

* List of available suffixes is a list of unique entries. Thus, more than one description for a single `_suffix` is not allowed. 
* Every suffix MUST belong to one and only one of the three suffix classes given above. 
* `Agglomerating suffixes` MUST attain a clear description of the qMRI application that they relate to. If available, hyperlinks to example applications and/or more detailed descriptions are encouraged.
* If there exist an `agglomerating suffix` which relates to one or many `designation suffix for qMRI map`, all the relevances MUST be indicated in the description of the `agglomerating suffix` by the "Associated output suffixes" expression.
* Unless the pulse sequence name is directly linked to a qMRI application (e.g. `MP2RAGE`),          




## Indexable Metadata 

### Proposed Change 

### Justification 

