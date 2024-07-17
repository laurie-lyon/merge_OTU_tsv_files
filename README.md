# merge_OTU_tsv_files
Goal: merge taxonomy information output as .tsv files from different taxonomy classifiers into an Excel sheet, ultimately allowing comparison of concordant vs. discordant taxon classifications between different programs

# Initial file formats
  - We are starting with an Excel file with statistically significant hits from an experiment, listed as OTU i and OTU j (analysis involves interactions between bacterial pairs within a system)
  - Each OTU i and j has been assigned a taxonomy classifier (Taxon i and Taxon j, respectively) by greengenes 1 (?)
  - We have two .tsv files that include taxonomy information obtained by (1) SILVA and (2) greengenes 2

# Changes I made because I didn't know how to do this in R 
  - Change OTU i and OTU j in Excel sheet to Feature_ID_i and Feature_ID_j
  - Copy .tsv to .xlsx file
  - Change Feature.ID to Feature_ID_i and save as (name of .tsv file)_i
  - Made a copy and changed Feature_ID_i to Feature_ID_j and save as (name of .tsv file)_j

# Commands learned in this exercise
  - read in excel file
    ```
      df <- read_excel()
    ```
  - convert features in the df from doubles/strings to characters 
    ```
      df <- df %>%
      mutate(columnheader = as.character(columnheader))
    ```
  - merge dfs
    ```
      df_merged <- df %>%
      left_join(df_2, by = c("Feature_ID" = "Feature_ID")
    ```

# Ways to try to optimize
  - Write function to repeat greengenes2 actions with Silva

