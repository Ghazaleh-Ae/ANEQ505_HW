ANEQ505
For Fridays
```
first command
sinteractive --reservation=aneq505 --time=01:00:00 --partition=amilan --nodes=1 --ntasks=2 --qos=normal
```
any other time reservation
```
ainteractive --
```
Make a new directory for the analysis
```
# make a directory using the mkdir command
mkdir decomp_tutorial  
  
# move into that directory using cd
cd decomp_tutorial
```
Import metadata and turn on Qiime2
```
# we first purge any loaded modules from the node we are on, this ensures no conflicting modules are "on"   
# modules are preloaded packages of things people commonly use  
  
module purge   
  
# we can turn "on" or "load" qiime2  
  
module load qiime2/2024.10_amplicon
```
Copy metadata from its source and move to our new folder and rename it
```
# make a directory for the metadata using the mkdir command while you are INSIDE of the decomp_tutorial directory 

mkdir metadata  
  
# move into that directory using cd

cd metadata

cp /pl/active/courses/2025_summer/CSU_2025/q2_workshop_final/QIIME2/metadata_q2_workshop.txt .

mv metadata_q2_workshop.txt metadata.txt
```
Visualize the metadata file
```
qiime metadata tabulate \  
--m-input-file metadata.txt \  
--o-visualization metadata.qzv
```

```
you can download FASTA file and change the format to txt to have access to all the sequences
```
