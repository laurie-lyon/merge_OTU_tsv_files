# merge_OTU_tsv_files
Goal: merge taxonomy information output as .tsv files from different taxonomy classifiers into an Excel sheet, ultimately allowing comparison of concordant vs. discordant taxon classifications between different programs

# Initial file formats
  - We are starting with an Excel file with statistically significant hits from an experiment, listed as OTU i and OTU j (analysis involves interactions between bacterial pairs within a system)
  - Each OTU i and j has been assigned a taxonomy classifier (Taxon i and Taxon j, respectively) by greengenes 1 (?)
  - We have two .tsv files that include taxonomy information obtained by (1) SILVA and (2) greengenes 2

# Information needed to merge files
  - What value in the .tsv file can be matchede to either OTU i or OTU j within the Excel sheet?
