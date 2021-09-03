# auto_term_harm_pipe
This repository includes a set of Python-based Jupyter notebooks that comprise a semi-automated term harmonization pipeline applied to harmonize medical history terms across 28 clinical trials of pulminary arterial hypertension.  These notebooks pair with the paper 'A Semi-Automated Term Harmonization Pipeline Applied to Pulmonary Arterial Hypertension Clinical Trials'. Below, we offer an overview of these pipelines and provide guidance for users on how to adapt these notebooks to their own target harmonization tasks.

## Project Overview:
This set of Jupyter notebooks have been set up to facilitate the process of term harmonization and make it as replicatable and automated as possible. The ultimate goal is to take an existing set of term data from one or more sources that have been concatenated into a single dataset, and map the available terms to an ontological standard. This procedure is aimed at resolving one term data type at a time, e.g. medical history terms, or adverse event terms. The output of this procedure is copy of the original concatenated datasets that adds columns for the harmonized standards terms (at their most specific level), along with any levels of a generalized hierarchy of terms that may be useful for downstream data analysis. Further we add columns that track the quality of the mapping for each row and at each level of term specificity.

In these notebooks we lay out and document the aspects of this project that are automated (and thus fully replicatable) as well as describe and provide instructions for those aspects that required manual subjective decision making (thus are not completely replicatable). These notebooks have been set up to be as generalizable to other term harmonization tasks as possible, however throughout these notebooks we will focus (as our target application) the task outlined below.  As a result it is expected that for any outside party to utilize these notebooks for their own harmonization tasks, some modification/customization of these notebooks will be required. Thus it is best to view these notebooks as a guide/template for future term harmonization tasks, as well as a record for reproducing the harmonization tasks that we completed for our own target application. Additionally some of the computational tasks can take a number of hours to complete, therefore 'in-progress' mapping files in .csv and excel format are saved and loaded by this code after most steps.

In general the materials required to utilize this notebook as a guide is (1) a set of source terms as rows in a dataset (including supplemental term columns, and/or any available hierarchy of more general terms from a relevant ontology) and (2) the reference files of a target ontology standard, e.g. MedDRA.  While these notebooks are designed specifically to utilize the MedDRA v21 ontology of terms, they could be adapted to any ontology that provides as standard terminology, as well as link between specific and more general terms as part of the ontology hierarchy (e.g. GO terms/hierarchy).

## Target Application:
The specific 'term harmonization task' that we tackled in building these notebooks, is to harmonize terms used to describe 'medical history' events (MHTERM) for subjects over a set of 28 separate drug trials (CMREF Project). We will use the MedDRA v21 hierarchy of terms as our ontological/terminology standard. We assume that not all drug trials used the MedDRA standard, and those that did likely did not used the same version (v21). The primary goal is to harmonize the available specific 'medical history' term information, such that all possible terms are mapped to MedDRA low level terms (LLT). The secondary goal is to impute values for the more general levels of the MedDRA term hierarchy (i.e. preferred terms (PT), higher level term (HLT), higher level group term (HLGT), and system organ class (SOC)). In our application all trials have terms available at a specific, 'patient reported' level which we focus on in the primary LLT mapping. However in some trials, supplemental term data is available beyond the patient reported terms that we will utilize when available to provide a greater opportunity to map these term rows to the LLT MedDRA standard. 

## Data Availability:
While the target term data used in this study has not been made available here (for privacy and proprietary reasons) we have included the MedDRA ontology files that we formatted as Excel files. These included: LLT.xlsx, PT.xlsx, HLT.xlsx, HLT_PT.xlsx, HLGT.xlsx, HLGT_HLT.xlsx, SOC.xlsx, and SOC_HLGT.xlsx.

## Code Generalization:
In order to keep the code as generalizable (to other projects/applications) as possible we describe below the labels used in the code herein and how it corresponds to our specific target application described above:

*Format (General Code Labels) = (Application Specific Labels), i.e. column header names from our target dataset)*

