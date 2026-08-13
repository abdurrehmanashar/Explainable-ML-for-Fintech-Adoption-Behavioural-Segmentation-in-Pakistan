# Data

The raw Global Findex Pakistan 2024 microdata is **not redistributed in this repository**, in line with the World Bank Microdata Library's terms of use for the dataset.

## How to obtain it

1. Go to https://microdata.worldbank.org/index.php/catalog/7961
2. Create a free World Bank Microdata Library account if you don't have one.
3. Click "Get Microdata" and download:
   - The dataset (CSV, Stata `.dta`, or SPSS `.sav` format)
   - The documentation PDF (the codebook — you'll need this to cross-reference variable names/labels)
4. Place the CSV in this `data/` folder. The scripts expect it at:
   ```
   data/Findex_Microdata_2025_Pakistan.csv
   ```
   (update the `CSV_PATH` variable at the top of `01_load_and_construct_target.py` if you name or place the file differently)

## Reference

World Bank. 2025. *Global Findex Database 2025, Pakistan.* Reference: `PAK_2024_FINDEX_v02_M`. World Bank Microdata Library.

## Note on survey weights

The dataset includes a `wgt` variable for correcting unequal sampling probability. This analysis does **not** apply survey weights — all reported percentages describe the n=1,000 analytical sample as collected, not weighted national population estimates. See the paper's Limitations section (Section VII) for the full discussion before using these figures for population-level claims.
