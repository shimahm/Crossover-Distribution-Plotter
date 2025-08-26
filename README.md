# CO Distribution Plotter

This repository contains an R script that visualizes **crossover (CO) frequency distributions** along chromosomes from two ploidy levels (e.g., haploid `N` and diploid `2N`). The script bins CO positions into fixed windows and plots **raw CO counts per window** across each chromosome, with **centromere and pericentromere regions shaded** for context.

---

## Features

- **Input:** CO positions for two ploidy levels (e.g., `N` and `2N`)
- Counts crossovers in **fixed windows** (user-defined, e.g., 200 kb)
- **Plot output:**
  - **Y-axis:** Physical position (Mb), top = chromosome start
  - **X-axis:** CO counts per window
  - **Facets:** Chromosomes across columns, ploidy levels across rows
  - Highlights **pericentromere (light)** and **centromere (dark)** regions
- **Output:** High-quality PNG and PDF summary plots

---

## Input Files

1. **Crossover positions (`N_file`, `2N_file`):**
   - Tab-separated, at least 3 columns:
     ```
     chr    ploidylevel    position
     ```
     Example:
     ```
     C01    N     19172614
     C02    N     1468624
     C01    2N    21008381
     ```

2. **Chromosome sizes (`chrom_sizes.tsv`):**
   - Tab-separated, 2 columns:
     ```
     chrom    length_bp
     ```

3. **Centromere & pericentromere coordinates (`cen_peri.tsv`):**
   - Tab-separated, at least 5 columns:
     ```
     chr    cen_Start    cen_End    peri_Start    peri_End
     ```

---

## Usage

```bash
Rscript plot_co_windows.R \
  PT_chrom_distribuition_N.txt \
  PT_chrom_distribuition_2N.txt \
  Darmor_V10_sizes.txt \
  DarmorV10_cen_peri_start_end.txt \
  200000 \
  co_windows
```

**Arguments:**

- `N_file`: Crossover list for haploid plants
- `2N_file`: Crossover list for diploid plants
- `chrom_sizes.tsv`: Chromosome lengths
- `cen_peri.tsv`: Centromere and pericentromere coordinates
- `window_bp`: Window size in bp (e.g. 200000, 300000, 500000)
- `out_prefix`: Prefix for output files

**Output:**

- `co_windows.png` – summary plot
- `co_windows.pdf` – vector version

Each panel shows:
- CO counts per window (x-axis)
- Physical position in Mb (y-axis, reversed)
- Shaded centromere/pericentromere regions
- Chromosomes across columns
- Ploidy levels across rows

---

## Example Interpretation

Running with `200000` bp windows on *DarmorV10* produces:
- Higher CO density toward chromosome arms
- Suppressed recombination in centromeric/pericentromeric regions
- Side-by-side comparison of N vs 2N crossover distributions

---

## Dependencies

- R ≥ 4.0
- R packages:
  - dplyr
  - readr
  - tidyr
  - ggplot2
  - purrr
  - stringr

Install missing packages with:
```r
install.packages(c("dplyr","readr","tidyr","ggplot2","purrr","stringr"))
```

---

## What’s New / Improvements

- **Window builder**: Now uses a safe split + `imap_dfr()` pattern that never duplicates column names (fixes errors from `rowwise()`/`unnest()`).
- **Zero-filling**: Explicit (ploidy × window) grid ensures all windows are present, even when no COs occur.
- **Robust column detection**: Centromere/pericentromere columns are recognized regardless of header names.

---

## License

MIT License (feel free to reuse and adapt).

---

<!--
## Figure Preview

![Sample Output](co_windows.png)
-->


---

