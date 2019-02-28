## **PROPOSAL**: Allow symbolic links to files in derivatives folder

**Justification**: Whether a file is "derived" or "raw" depends on where you're starting from.
To give a specific example, some researchers will be working on _creating_ quantitative maps, while others will want to _use_ the map as an input file to a structural processing pipeline.
There is a possible natural cut off by saying that files in the BIDS specification are "raw" as they come off the scanner, and "derived" if offline processing has happened.
Unfortunately, it is not possible to harmonise datasets across different scanners when following this rule.
For example [GILLES - INSERT PHILLIPS EXAMPLE HERE!]

We propose that files in the derived folder can be symbolically linked to the main BIDS folder.
For example, a derived quantitative map `sub-01_R1.nii.gz` may be linked as `sub-01_T1w.nii.gz` to be used as a standard structural image for functional MRI processing pipelines.

[WE NEED AN ACCOMPANYING JSON, RIGHT?]

*What follows is the text from the current BIDS specification with the suggested additions included.*

---

***I'm not sure where to put this??***

Quantitative images/maps should be stored in the derivatives-folder, but can be
symlinked to the corresponding sourcedata-directory, to facilitate the easy use
of these images as input to processing workflows implemented as BIDS-apps.