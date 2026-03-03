## DeepBet - U-Net Brain extraction tool for nonhuman primates

Date: April 12th, 2021

----
## Description
This repo includes the brain extraction tool (DeepBet v1.0) for skull-stripping the nonhuman primate images. We also include brain masks of 136 macaque monkeys (20 sites) from [PRIME-DE](http://fcon_1000.projects.nitrc.org/indi/indiPRIME.html). The tool is constructed using a convolutional network - UNet model, initially trained on a [human sample](https://academic.oup.com/gigascience/article/5/1/s13742-016-0150-5/2737425) and updated with macaque data. 

In this repo, we also include the outputs from other tools (AFNI, FSL, FreeSurfer, ANTS) - [a glance of the performance for different pipelines](#A-galance-of-performance-for-different-pipelines)

Reference: [Wang et al., U-net model for brain extraction: Trained on humans for transfer to non-human primates, 2021, NeuroImage](https://www.sciencedirect.com/science/article/pii/S1053811921002780)

## Docker Image
#### Pull
The docker image has been uploaded onto DockerHub, download it by using the following command
```
docker pull sandywangrest/deepbet:1.0
```

#### Helper
For the usage of this image, run
```
docker run sandywangrest/deepbet
```

#### Storage Requirement
~5GB hard disk space for whole docker image, including pytorch (~4GB), nibabel, scipy (~188MB), 12 U-Net models (356MB) and our code (44KB)

## U-Net model
----
#### Local installation 

python3, numpy, pytorch, nibabel, scipy

#### Run brain mask prediction 
```
python3 /path_to_the_code/muSkullStrip.py -in /path_to_the_data/input_t1.nii.gz -model /path_to_the_model/selected_model.model -out /path_to_the_output_directory
```
Output: *_pre_mask.nii.gz

#### Custimize the model for your own dataset 
```
python3 /path_to_the_code/trainSs_UNet.py -trt1w /directory_of_the_training_images -trmsk /directory_of_the_training_image_masks -out /output_directory -vt1w /directory_of_the_validation_images -vmsk /directory_of_the_validation_image_masks -init /initial_model_to_start_with
```
Note: Our macaque model was a transfer-learning model using a human dataset as the 'initial model' (-init option). You can use the model we provided to custimize the model for your own dataset (even across species). 


#### The trained models can be used in prediction (**muSkullStrip.py -model**) or model-updating (**trainSs_Unet.py -init**)
1. **Site-All-T-epoch_36.model**: Trained on 12 macaques across 6 sites (2 macaques per site) from PRIME-DE. Six sites include newcastle, ucdavis, oxford, ion, ecnu-chen, and sbri.

2. **Site-All-T-epoch_36_update_with_Site_6_plus_7-epoch_09.model**: Trained on 19 macaques across 13 sites from PRIME-DE (12 macaques across 6 sites used in the first model and 7 macaques across 7 additional sites) Seven sites include NIMH, ecnu-k, nin, rockefeller, uwo, mountsinai-S, and lyon.

3. **Site-All-T-epoch_36_update_with_Site_*.model**: Site-specific model for NIMH, ecnu-k, nin, rockefeller, uwo, mountsinai-S, and lyon.

4. **Site-All-T-epoch_36_update_with_Site_Pigs_09.model**: The **pig** model - Trained on 12 macaques and updated with the pig data (N=3).

### [Download the models](https://github.com/HumanBrainED/NHP-BrainExtraction/tree/master/UNet_Model)

#### Manually edited brain masks for transfer-learning training (12 macaque monkeys from 6 sites, 2 per site)
![Training masks](https://github.com/TingsterX/PRIME-DE/blob/master/BrainExtraction/release/1_train12.gif)

#### Manually edited brain masks for model-updating training (7 macaque monkeys from 7 sites, 1 per site)
![Testing masks](https://github.com/TingsterX/PRIME-DE/blob/master/BrainExtraction/release/2_train7.gif)

#### Brain masks for 136 macaque monkeys (released mask)
![release](https://github.com/TingsterX/PRIME-DE/blob/master/BrainExtraction/release/4_release.gif)

### [Download brain masks (136 macaques)](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/brainmasks/brainmask_T1w_136macaques.tar)

### A galance of performance for different pipelines 
#### Data: 136 macaques (20 sites) from [PRIME-DE](http://fcon_1000.projects.nitrc.org/indi/indiPRIME.html).

[UNet 12+7 Model](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_UNet12%2B7.md)

[AFNI @animal_warper](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_AFNI-animalwarper.md)

[AFNI 3dSkullStrip](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_AFNI-3dSkullStrip.md)

[FLIRT+ANTS](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_FLIRT%2BANTS.md)

[FSL BET](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_FSL.md)

[FSL+ BET](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_FSL+.md)

[FreeSurfer](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_FS.md)

[FreeSurfer+](https://github.com/HumanBrainED/NHP-BrainExtraction/blob/master/PRIME-DE_BrainMask/vcheck_summary_FS+.md)

Reference: [Wang et al., U-net model for brain extraction: Trained on humans for transfer to non-human primates, 2021, NeuroImage](https://www.sciencedirect.com/science/article/pii/S1053811921002780)



# For BLAB users 


This is the implementation of the U-Net brain extraction + ANTs template-driven brain extraction of Wang et al., U-net model for brain extraction: Trained on humans for transfer to non-human primates, 2021, NeuroImage (https://www.sciencedirect.com/science/article/pii/S1053811921002780) on our lab data. 

The U-Net-only way often failed on our brain because we used surface coil on temporal regions, leading to poor contrast for frontal regions. 

<img width="1032" height="564" alt="image" src="https://github.com/user-attachments/assets/3a5e821c-21e0-467e-a3ed-8218f66b0a34" />

Using the U-Net mask + ANTs approch

<img width="1044" height="567" alt="image" src="https://github.com/user-attachments/assets/a0198cf7-a997-4852-9ee7-1b52dbc28493" />

All the annoying transformation were only used for integrating in the existing BLAB fMRI preprocessing pipelines for cross-session anatomical imaging alignment. 

##

## Setup

```bash
DATA_DIR=/space/data/blab/cooked/20M_/M_MaoDan_20260128/mri/orig/004
DOCKER_IMG=sandywangrest/deepbet:1.0
ANTS_DIR=/home/blab/wanru/ants-2.6.5/bin
export PATH=${ANTS_DIR}:$PATH

TEMPLATE_DIR=/home/blab/wanru/NMT_v2.0
TEMPLATE_BRAIN=${TEMPLATE_DIR}/NMT_v2.0_sym_SS.nii.gz
TEMPLATE_MASK=${TEMPLATE_DIR}/NMT_v2.0_sym_brainmask.nii.gz
```

## Step 0: ANTs preprocessing (denoising and N4)

```bash
${ANTS_DIR}/DenoiseImage -d 3 \
  -i ${DATA_DIR}/mprage.nii \
  -o ${DATA_DIR}/mprage_denoised.nii

${ANTS_DIR}/N4BiasFieldCorrection -d 3 \
  -i ${DATA_DIR}/mprage_denoised.nii \
  -o ${DATA_DIR}/mprage_denoised_n4.nii \
  -s 4 -b [200] -c [50x50x50x50,0.0]
```

## Step 1: Reorient to RAS

```bash
docker run -v ${DATA_DIR}:/data ${DOCKER_IMG} python3 -c "
import nibabel as nib
from nibabel.orientations import axcodes2ornt, ornt_transform
import numpy as np

orig = nib.load('/data/mprage_denoised_n4.nii')
orig_ornt = nib.io_orientation(orig.affine)
ras_ornt = axcodes2ornt(('R','A','S'))
fwd = ornt_transform(orig_ornt, ras_ornt)

ras_img = orig.as_reoriented(fwd)
hdr = ras_img.header.copy()
hdr.set_slope_inter(1.0, 0.0)
ras_out = nib.Nifti1Image(ras_img.get_fdata().astype(np.float32), ras_img.affine, hdr)
nib.save(ras_out, '/data/mprage_RAS.nii')
"
```

## Step 2: Deepbet skull strip

```bash
docker run -v ${DATA_DIR}:/data ${DOCKER_IMG} muSkullStrip.py \
  -in /data/mprage_RAS.nii \
  -model Site-All-T-epoch_36_update_with_Site_6_plus_7-epoch_09.model
```

## Step 3: U-Net prior brain (mask back to original orientation)

```bash
docker run -v ${DATA_DIR}:/data ${DOCKER_IMG} python3 -c "
import nibabel as nib
from nibabel.orientations import axcodes2ornt, ornt_transform
import numpy as np

orig = nib.load('/data/mprage_denoised_n4.nii')
orig_ornt = nib.io_orientation(orig.affine)
ras_ornt = axcodes2ornt(('R','A','S'))

mask_ras = nib.load('/data/mprage_RAS_pre_mask.nii.gz')
inv_xform = ornt_transform(ras_ornt, orig_ornt)
mask_orig = mask_ras.as_reoriented(inv_xform)

brain_data = orig.get_fdata() * (mask_orig.get_fdata() > 0)
brain_img = nib.Nifti1Image(brain_data.astype(np.float32), orig.affine, orig.header)
nib.save(brain_img, '/data/mprage_unet_brain.nii.gz')
"
```

## Step 4: Affine registration (U-Net brain → template)

```bash
${ANTS_DIR}/antsRegistrationSyN.sh -d 3 -t a \
  -f ${TEMPLATE_BRAIN} \
  -m ${DATA_DIR}/mprage_unet_brain.nii.gz \
  -o ${DATA_DIR}/unet2template_ \
  -n 8
```

## Step 5: Warp template mask into subject space

```bash
${ANTS_DIR}/antsApplyTransforms -d 3 \
  -i ${TEMPLATE_MASK} \
  -r ${DATA_DIR}/mprage_denoised_n4.nii \
  -t [${DATA_DIR}/unet2template_0GenericAffine.mat,1] \
  -o ${DATA_DIR}/template_mask_in_subject.nii.gz \
  -n GenericLabel
```

## Step 6: Refined mask (union U-Net + template) and final brain

```bash
docker run -v ${DATA_DIR}:/data -v ${TEMPLATE_DIR}:/template ${DOCKER_IMG} python3 -c "
import nibabel as nib
import numpy as np
from nibabel.orientations import axcodes2ornt, ornt_transform

orig = nib.load('/data/mprage_denoised_n4.nii')
orig_ornt = nib.io_orientation(orig.affine)
ras_ornt = axcodes2ornt(('R','A','S'))

mask_ras = nib.load('/data/mprage_RAS_pre_mask.nii.gz')
inv_xform = ornt_transform(ras_ornt, orig_ornt)
unet_mask = mask_ras.as_reoriented(inv_xform).get_fdata() > 0
tmpl_mask = nib.load('/data/template_mask_in_subject.nii.gz').get_fdata() > 0

refined_mask = np.logical_or(unet_mask, tmpl_mask).astype(np.float32)
mask_img = nib.Nifti1Image(refined_mask, orig.affine, orig.header)
nib.save(mask_img, '/data/mprage_refined_mask.nii.gz')

brain_data = orig.get_fdata() * refined_mask
brain_img = nib.Nifti1Image(brain_data.astype(np.float32), orig.affine, orig.header)
nib.save(brain_img, '/data/mprage_refined_brain.nii.gz')

added = np.sum(refined_mask.astype(bool) & ~unet_mask)
total = np.sum(refined_mask > 0)
print(f'U-Net mask voxels:     {np.sum(unet_mask)}')
print(f'Template mask voxels:  {np.sum(tmpl_mask)}')
print(f'Refined mask voxels:   {int(total)}')
print(f'Recovered voxels:      {int(added)} ({100*added/total:.1f}% of final mask)')
"
```

## Final: Uncompress refined brain

```bash
gunzip -k ${DATA_DIR}/mprage_refined_brain.nii.gz
```

**Outputs:** `mprage_refined_mask.nii.gz`, `mprage_refined_brain.nii.gz` (and `.nii`).
