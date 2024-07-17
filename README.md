# merge_OTU_tsv_files
Goal: merge taxonomy information output as .tsv files from different taxonomy classifiers into an Excel sheet, ultimately allowing comparison of concordant vs. discordant taxon classifications between different programs

# Initial file formats
  - We are starting with an Excel file with statistically significant hits from an experiment, listed as OTU i and OTU j (analysis involves interactions between bacterial pairs within a system)
  - Each OTU i and j has been assigned a taxonomy classifier (Taxon i and Taxon j, respectively) by greengenes 1 (?)
  - We have two .tsv files that include taxonomy information obtained by (1) SILVA and (2) greengenes 2

# Information needed to merge files
  - What value in the .tsv file can be matchede to either OTU i or OTU j within the Excel sheet?

#' ---
#' title: "Merging Interaction Taxon Assignments greengenes1, greenegenes2, SILVA"
#' author: "Laurie Lyon"
#' date: "July 17th, 2024"
#' ---

# Step 1: Install necessary libraries
install.packages("readxl")
install.packages("dplyr")
install.packages("writexl")

# Step 2: Load the libraries
library(readxl)
library(dplyr)
library(writexl)

# Step 3: Read the Excel file
excel_file_path <- "./interactions_taxonomy_files_to_merge/taxa_interactions_GG1.xlsx"
df_excel <- read_excel(excel_file_path)

# Step 4: Read the Excel files for the greengenes2 taxa assignments
excel_file_gg2_i <- "./interactions_taxonomy_files_to_merge/rep_set_taxa_assignments_GG2_i.xlsx"
excel_file_gg2_j <- "./interactions_taxonomy_files_to_merge/rep_set_taxa_assignments_GG2_j.xlsx"

df_gg2_i <- read_excel(excel_file_gg2_i)
df_gg2_j <- read_excel(excel_file_gg2_j)


# Step 5: Convert Feature_ID_j in df_excel to character
df_excel <- df_excel %>%
  mutate(Feature_ID_i = as.character(Feature_ID_i))

## Step 5: Merge df_excel with df_gg2_i by the column Feature_ID_j
df_merged_gg2_i <- df_excel %>%
  left_join(df_gg2_i, by = c("Feature_ID_i" = "Feature_ID_i"))

# Print the result of the first merge
print("Column names in the merged GG2_i data frame:")
print(colnames(df_merged_gg2_i))

# Step 5: Convert Feature_ID_j in df_merged_gg2_i to character
df_merged_gg2_i <- df_merged_gg2_i %>%
  mutate(Feature_ID_j = as.character(Feature_ID_j))

# Step 6: Merge df_merged_gg2_j with df_gg2_i by the column Feature_ID_i
df_merged_gg2_ji <- df_merged_gg2_i %>%
  left_join(df_gg2_j, by = c("Feature_ID_j" = "Feature_ID_j"))

# Print the result of the second merge
print("Column names in the merged GG2_ji data frame:")
print(colnames(df_merged_gg2_ji))

# Step 7: Save the updated dataframe to a new Excel file
output_file_path <- "./interactions_taxonomy_files_to_merge/Supplemental_Table_1_GG1_GG2.xlsx"
write_xlsx(df_merged_gg2_ji, output_file_path)

# Repeat with Silva taxonomy after tsv --> xlsx and renaming columns
excel_file_Silva_i <- "./interactions_taxonomy_files_to_merge/rep_seq_taxa_assignments_Silva_i.xlsx"
excel_file_Silva_j <- "./interactions_taxonomy_files_to_merge/rep_seq_taxa_assignments_Silva_j.xlsx"

df_Silva_i <- read_excel(excel_file_Silva_i)
df_Silva_j <- read_excel(excel_file_Silva_j)

df_merged_gg2_ji_Silva_i <- df_merged_gg2_ji %>%
  left_join(df_Silva_i, by = c("Feature_ID_i" = "Feature_ID_i"))

df_merged_gg2_Silva <- df_merged_gg2_ji_Silva_i %>%
  left_join(df_Silva_j, by = c("Feature_ID_j" = "Feature_ID_j"))

output_file_path <- "./interactions_taxonomy_files_to_merge/Supplemental_Table_1_GG1_GG2_Silva_v4.xlsx"
write_xlsx(df_merged_gg2_Silva, output_file_path)
