## **PROPOSAL**: Adjust the definition of `RepetitionTime` in section [8.3.3 Task (including resting state) imaging data](https://docs.google.com/document/d/1HFUkAEE-pB-angVcYe6pf_-fVf4sCpOHKesUvfb8Grc/edit#bookmark=id.jm7qgqg5x2on) and add two new fields to section [8.3.2 Anatomy imaging data](https://docs.google.com/document/d/1HFUkAEE-pB-angVcYe6pf_-fVf4sCpOHKesUvfb8Grc/edit#bookmark=id.3pszfzgi4dpj).

**Justification**: `RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 8.3.3 and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts. 

What follows is the text from the current BIDS specification along with the suggested additions.
Additions and replacements are rendered twice: in easy to read markdown and in "code diff" format to make it easy to see the proposed changes.

---

### 8.3.2 Anatomy imaging data

Some meta information about the acquisition MAY be provided in an additional JSON file.
See Common MR metadata fields for a list of terms and their definitions.

There are also some OPTIONAL JSON fields specific to anatomical scans:

* `RepetitionTimeExcitation`: The time in seconds between successive excitation pulses that excite the same tissue.
As with `RepetitionTimePreparation` this corresponds to 
[DICOM Tag 0018, 0080](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)): 
"the period of time … between the beginning of a pulse sequence and the beginning of the succeeding (essentially identical) pulse sequence".
Note that although this would typically be called `RepetitionTime` please use `RepetitionTimeExcitation` for structural scans with multiple excitations as `RepetitionTime` is already defined as the amount of time that it takes to acquire a single volume in section 8.3.3 and to distinguish it from `RepetitionTimePreparation`.

* `RepetitionTimePreparation`: The period of time in seconds that it takes a preparation pulse block (prepulse) to re-appear at the beginning of the succeeding (essentially identical) pulse sequence. Common examples of preparation blocks are:<br />
   &nbsp; `Inversion prepulse`: An (spatially selective or non-selective) inversion RF pulse 
   applied prior to the excitation pulse to prepare a desired tissue contrast. Typically
   to create higher levels of T1 weighting.<br /> 
   &nbsp;`Magnetization transfer prepulse`: Spectrally selective (off-resonant) RF pulse(s) 
   that is/are applied prior to the excitation pulse to saturate protons associated with 
   macromolecules.<br /><br />
Please use RepetitionTimePrepation to define the segment (overall or outer) repetition time (TR), if you need to reserve `RepetitionTimeExcitation` field for the readout (echo-train or inner) TR. For example, in the MP2RAGE sequence, RepetitionTimePreperation corresponds to the MP2RAGE_TR, whereas RepetitionTimeExcitation stands for the TR within individual readout blocks. 

```diff
+ * `RepetitionTimeExcitation`: The time in seconds between successive 
+ excitation pulses that excite the same tissue.
- As with `RepetitionTimeInversion` this corresponds to 
+ As with `RepetitionTimePreparation` this corresponds to 
+ [DICOM Tag 0018, 0080](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)): 
+ "the period of time … between the beginning of a pulse sequence and the 
+ beginning of the succeeding (essentially identical) pulse sequence".
+ Note that although this would typically be called `RepetitionTime` please use
+ `RepetitionTimeExcitation` for structural scans with multiple excitations as 
+ `RepetitionTime` is already defined as the amount of time that it takes to 
+ acquire a single volume in section 8.3.3 and to distinguish it from 
- `RepetitionTimeInversion`.
+ `RepetitionTimePreparation`.
+
- * `RepetitionTimeInversion`: The time in seconds between successive inversion 
- pulses in an inversion recovery (IR) sequence, such as MP(2)RAGE.
- As with `RepetitionTimeExcitation` this corresponds to 
- [DICOM Tag 0018, 0080](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)):
- "the period of time … between the beginning of a pulse sequence and the 
- beginning of the succeeding (essentially identical) pulse sequence".
- Note that although this would typically be called `RepetitionTime` please use 
- `RepetitionTimeInversion` for structural scans with successive inversion pulses
- as `RepetitionTime` is already defined as the amount of time that it takes to
- acquire a single volume in section 8.3.3 and to distinguish it from 
- `RepetitionTimeExcitation`.
+ *`RepetitionTimePreparation`: The period of time in seconds that it takes a 
+ preparation pulse block (prepulse) to re-appear at the beginning of the succeeding  
+ (essentially identical) pulse sequence. A prepulse can refer one of the following:
+   `Inversion prepulse`: An (spatially selective or non-selective) inversion RF pulse 
+   applied prior to the excitation pulse to prepare a desired tissue contrast. Typically
+   to create higher levels of T1 weighting. 
+   `Magnetization transfer prepulse`: Spectrally selective (off-resonant) RF pulse(s) 
+   that is/are applied prior to the excitation pulse to saturate protons associated with 
+   macromolecules. 
+ Please use RepetitionTimePrepation to define the segment (overall or outer) repetition time (TR),
+ if you need to reserve `RepetitionTimeExcitation` field for the readout (echo-train or inner)
+ TR. For example, in the MP2RAGE sequence RepetitionTimePreperation corresponds to 
+ the MP2RAGE_TR, whereas RepetitionTimeExcitation stands for the TR within individual readout blocks. 
```

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
