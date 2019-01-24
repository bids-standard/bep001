## **PROPOSAL**: Adjust the definition of `RepetitionTime` in section [4.1.x Task (including resting state) imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) and add two new fields to section [4.1.y Anatomy imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data).

**Justification**: `RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 4.1.x and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts in 4.1.y.

*What follows is the text from the current BIDS specification with the suggested additions inclued.*
*The text is also shown in "code diff" format to make it easy to see the proposed changes.*

---

***Section starts at line 107 in [src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md](https://github.com/bids-standard/bids-specification/blob/v.1.1.2/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data)***

### Anatomy imaging data

***(Lines 109 - 171 not rendered)***

Some meta information about the acquisition MAY be provided in an additional
JSON file. See Common MR metadata fields for a list of terms and their
definitions. There are also some OPTIONAL JSON fields specific to anatomical
scans:

| Field name              | Definition                                                                                                                                         |
| :---------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ContrastBolusIngredient | OPTIONAL. Active ingredient of agent. Values MUST be one of: IODINE, GADOLINIUM, CARBON DIOXIDE, BARIUM, XENON. Corresponds to DICOM Tag 0018,1048. |
| RepetitionTimeExcitation | OPTIONAL. The time in seconds between successive excitation pulses that excite the same tissue. The DICOM tag that best refers to this parameter is [(0018, 0080)](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)). This field may be superseded by `RepetitionTimePreparation` for certain use cases, such as [MP2RAGE](https://infoscience.epfl.ch/record/172927/files/mp2rage.pdf). Use `RepetitionTimeExcitation` (in combination with `RepetitionTimePreparation` if needed) for anatomy imaging data rather than `RepetitionTime` as it is already defined as the amount of time that it takes to acquire a single volume in section 4.1.x. |
| RepetitionTimePreparation | OPTIONAL. The period of time in seconds that it takes a preparation pulse block to re-appear at the beginning of the succeeding (essentially identical) pulse sequence. |

#### A note on the definition of RepetitionTime

`RepetitionTime` is defined section 4.1.x as the amount of time that it takes to acquire a single volume.
While this is a common definition of `RepetitionTime` in the functional MRI analysis community, it is not the common definition in the MR Physics community.
Redefining a metadata term would invalidate BIDS formatted datasets so the terms `RepetitionTimeExcitation` and `RepetitionTimePreparation` were introduced by BEP001 in version 1.x.x to disambiguate the different uses of these terms for quanitatiative anatomical neuroimaging.

An example of an anatomical acquisition sequence that would require the `RepetitionTimeExcitation` metadata field is .......

Some anatomical acquisitions need to record two measurements of time.
`RepetitionTimePreparation` can be used to describe the time it takes for a preparation pulse block to re-appear at the beginning of the succeeding pulse sequence.
The pulse block may contain one prepulse or a train of prepulses.
Common examples of prepulses are an inversion prepulse and a magnetization transfer prepulse.
An inversion RF pulse applied prior to the excitation pulse may be used to prepare a desired tissue contrast, typically to create higher levels of T1 weighting.
A magnetization transfer prepulse is an off-resonant RF pulse that is applied prior to the excitation pulse to saturate protons associated with macromolecules.
(Note that there can be multiple off-resonanct RF pulses.)


An example of an anatomical acquisition sequence that would require the `RepetitionTimeExcitation` and `RepetitionTimePreparation` files is the MP2RAGE sequence (Marques et al. 2010).
For this sequence `RepetitionTimePreparation` corresponds to the `MP2RAGE_TR`, whereas `RepetitionTimeExcitation` stands for the TR within individual readout blocks.

Another example of an anatomical acquisition sequence that would require the `RepetitionTimeExcitation` and `RepetitionTimePreparation` files is a magnetization transfer weighted scan.
For this sequence `RepetitionTimePreparation` should be used to defines the segment (overall or outer) repetition time (TR) if `RepetitionTimeExcitation` should be reserved for the readout (echo-train or inner) TR. 

```diff
Some meta information about the acquisition MAY be provided in an additional
JSON file. See Common MR metadata fields for a list of terms and their
definitions. There are also some OPTIONAL JSON fields specific to anatomical
scans:

