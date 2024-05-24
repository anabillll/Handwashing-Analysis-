# Handwashing Analysis

This repository contains an R script for analyzing the impact of handwashing and autopsies on maternal mortality rates, based on historical data from Dr. Ignaz Semmelweis's research.

## Files

- `handwashing_analysis.R`: The main R script performing the analysis.

## Analysis

1. **Data Loading**: Loads clinic and hospital data from CSV files.
2. **Death Rate Calculation**: Adds a `death_rate` column to both datasets.
3. **Filtering**: Focuses on data from the Vienna hospital.
4. **Summary Statistics**: Calculates average death rates before and after the introduction of handwashing and autopsies.

## Usage

1. Save the `handwashing_analysis.R` script in your local repository.
2. Run the script in R or RStudio.

## Requirements

- R
- dplyr package

## How to Run

```r
# Load necessary library
library(dplyr)

# Load the data
clinic_data <- read.csv("datasets/clinic_data.csv")
hospital_data <- read.csv("datasets/hospital_data.csv")

# Add death_rate column to clinic_data and hospital_data
clinic_data <- clinic_data %>%
  mutate(death_rate = deaths / births)

hospital_data <- hospital_data %>%
  mutate(death_rate = deaths / births)

# Filter for Vienna hospital
hospital_data_vienna <- hospital_data %>%
  filter(hospital == "Vienna") %>%
  mutate(autopsies_introduced = year >= 1823)

# Calculate average death rates
rate_by_clinic_pre_handwashing <- clinic_data %>%
  filter(year < 1847) %>%
  group_by(clinic) %>%
  summarize(avg_rate = mean(death_rate), .groups = 'drop')

rate_by_autopsies_introduced <- hospital_data_vienna %>%
  group_by(autopsies_introduced) %>%
  summarize(avg_rate = mean(death_rate), .groups = 'drop')

# Print results
print(rate_by_clinic_pre_handwashing)
print(rate_by_autopsies_introduced)
