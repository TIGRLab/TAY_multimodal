# TAY_multimodal

This github project will house code related to Anisha Miah's summer project combining/comparing DWI NODDI and rsfMRI metrics from TAY data.

Starte June 2025

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
- understand fMRIPREP QC and rsfMRI QC metrics
 - example completed pipeline outputs `/scratch/galinejad/ScanD/SHARED_FOLDER_25/`
 - 
## fALFF/ALFF + reHO (will discuss with Colin)

 - literature: discuss wiht Colin Hawco (newest)
 - extraction - all done by XCP-D (which is stage-3.sh of TIGR_BIDS)
 -   learn how to understand output to find fALFF outputs
 -   learn how to concatenate across subjects from the output
 -   also grab coverage files to incorporate into QC
 -   understand/do visual qc of XCP and fmriprep to grab motion related QC metrics
 -   Decision with Colin: are we going to work from the GSR XCP-outputs - and take the reHo and alff values

## Global Corr
 - literture for it (prob from Anticivic lab) - https://elifesciences.org/articles/66968
 - additonal script ontop for XCP needed to calcualte it

## Principle Gradient
 - literature
    - Margiles paper, https://pmc.ncbi.nlm.nih.gov/articles/PMC5098630/
    - Ju-Chi most recent paper: https://pubmed.ncbi.nlm.nih.gov/39260567/
    - More updated review: https://www.sciencedirect.com/science/article/pii/S1053811922001161?via%3Dihub
 - additional calculation in python (using BrainSpace package) to calulate it - Ju-Chi has scripts

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
