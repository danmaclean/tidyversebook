# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Quarto book project titled "Wrangling data with 'tidyverse'" by Dan MacLean. The book teaches tidy data analysis principles using R's tidyverse packages (dplyr, tidyr, readr, ggplot2, tibble, and purrr).

## Project Structure

The book is organized as a Quarto project (`_quarto.yml`) with the following structure:
- **Main chapters**: Numbered `.qmd` files (01-tidy-data.qmd through 06-readr.qmd)
- **Index**: `index.qmd` (motivation and overview)
- **Appendices**: `prerequisites.qmd`, `r-fundamentals.qmd`, `acknowledgements.qmd`
- **Assets**: Images and data files in `assets/` directory (includes `diamonds.csv`, `datasets.xlsx`)
- **Output**: Rendered book goes to `docs/` directory (for GitHub Pages hosting)

The book has three main parts:
1. Background (tidy data concepts)
2. Working with tidy data in `dplyr` (verbs, joining, ggplot integration)
3. Making messy data tidy with `tidyr` (includes readr for data import)

## Building and Preview

**Render the entire book:**
```bash
quarto render
```

**Preview with live reload:**
```bash
quarto preview
```

**Render specific format:**
```bash
quarto render --to html
quarto render --to pdf
```

The output directory is `docs/` (configured in `_quarto.yml`).

## R Environment

This project uses `renv` for R package management:
- Activate renv: `source("renv/activate.R")` (automatic via `.Rprofile`)
- Restore packages: In R, run `renv::restore()`
- Update lockfile after adding packages: In R, run `renv::snapshot()`

Required R packages are specified in `renv.lock`.

## Key Files

- `_quarto.yml`: Quarto project configuration (book structure, chapters, output formats, theme)
- `_bookdown.yml`: Legacy bookdown configuration (likely not active)
- `.Rprofile`: Automatically activates renv environment
- `renv.lock`: R package dependencies lockfile

## Content Development

Each `.qmd` chapter follows this structure:
- Chapter title
- "About this chapter" section with Questions, Objectives, and Keypoints
- Content with R code chunks demonstrating tidyverse functions
- Embedded images using `knitr::include_graphics()`

The book uses:
- Pipe operator `%>%` syntax extensively
- Built-in datasets and sample data from `assets/`
- `nycflights13` package for exercises/quizzes

## Output Formats

Configured in `_quarto.yml`:
- **HTML**: Theme is "cosmo", includes search, Hypothesis comments, external chatbot link
- **PDF**: Uses scrreprt documentclass, available for download from HTML version