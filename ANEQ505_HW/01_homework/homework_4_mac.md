Set up correct directory structure~={red}(1point)=~
Correct file path for loading in metadata~={red}(1 point)=~
Correct file path for loading in alpha diversity data ~={red}(1 point)=~
Correct file path for loading in beta diversity data ~={red}(1 point)=~
Correct file path for loading in tabulate_results.tsv (taxabarplot) ~={red}(1 point)=~
Filter table for ANCOM-BC2~={red} (~={red}1 point=~)=~
Filtering out low abundance/low prevalence ASVs ~={red}(~={red}1 point=~)=~
Collapse to species level ~={red}(~={red}1 point=~)=~
Running ANCOM-BC2 ~={red}(~={red}1 point=~)=~
Visualize ANCOM-BC outputs ~={red}(1 point)=~
Questions ~={red} (5 points)=~ 

~={red}15 points total=~
------------------------------------------------------------------

Due: 04/09/2026

**For complete credit for this assignment, you must answer all questions and include all commands in your Obsidian upload.** 

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Create publication-ready figures for alpha and beta diversity.
3. Understand how to run ANCOM-BC2 and how to interpret the results. 
--------------------------------------------------

#### Cow Body Site - making figures in R

**Set up the cow R analysis file structure**
- Make a cow_r directory on your local computer, and inside the cow_r directory, make the following directories 
cow_r  
├── 01_notes  
├── 02_data  
├── 03_metadata  
├── 04_code  
└── 05_figures

- Inside the 04_code directory, make the following directories
04_code  
├── alpha_div 
├── beta_div 
├── taxonomy

- Download the cow_metadata.txt, shannon.tsv, unweighted_unifrac.txt, tabulated_results.tsv, and cow_HW4_r.Rmd files from Canvas and put them in the correct directories. 

**What directory should the cow_HW4_r.Rmd file go in? ~={red}(1 point)=~**

- *Write the directory here:* cow_r/04_code
#### Statistical analysis and figure generation in R 

- Now that we have set up the correct file structure and put our files in the correct directories, we can start our cow R analysis. 
- Open the cow_HW4_r.Rmd file and start working through the analysis.

**Note that if you open the markdown file in your Downloads, the working directory will not be correct. Make sure to only open the markdown file after you have put it in the correct working directory.**

**Read in metadata ~={red}(1 point)=~**
- Fill in the file path you used in the R Markdown to load the metadata. 
```
metadata <- read_tsv("../03_metadata/cow_metadata.txt")
```

**Read in alpha diversity data ~={red}(1 point)=~**
- Fill in the file path you used in the R Markdown to load the shannon data
```
shannon <- read_tsv("../04_code/alpha_div/shannon.tsv")
```

**Read in beta diversity data ~={red}(1 point)=~**
- Fill in the file path you used in the R Markdown to load the unweighted unifrac data
```
uw_unifrac <- read_tsv("../04_code/beta_div/unweighted_unifrac.txt")
```

**Load in tabulated results ~={red}(1 point)=~**
- Fill in the file path you used in the R Markdown to load the tabulated_results.tsv
```
tabulated_results <- read_tsv("../04_code/taxonomy/tabulated_results.tsv")
```

#### Cow Body Site - ANCOM-BC2 in Qiime2
**Start an interactive session and activate Qiime2**
```
ainteractive --ntasks=4 --time=04:00:00
```

