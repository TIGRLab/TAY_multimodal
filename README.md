# TAY_multimodal

fMRI Date Processed: 06-01-25 to 06-11-25
NODDI Date Processed: 06-09-25 to 07-12-25

Dataset:
 - StudyInfo: combining/comparing DWI NODDI and rsfMRI metrics from TAY data, as well as DWI NODDI and age.
 - Dataset: TAY CAMH Cohort Study, 54 participants that completed the [18F]SynVesT-1 PET substudy (aged 16-24)
   - Outputs were derived from TIGRLab/SCanD_Project v2.0.0
       - NODDI outputs used QSIPREP for preprocessing and AMICO NODDI model to derive NDI and ODI values
       - fMRI outputs used fMRIprep fit for preprocessing and used xcp_d outputs

Contact:
 - Principal Investigator: Erin Dickie
 - Primary: Anisha Miah - anisha.miah@utoronto.ca / anishamiah09@gmail.com
 - Other: Christin Schifani

System:
 - Dependencies:
    library(ggcorplot)
    library(ggseg)
    library(ggplot2)
    library(ggridges)
    library(ggsegGlasser)
    library(ppcor)
 - RanFrom: SciNet

Commands/Scripts:
  - Scripts located in 01dwi_wrangling.Rmd
  - Atlases used: HCP MMP (Glasser) and HCP subcortical (aseg)
  - Paths:
     - fMRI outputs: path_xcpd = "/scratch/amiah/SCanD_project/data/local/derivatives/xcp_d/0.7.3/"
     - DWI NODDI outputs: path_noddi = "/projects/jwong/qsiprep/output/noddi_extract/ciftify_parcellations/"
     - Age at time of MRI: age_df = "/scratch/jwong/tay_pet_age.csv"
  - Functions made:
    - wrangle_data(path, pattern) concatenates subject files into one dataframe
    - calculate_GC(dataframe, string) calcualtes postitive, absolute and mean global signal correlation and can be customized for individual or group dataframe based on string input
    - matrix_QC(dataframe, run_num, title_string) creates a matrix between subject to subject for global signal correlation between atlases
  - First section (lines 39-220) involves concatenating all age and fMRI metrics. Also includes brainmaps of ten random subjects global signal correlation maps, alonside group brain maps
  - lines 222-284: looks at quality controlling global signal correlation by looking at subject to subject matrices, as well as graphing timeseries of each ROI of some random number of patients
  - lines 285-315: concatenating all NODDI outputs between subjects
  - lines 318-344: creating full dataframes of all required metrics (rsfMRI metrics, NODDI metrics, age) of each atlas
  - lines 364-561: group brainmaps of each metric. Some brainmaps use the matlab colours, while others use viridis_c to visualize
  - lines 565-829: looks at age-NODDI correlations and maps it onto a brainmap as well as density plots
  - lines 833-1320: looks at NODDI-rsfMRI correlations and maps it onto a brainmap as well as density plots
  - lines 1322-1902: looks at NODDI-rsfMRI correlations with age regressed out and brainmaps it

Quality Control:
 - Primary: Anisha Miah 
   - one subject removed from NODDI outputs due to misalignment of NDI/ODDI map with parcellations

Data Requests/Publications: N/A

For each metric, we need to:

1. Read literature and discuss to understand the metric
2. build/clean up scripts ot extract the metric
3. help design QA process and QC the metric related pipelines.

# Another concept is parcallations

 - read lit to understand what they are
 - understand how they are represented in BIDS derivatives outputs

Diffusion Metrics (NODDI - NDI, ODI, free water)
- reading list from Christin
- extracted pipeline outputs - `/archive/data/TAY/pipelines/versioned_outputs/pet_subsample/`
- need to understand these outputs and concanate group outputs by atlas
- qc files exists but QC process not set
-   qc should go through qsiprep both ways:
-    https://imaging-genetics.camh.ca/documentation/#/resources/Qsiprep-QC-guide
-    add also inspect the .html pages in the qsiprep folder that describe the workflow

# rsfMRI metrics

- Literature: History of rsfMRI - good overview to start
- XCP: https://pmc.ncbi.nlm.nih.gov/articles/PMC10690221/
- understand fMRIPREP QC and rsfMRI QC metrics
 - example completed pipeline outputs `/scratch/galinejad/ScanD/SHARED_FOLDER_25/`
 - 
## fALFF/ALFF + reHO (will discuss with Colin)

 - literature: (https://pmc.ncbi.nlm.nih.gov/articles/PMC11245004/#S2)
 - extraction - all done by XCP-D (which is stage-3.sh of TIGR_BIDS)
 -   learn how to understand output to find fALFF outputs
 -   learn how to concatenate across subjects from the output
 -   also grab coverage files to incorporate into QC
 -   understand/do visual qc of XCP and fmriprep to grab motion related QC metrics
 -   Decision with Colin: are we going to work from the GSR XCP-outputs - and take the reHo and alff values

## Global Corr
 - literture for it (prob from Anticivic lab) - https://elifesciences.org/articles/66968
 - additonal script ontop for XCP needed to calcualte it
 -  to calculate - grad correlation matrix
 -   Total GC = mean of row
 -   Positive GC = mean of all positive values in the row
 -   Absolute GC = mean of absolute values in the row

## Principle Gradient
 - literature
    - Margiles paper, https://pmc.ncbi.nlm.nih.gov/articles/PMC5098630/
    - Ju-Chi most recent paper: https://pubmed.ncbi.nlm.nih.gov/39260567/
    - More updated review: https://www.sciencedirect.com/science/article/pii/S1053811922001161?via%3Dihub
 - additional calculation in python (using BrainSpace package) to calulate it - Ju-Chi has scripts

 - To get new templates (on a lab computer - done by erin):
 - ``` module load connectome_workbench; wb_command -cifti-parcellate /scratch/edickie/parcellate_gradients/hcp.embed.dscalar.nii /scratch/amiah/SCanD_project/data/local/derivatives/xcp_d/0.7.3/atlases/atlas-Glasser/atlas-Glasser_space-fsLR_den-91k_dseg.dlabel.nii COLUMN /scratch/edickie/parcellate_gradients/atlas-Glasser_space-fsLR_den-91k_desc-Margulies2016_gradients.pscalar.nii"```
 - getting a jupyter env on scinet (in the scinet terminal)

```sh
module load python/
cd $SCRATCH
virtualenv --system-site-packages brainspace_2025
source brainspace_2025/bin/activate
pip install brainspace
venv2jup   
```

(then open up jupyter on demand and you should see "brainspace_2025" as an option

### notes on gradients from meeting with Ju-Chi
 - Ju-Chi's gradient calculating code from paper: `/scratch/jcyu/spin_gradients/code/3.3_spins_gradiant.py`
   - inside this script - the `calc_aligned_gradient()` function is the most important  
 - need to re-parcellate from margiles file to get templates: `/scratch/jcyu/spin_gradients/hcp.embed.dscalar.nii`


   # July goals

    - Compare the NODDI to the rsfMRI metrics using stats
    - pulling all the metrics into one dataframe?
    - correlate across the brain within subject
    - correlate across subject wihtin parcels
    - make pretty ggseg pictures
    - 
