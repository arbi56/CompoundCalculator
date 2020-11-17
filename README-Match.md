# Match
20201117 Notebook to match a list of calculated masses with a peak list

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://colab.research.google.com/github/arbi56/CompoundCalculator/blob/master/Match.ipynb)
## Background

Compares a list of masses and labels generated with CompoundCalculator to an input peak list. It is convenient to open the Calculator and Match notebooks in side-by-side windows (Jupyter Lab allows this) so it is easy to update the target ion list and repeat the matching.

## Approach

The program allows for the possibility that complex peak and target ion lists can result in multiple matches. The cell that shows the matches has a simplify mode which shows only one match for each peak but appends a string showing the number of matches, currently the match show is the one with the shortest (simplest) label - Occam's razor - but picking based on mass error could be anther option. There is also a cell that shows peaks with redundant matches to emphasize their existence and allow adjustment of the matching or ion generation parameters if necessary.

Unmatched peaks greater than a given intensity threshold (percent base peak intensity) are shown. The idea is that this is part of interactive spectrum interpretation, i.e. once the origin of unmatched peaks is understood the ion generation parameters can be adjusted and applied to other peaks.

In order to ensure that isotope peaks stay with the monoisotopic peaks, the program only searches for 13C peaks for matched peaks. Peaks identified as isotopes can also match entries in the target ion list so that other possibilities are shown.

The peak list must be tab-delimied and have mass values but can also contain columns for Retention Time (RT) and Intensity; the function that reads the peak list tries to determine which columns are present.

Results can be saved in several ways including a simple mass/intensity list and more detailsd lists. The former is useful with PeakView which allows tezt lists to be imported as spectra and overlaid on the original data to vislauize the matches and highlight unmatched peaks.

## Matching
Base compounds are provided as a list of (name, mass) tuples which specifies the main compound(s) but allows other components, specific modifications, such as the loss of C2H4 in Vinpocetin, and unknowns to be considered.


## Ion generation



## Summary
The compounds, parameters used and results are summarized in a cell that also save this information in a single line that can be written to the output file.


## Output
The Notebook can save masses less than an upper limit to a text file named according to the base name and the polarity; existing files will be overwritten. The file name used is based on the compound names, the polarity and whether the XIC format is used; it can optionally include the date and time the list was generated as a string YYMMDD_HHMMSS.


## Comments