- **ANCOMBC2 is only available in the 2026 versions of qiime2, so we need to activate the latest version. Make sure to activate qiime2026**
```
module purge
module load qiime2/2026.1_amplicon
```
(When running commands using qiime2/2026.1_amplicon you might get this warning: */curc/sw/install/bio/qiime2/2026.1/2026.1_amplicon_env/lib/python3.10/site-packages/unifrac/__init__.py:9: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81. import pkg_resources*. This is just saying that one of the qiime2 packages needs to be updated it won't affect the qiime2 outputs.)


**Filter controls out of our table
```
# Get matadata with no controls
cp /pl/active/courses/2025_summer/CSU_2025/cow_hw/cow_metadata_nocontrols.txt .

qiime feature-table filter-samples \
  --i-table ../dada2/table_nomitochloro_gg2_filtered300.qza \
  --m-metadata-file cow_metadata_nocontrols.txt \
  --o-filtered-table table_nomitochlorocontrols_gg2_filtered300.qza
```

**Filter Samples ~={red}(1 point)=~** 
- Navigate into the cow tutorial and make a new ancombc2 directory for the ANCOM-BC2 analysis
- Navigate into the ancombc2 directory
- Choose the min frequency for sample filtering:
```
qiime feature-table filter-samples \
--i-table table_nomitochlorocontrols_gg2_filtered300.qza \
--p-min-frequency 5000 \
--o-filtered-table table_5k.qza
```

**Filter out low abundance and low prevalence ASVs ~={red}(1 point)=~**

```
qiime feature-table filter-features \
--i-table table_5k.qza \
--p-min-frequency 50 \
--p-min-samples 20 \
--o-filtered-table table_5k_abund.qza
```

**Collapse features to genus level ~={red}(1 point)=~**
- We will collapse to the genus level to make it easier to interpret the results. (Hint: We used 7 for species, so think about which number you would use for genus.)

```
qiime taxa collapse \
--i-table table_5k_abund.qza \
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \
--p-level 6 \
--o-collapsed-table table_5k_abund_L6.qza
```


**Run ANCOM-BC2 ~={red}(1 point)=~**

```
qiime composition ancombc2 \
--i-table table_5k_abund_L6.qza \
--m-metadata-file cow_metadata_nocontrols.txt \
--p-fixed-effects-formula body_site \
--o-ancombc2-output ancombc2_results_bodysite_genus.qza
```


**Visualize the ANCOM-BC2 results ~={red}(1 point)=~**
- Generate a barplot to visualize the differentially abundant features. 
```
qiime composition tabulate \
--i-data ancombc2_results_bodysite_genus.qza \
--o-visualization ancombc2_bodysite_genus.qzv
  
qiime composition ancombc2-visualizer \
  --i-data ancombc2_results_bodysite_genus.qza \
  --o-visualization ancombc2_barplot_bodysite_genus.qzv
```

## Homework questions: (~={red}5 POINTS=~)
1. Describe one way to get data from your qiime2 outputs into a format that can be used for R. 
   One efficient way to do this is by using the qiime tools export command in the terminal. This allows us to take a QIIME 2 artifact (like our ANCOM-BC results or feature tables) and extract the raw data into a .tsv file. Once exported, these files can be easily read into R using standard functions like read_tsv () for statistical analysis and custom plotting.

2. Which body site appeared most distinct in the taxa bar plot, meaning it was not similar to at least one of the other body sites? Explain why that site looks different.
   The fecal samples appeared most distinct in the taxa bar plot. This is biologically expected because the fecal microbiome is composed of anaerobic bacteria that have passed through the complex environment of the digestive tract. These communities are vastly different from the other body sites (skin, nasal, oral, and udder), which are external or exposed to higher oxygen levels, leading to a completely different microbial fingerprint.

3. When generating the filtered table for ANCOM-BC2, what value did you choose for `--p-min-frequency`? Which core metrics parameter should this match, and why do these values need to be the same? (Report your core metrics value here: 5000)
   I chose a value of 5000 for `--p-min-frequency`. This matches the `--p-sampling-depth` we used earlier in the core diversity metrics step. It's important that these values match so we remain consistent across our entire analysis. By using the same threshold, we ensure that the same set of high quality samples is being used for both diversity calculations and differential abundance testing, which prevents low depth noise from biasing our results.

4. Why do we filter out samples with low frequency and low abundance ASVs?
   We filter these out primarily to reduce noise and minimize biological bias. Low Frequency (Sampling Depth): If a sample has very few total sequences, it doesn't provide a fair representation of the community. Low Abundance ASVs: ASVs that only appear a couple of times across entire dataset are often the result of sequencing errors, PCR artifacts, or minor contamination. If we leave them in, they can artificially inflate our diversity scores and make it harder for statistical tools like ANCOM-BC2 to find real, significant patterns. By removing them, we ensure our fingerprint for each body site is built on high quality, reliable data.

5. What was the most enriched genus in skin compared to fecal, and what was the most depleted genus in skin compared to fecal (make sure adjusted p is set to less than 0.05)?
   Based on the ANCOM-BC2 barplot, the most enriched genus in skin (relative to fecal) is Staphylococcus. This makes sense as it's a primary inhabitant of the skin's dry environment. The most depleted genus in skin compared to fecal is Bacteroides. These are obligate anaerobes that thrive in the gut but are essentially "depleted" or absent on the oxygen-rich surface of the skin.
	

## Extra credit~={orange} (3 points)=~ generate a classification model to see how well we can predict cow body site

```
cd /scratch/alpine/$USER/cow/
mkdir ml   
cd ml

#remove controls
qiime feature-table filter-samples \
--i-table ../core_metrics_results/rarefied_table.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--p-where "[body_site] != 'control'" \
--o-filtered-table rarefied_table_no_controls.qza

qiime taxa collapse \
--i-table rarefied_table_no_controls.qza \
--i-taxonomy ../taxonomy/taxonomy_gg2.qza \
--p-level 7 \
--o-collapsed-table rarefied_table_no_controls_L7.qza
```

```
qiime sample-classifier classify-samples \
--i-table rarefied_table_no_controls_L7.qza \
--m-metadata-file ../metadata/cow_metadata_nocontrols.txt \
--m-metadata-column bodysite \
--p-random-state 123 \
--p-n-jobs 1 \
--output-dir sample_classifier_results_bodysite
```

### **Questions:**
1. Why might removing controls be important before downstream analysis? 
   Removing controls (like extraction blanks or mock communities) is essential because these samples don't represent actual cow biology. If left in, the Machine Learning model might learn to identify a body site based on lab contaminants or artificial patterns rather than real biological signals. This would make the model less accurate when trying to predict real world samples.
   
2. what 2 features that are high in fecal samples? 
   In the feature importance plot, the species Cryptobacteroides sp902787255 and Faecousia sp000434635 stand out as the top predictors for fecal samples. These specific gut-associated taxa are highly unique to the fecal microbiome, making them excellent 'fingerprints' for the model to identify that site.
   
3. what are 2 features that are low in nasal?
   Looking at the classifier results, the genera Parabacteroides_B_862066 and Ruoffia appear at the bottom of the importance list for nasal samples. Their relative absence or low frequency in the nasal cavity helps the machine learning model distinguish those samples from other body sites where these bacteria might be more common.
   
4. what is the accuracy of your model, and if the accuracy of the classifier is high, what does that suggest about the microbial compositions of each site?
   The classification model achieved a baseline accuracy of 88%. This high accuracy suggests that the microbial communities at each body site, fecal, nasal, oral, etc. are distinct enough that a computer can correctly identify the site just by looking at the bacteria present. It confirms that the 'microbial fingerprint' of each site is unique and consistent across the cows in this study.