# Fix_yaml_for_mwf


This function corrects the echo ordering for .nrrd files which have been generated with DICOM to NRRD conversion tool. In certain cases, the aforementioned DICOM to NRRD tool incorrectly orders the echoes of the signal. This function corrects that mistake. 

Function was tested for Myelin Water Function acquisition files (CPMG sequence).
Works with single slice (2D) and volumetric (3D) data. 

Specify directory or file with .nrrd extension. Each .nrrd file must have a matching .yaml file.

The script will check each .yaml file for presence of multiple echoes. If the order is incorrect, it will fix the .nrrd file ordering. 

If no explicit output directory is specified in the arguments, a new file name will be created in the same directory as the original input file with a suffix to its name - "_echoes_reordered.nrrd"