Ontology Standard:
* (TL1) - Term Level 1 = (LLT) - lower level term
* (TL2) - Term Level 2 = (PT) - preferred term
* (TL3) - Term Level 3 = (HLT) - higher level term
* (TL4) - Term Level 4 = (HLGT) - higher level group term
* (TL5) - Term Level 5 = (SOC) - system organ class term

Target Dataset:
* (DL1_FT1) - Data Level 1 Focus Term 1  = (MHTERM) - Medical History Term (assumed available for every row)
* (DL1_FT2) - Data Level 1 Focus Term 2  = (LLT_NAME) - The primary alternative term.  In this case the original attempted mapping to LLT. (available for some rows)
* (DL1_FT3) - Data Level 1 Focus Term 3  = (MHMODIFY) - An alternative, 'modified' term available in the data for mapping to the lowest term level . (available for some rows)
* (DL2) - Data Level 2 = (PT_NAME) - MedDRA (unknown version) preferred term (available for some rows)
* (DL3) - Data Level 3 = (HLT_NAME) - MedDRA (unknown version) higher level term (available for some rows)
* (DL4) - Data Level 4 = (HLGT_NAME) - MedDRA (unknown version) higher level group term (available for some rows)
* (DL5) - Data Level 5 = (SOC_NAME) - MedDRA (unknown version) system organ class term (available for some rows)

