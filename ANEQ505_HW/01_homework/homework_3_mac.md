~={red}(1point)=~ for Alpha Rarefaction Plot
Run Core Metrics ~={red}(1 point; .25pts per line)=~
Make alpha diversity plots ~={red}(3points)=~
~={red}10 points=~ for the questions 

~={red}15 points total=~
------------------------------------------------------------------

Due: 

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.**

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Perform alpha rarefaction and determine an appropriate sequencing depth.
3. Run core metrics, generate plots for alpha and beta diversity
--------------------------------------------------

**Cow Site Data Workflow**, part 3

Load qiime2 in a terminal session after you go into the **cow** folder

```
# Insert the two commands to activate qiime2

module purge
module load qiime2/2024.10_amplicon

```

### Alpha Rarefaction Plot ~={red}(1 point)=~
- Chose the input sequencings depths (min and max) for generating the alpha rarefaction plot: 

```
#go to the cow directory

qiime diversity alpha-rarefaction \
--i-table dada2/cow_table_dada2_filtered300.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization alpha_rarefaction_curves_16S.qzv \
--p-min-depth 3 \
--p-max-depth 10000
```


### Run Core Metrics ~={red}(1 point)=~

```
qiime diversity core-metrics-phylogenetic \
--i-table dada2/cow_table_dada2_filtered300.qza \
--i-phylogeny tree/tree_gg2.qza \
--m-metadata-file metadata/cow_metadata.txt \
--p-sampling-depth 1000 \
--output-dir core_metrics_results
```


### Visualize alpha diversity plots
- generate a plot to visualize the observed features ~={red}(1 point)=~
```
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/observed_features_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/observed_features_vector.qzv
```

- generate a plot to visualize faith's PD ~={red}(2 points)=~
```
## insert the entire code chunk for generating this visualization 

qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/faith_pd_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/faith_pd_vector.qzv

```



## Homework questions ~={red}(10 points)=~

1. what is the name of the file you needed to use to figure out what min and max depths to use to generate the alpha rarefaction plot? (Hint: which file contains the sequencing depths for each sample)
cow_table_dada2_filtered300.qzv

2. what did you choose for the rarefaction depth (the input for core metrics -p-sampling-depth flag)? why? 
I chose 1000 for the rarefaction depth because looking at the alpha rarefaction curve, the Shannon diversity metric plateaus around 1000 sequencing depth. Beyond 1000, adding more sequences doesn't significantly increase observed diversity, so 1000 is the optimal rarefaction depth.

3. Which cow body location had more observed features? Which has the lowest?
Based on the observed features plot, fecal samples had the highest number of observed features, followed very closely by skin samples (both around 270 features). Udder samples had fewer features at around 210, followed by oral samples at about 85 features. Nasal samples had the lowest number of observed features among the actual cow body sites, with only around 15-20 features. The positive control samples had even fewer features (around 10-15), but those aren't real cow samples so they don't really count for comparison. So overall, skin had the highest diversity and nasal had the lowest diversity in terms of observed features.

4. What is the main difference between Faiths PD and Shannons alpha diversity metrics?
Faith's PD is a phylogenetic alpha diversity metric that takes into account the evolutionary relationships between different taxa in a community. Shannon diversity, on the other hand, is a non-phylogenetic metric that only considers the abundance and richness of species without accounting for how closely related they are evolutionarily. In other words, Faith's PD incorporates the phylogenetic tree structure into its calculation, while Shannon diversity does not.

5. Which diversity metrics produced by the core-metrics pipeline require phylogenetic information?
Faith's PD, Unweighted UniFrac, and Weighted UniFrac require phylogenetic information.

6. Which two body sites have the highest Faiths PD alpha diversity?  Are the groups significantly different?
The two body sites with the highest Faith's PD alpha diversity are skin and fecal samples. Skin samples have the highest median Faith's PD values (around 53-55), while fecal samples have the second highest (around 43-45).
   Yes, the groups are significantly different from each other. The overall Kruskal-Wallis test across all body sites shows a highly significant p-value (p < 0.001), indicating that Faith's PD values differ significantly among all body site groups. Additionally, the pairwise comparison between skin and fecal shows they are significantly different from each other (p = 0.0001969, p < 0.05).
   
7. Does it seem like there are any groupings in the beta diversity? What are the groupings? 
8. Why do you think these samples are grouping together? 
9. What test can you run to determine if the groups are significantly different?
10. What command would you use to run that test?

```
#insert command for running the test you suggest from question 7



```