| Field name              | Definition                                                                                                                                         |
| :---------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ContrastBolusIngredient | OPTIONAL. Active ingredient of agent. Values MUST be one of: IODINE, GADOLINIUM, CARBON DIOXIDE, BARIUM, XENON. Corresponds to DICOM Tag 0018,1048. |
+| RepetitionTimeExcitation | OPTIONAL. The time in seconds between successive excitation pulses that excite the same tissue. The DICOM tag that best refers to this parameter is [(0018, 0080)](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)). This field may be superseded by `RepetitionTimePreparation` for certain use cases, such as [MP2RAGE](https://infoscience.epfl.ch/record/172927/files/mp2rage.pdf). Use `RepetitionTimeExcitation` (in combination with `RepetitionTimePreparation` if needed) for anatomy imaging data rather than `RepetitionTime` as it is already defined as the amount of time that it takes to acquire a single volume in section 4.1.x. |
+| RepetitionTimePreparation | OPTIONAL. The period of time in seconds that it takes a preparation pulse block to re-appear at the beginning of the succeeding (essentially identical) pulse sequence. |
+
+#### A note on the definition of RepetitionTime
+
+`RepetitionTime` is defined section 4.1.x as the amount of time that it takes to acquire a single volume.
+While this is a common definition of `RepetitionTime` in the functional MRI analysis community, it is not the common definition in the MR Physics community.
+Redefining a metadata term would invalidate BIDS formatted datasets so the terms `RepetitionTimeExcitation` and `RepetitionTimePreparation` were introduced by BEP001 in version 1.x.x to disambiguate the different uses of these terms for quanitatiative anatomical neuroimaging.
+
+An example of an anatomical acquisition sequence that would require the `RepetitionTimeExcitation` metadata field is .......
+
+Some anatomical acquisitions need to record two measurements of time.
+`RepetitionTimePreparation` can be used to describe the time it takes for a preparation pulse block to re-appear at the beginning of the succeeding pulse sequence.
+The pulse block may contain one prepulse or a train of prepulses.
+Common examples of prepulses are an inversion prepulse and a magnetization transfer prepulse.
+An inversion RF pulse applied prior to the excitation pulse may be used to prepare a desired tissue contrast, typically to create higher levels of T1 weighting.
+A magnetization transfer prepulse is an off-resonant RF pulse that is applied prior to the excitation pulse to saturate protons associated with macromolecules.
+(Note that there can be multiple off-resonance RF pulses.)
+
+An example of an anatomical acquisition sequence that would require the `RepetitionTimeExcitation` and `RepetitionTimePreparation` files is the MP2RAGE sequence (Marques et al. 2010).
+For this sequence `RepetitionTimePreparation` corresponds to the `MP2RAGE_TR`, whereas `RepetitionTimeExcitation` stands for the TR within individual readout blocks.
+
+Another example of an anatomical acquisition sequence that would require the `RepetitionTimeExcitation` and `RepetitionTimePreparation` files is a magnetization transfer weighted scan.
+For this sequence `RepetitionTimePreparation` should be used to defines the segment (overall or outer) repetition time (TR) if `RepetitionTimeExcitation` should be reserved for the readout (echo-train or inner) TR. 
```
---


### 8.3.3 Task (including resting state) imaging data

Some meta information about the acquisition MUST be provided in an additional JSON file.

Required fields:

* `RepetitionTime`: The time in seconds between the beginning of an acquisition of one volume and the beginning of acquisition of the volume following it (TR).
When used in the context of functional acquisitions this parameter best corresponds to [DICOM Tag 0020,0110](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0020,0110)): the "time delta between images in a dynamic of functional set of images" but may be found in [DICOM Tag 0018, 0080](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)): "the period of time in msec between the beginning of a pulse sequence and the beginning of the succeeding (essentially identical) pulse sequence".
(Remember that DICOM measures must be converted from milliseconds to seconds to fit the BIDS specification.)
Please note that this definition includes time between scans (when no data has been acquired) in case of sparse acquisition schemes.
This value needs to be consistent with the '`pixdim[4]`' field (after accounting for units stored in '`xyzt_units`' field) in the NIfTI header.
This field is mutually exclusive with `VolumeTiming`.

```diff
* `RepetitionTime`: The time in seconds between the beginning of an acquisition
of one volume and the beginning of acquisition of the volume following it (TR).
+ When used in the context of functional acquisitions this parameter best
+ corresponds to [DICOM Tag 0020,0110](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0020,0110)):
+ the "time delta between images in a dynamic of functional set of images" but
+ may be found in [DICOM Tag 0018, 0080](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)):
+ "the period of time in msec between the beginning of a pulse sequence and the
+ beginning of the succeeding (essentially identical) pulse sequence".
+ (Remember that DICOM measures must be converted from milliseconds to seconds
+ to fit the BIDS specification.)
Please note that this definition includes time between scans (when no data has
 been acquired) in case of sparse acquisition schemes.
This value needs to be consistent with the '`pixdim[4]`' field (after accounting
 for units stored in '`xyzt_units`' field) in the NIfTI header.
- This field is mutually exclusive with `VolumeTiming` and is derived from
- DICOM Tag 0018, 0080 and converted to seconds.
+ This field is mutually exclusive with `VolumeTiming`.
```