## Summary of Notebooks (by step):
### Step 1 (Data Preparation):
This notebook is meant to cover initial data preparation.  This part of the harmonization process may include (1) loading the data, (2) summarizing the data for review, (3) cleaning the data (as needed), (4) formatting the data (as needed for running the remainder of the pipeline, (5) saving the cleaned, formatted file for the remainder of the harmonization pipeline. The target data is the concatenation of all available term instance rows over all studies to be harmonized.  This notebook also loads and provides summary information on the relevant MedDRA ontology files that will serve as our standard.

### Step 2 (Exact Matching):
This notebook is meant to perform exact matching, in order to identify the clear, 'top-quality' matches between our available most specific terms in the data, and the lowest level of our chosen standard terminology (in this case, LLT of the MedDRA v21).

### Step 3 (Fuzzy Matching):
This notebook is meant to perform fuzzy matching on all rows where an exact match could not be found for the term mapping. Here we again focus on finding matches between our available most specific terms in the data, and the lowest level of our chosen standard ontology (in this case, LLT of the MedDRA v21).

### Step 4 (Integrate Data and Conduct Quality Control on Manual Annotations):
This notebook is focused on organizing and formatting files for further downstream harmonization.

At this point Fuzzy Matching has been run, and in between notebooks 3 and this one, manual annotation has been performed using predefined instructions for all terms that were not 'exactly' matched.

To make this task more tractable, we first took the output file from 'Step 3' (i.e. MH_harmonization_map_5.csv) and manually created a newly formatted file (with exact matches removed) for downstream manual annotation (i.e. MH_harmonization_map_5_manual_formated_unmatched.csv). The set of over 7000 terms in that file (that failed to be exactly matched) were broken up in to 7 different files to spread out this manual work over several individuals (saved to '/Manual_Term_Assignments/'. The file 'LLT_Fuzzy_Matching_Instructions' specifies the instructions provided to each human participant in this manual annotation stage of the process. Generally speaking the task was to look at the results of fuzzy matching and attempt to pick the best MedDRA standard term identified (if any). An 'annotation quality code' of 4 is used if there is a relatively clear best term match that should not require any further follow up. A code of 5 is used if the ‘best’ chosen match is not clearly ideal, and there is reasonable uncertainty that this match is best. And lastly a code of 6 is used if no ‘best’ match could reasonably be identified from the available information.

This first manual annotation was completed by individuals that do not necessarily have expertise in medical history terminology, but whom had sufficient skill level to make reasonable determinations as to best term matches.

In this notebook (step 4) we check the individual (manually annotated) term files for consistency/validity, and reintegrate the datasets, into a single file. We summarize and report the counts of the matching quality scores. We also create a new file that includes only terms manually assigned a score of 5 or 6. This subset is passed on to an individual(s) with medical term expertise to go through and attempt to check terms previously assigned a score of 5, (and improve the confidence score to a 4 of deemed relevant), and attempt to assign terms with a score of 6 to some matching MedDRA term standard.

Later in this notebook we take this secondary manually scoring file and reintegrate it with the set of all terms, along with their MedDRA term matches and matching quality scores. We summarize and report the final term quality score counts following now that fuzzy matching and subsequent manual annotation have been completed, ideally minimizing the number of 6's (i.e. not matching MedDRA terms found).

### Step 5 (Impute Term Hierarchy - TL2 and TL3):
This notebook loads the working mapping file (following completion of LLT mapping (which we assume has been completed fully). Next it will perform the first two levels of term hierarchy imputation. In the current target application this involves imputing the PTs from LLTs, and then imputing the HLTs from the PTs. PT imputation is direct, since there are no alternative branches possible, so this task can be fully automated. However HLT imputation includes possible branching, therefore further manual, subjective annotation will be required after running this notebook. Whenever the data includes relevant term data, we will apply fuzzy matching to assist in the selection of the most appropriate of available HLT branches. When this relevant term data is not available we pick the first of the possible branches by default. Branch 'quality' is tracked much like term mapping quality.

### Step 6 (Impute Term Hierarchy - TL4):
This notebook loads the working mapping file (following completion of PT and HLT mapping (which we assume has been completed fully). Next it performs the next level of term hierarchy imputation. In the current target application this involves imputing the HLGTs from HLTs. However HLGT imputation includes possible branching, therefore further manual, subjective annotation will be required after running this notebook. Whenever the data includes relevant term data, we will apply fuzzy matching to assist in the selection of the most appropriate of available HGLT branches. When this relevant term data is not available we pick the first of the possible branches by default. Branch 'quality' is tracked much like term mapping quality.

### Step 7 (Impute Term Hierarchy - TL5):
This notebook loads the working mapping file (following completion of PT, HLT, and HLGT mapping (which we assume has been completed fully). Next it performs the next level of term hierarchy imputation. In the current target application this involves imputing the SOCs from HLGTs. However SOC imputation includes possible branching, therefore further manual, subjective annotation will be required after running this notebook. Whenever the data includes relevant term data, we will apply fuzzy matching to assist in the selection of the most appropriate of available SOC branches. When this relevant term data is not available we pick the first of the possible branches by default. Branch 'quality' is tracked much like term mapping quality.

### Step 8 (Finalize Harmonization and Generate Summary):
This notebook will take the working file which has added imputed SOCs to the rest of the mapping and will then map all these unique rows back to the original target dataset from the first notebook. This notebook will also perform a quality control check, summary, and final formatting of the mapping file for the target application.

## Dependencies:
We recommend users have Python 3 or higher, having installed the anaconda python package (e.g. https://www.datacamp.com/community/tutorials/installing-anaconda-windows). It may be necessary to install some additional packages that are used by one or more of these notebooks (e.g. fuzzywuzzy).  We recommend using the 'pip install' mechanism for easily installing these packages (e.g. https://packaging.python.org/tutorials/installing-packages/).

## Adapting Notebooks to New Harmonization Tasks:
Different term harmonization tasks can be quite variable in terms of the target data and output. Do to the heterogeneity inherent to one harmonization task vs. another this notebook needs to be adapted to new harmonization tasks. In these notebooks we sought to harmonize medical history terms using the MedDRA ontology as the terminology standard, specifically lowest level terms (LLTs). Additionally we sought to impute the more general term levels based on the same MedDRA standard.  Some harmonization tasks may only require the former, which includes notebook steps 1-4, and parts of step 8. The following considerations are needed to adapt these notebooks to a new term harmonization task.

1. Replace all filenames and unique column names (in the target data or terminology standard datasets) within the notebooks to match your target data as well as your selected terminology standard file.
2. Extend or remove code segments as needed to utilize any alternative term columns that may be in the target data (i.e. alternative entries of term information that might be useful in searching for a match within the term standard), or any columns in the target data that provide information that is useful for imputation of higher level (i.e. more general) terms after mapping the lowest level terms.
3. If (like in our target application) there is a need to impute higher level terms, adapt the code as needed to operate with however the target ontology standard formats connections between lower and higher level terms.  These notebooks are specific to working with MedDRA.
