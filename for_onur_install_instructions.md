Hi Onur -   

  
## Data  
Multi-slice data is here   
  
`ls /fileserver/scratch/serge/yaml_correction_example_data_for_onur`  

it is a copy of test-retest MWF data from 'alireza6' mwf acquisitions.   

## YAML correction   
IF you have python installed with 'numpy' and 'pynrrd' packages, then skip `pip install`   

    `pip install pynrrd`  
    `pip install numpy`  
  
## Execute with: 
`python /home/ch215616/code/mwf/fix_yaml.py`  

## Notes
You can specify input to a .yaml or .nrrd file, or an entire directory. If it is a directory - it will check every .yaml and corresponding .nrrd file in a directory and re-order echoes (if they need to be re-ordered).  

Re-ordered echoes are saved as <old_name>_echoes_reordered.nrrd  

You can also specify a custom output directory to save to via '--outdir'   

e.g. 

`python /home/ch215616/code/mwf/fix_yaml.py -i /fileserver/scratch/serge/yaml_correction_example_data_for_onur`

Please let me know if you cannot access either the .python file or the data   


## Running ANIMA on multi-slice data  

I corrected one of the .nrrd MWF files in the data directory and saved it to new subdirectory:   
`/fileserver/scratch/serge/yaml_correction_example_data_for_onur/for_anima`

Next you need to convert .nrrd to .nii.gz to make it work with Anima.   
For some reason crlConvertBetweenFileFormats cannot deal with MWF data because it has 32 echoes, so I wrote my own conversion in python.   

(you need to have 'nibabel' library to use this python script.  Do this via `pip install nibabel` )

`python nrrd_to_nifti.py -i 15.20141218.slab2_myelin_6slices_TR2000_PF_RFnormal.xse2d32_echoes_reordered.nrrd` 

Warning: please note that this tool does NOT preserve the original .nrrd header. It loads the matrix from .nrrd and then saves it into a .nii.gz in the default simple header. No orientation etc.   

Warning: Very important - if you will use your own .nrrd to .nii.gz conversion tool you must make sure that the echoes are listed as LAST axes, i.e. (x,y,z,echoes), else Anima will give you wrong results. I had this mistake before.    

Run anima as I shown before (substitute name 'volunteer1.nii.gz' with the new nifti filename)  

## minimum example
`./bin/animaGMMT2RelaxometryEstimation -e 9 -o out2.nii.gz -i volunteer1.nii.gz   `

# with all possible outputs  
`./bin/animaGMMT2RelaxometryEstimation -e 9 -o out2.nii.gz -i volunteer1.nii.gz -O weights_im.nii.gz --out-m0 m0.nii.gz --out-b1 B1.nii.gz --out-sig sigma-square.nii.gz   `

## For anima installation instruction
I built a docker for anima here - https://github.com/sergeicu/anima-docker


Kind regards,
Serge 
