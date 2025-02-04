
<!-- README.md is generated from README.Rmd. Please edit that file -->

# susoflows

<!-- badges: start -->
<!-- badges: end -->

The goal of `susoflows` is to provide simple functions for common and
complex workflows with Survey Solutions. Rather than chain together
several `susoapi` commands, use one `susoflows` command instead.

## Installation

The package is not yet on CRAN, but can be installed via the following
command:

``` r
# install.packages("devtools")
devtools::install_github("arthur-shaw/susoflows")
```

## Usage

Consider downloading data. This seemingly simple process involves
several steps with `susoapi`:

``` r
# Step 1: Get the details for the questionnaire of interest
# First, First, get info on all questionnaires
qnrs <- susoapi::get_questionnaires()

# Then, filter to the questionnaire of interest and extract the questionnaire ID and version number.
qnr_id <- qnrs %>% 
  dplyr::filter(Title == "Your questionnaire title") %>%
  dplyr::pull(QuestionnaireIdentity)

# Step 2: Start an export job
job_id <- susoapi::start_export(
  qnr_id = qnr_id,
  export_type = "STATA"
)

# Step 3: Check on the status of the job
# is it yet?
susoapi::get_export_job_details(job_id = job_id)
# is it ready yet?
susoapi::get_export_job_details(job_id = job_id)
# ... is ready yet? Yes.
susoapi::get_export_job_details(job_id = job_id)

# Step 4: Download the export file once the export job is complete
susoapi::get_export_file(
  job_id = job_id, 
  path = "your/file/path/"
)

# Step 5: Repeat the steps above for each version of the questionnaire (if applicable).
# For example, imagine that a questionnaire has versions 1 and 2, and
# both versions were used to collect data that needs to be exported.
```

Consider an easier, one-step alternative with `susoflows`:

``` r
# Download data for all questionnaires that match characters in `matches`
# Example 1: data for all versions of a questionnaire
# Example 2: data for all questionnaires with "household" in the title
# note: `matches` is a regular expression
download_matching(
    matches = "Your questionnaire title", 
    export_type = "STATA",
    path = "your/file/path/"
)
```
