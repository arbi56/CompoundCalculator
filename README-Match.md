# Match
20201117 Notebook to match a list of calculated masses with a peak list

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://colab.research.google.com/github/arbi56/CompoundCalculator/blob/master/Match.ipynb)
## Background
Compares a list of masses and labels generated with CompoundCalculator to an input peak list. It is convenient to open the Calculator and Match notebooks in side-by-side windows (Jupyter Lab allows this) so it is easy to update the target ion list and repeat the matching.

## Overview

The program matches the target ion and peak lists using a tolerance that can be specified in amu (fixed( or ppm (increases with mass). If both are non-zero the ppm is calulated and the larger window used which allows the window to increase with mass but never be less that a certain value. The program also allows for the possibility that complex peak and target ion lists can result in multiple matches for each peak. The function that prints the matches has a simplify mode which shows only one match for each peak but appends a string showing the number of matches; the match shown is the one with the smallest absolute error. Matched peaks can be grouped according to the 'root' field, which is part of the target ion list, and there is a cell that shows peaks with redundant matches to emphasize their existence and allow adjustment of the matching or ion generation parameters if necessary.

Unmatched peaks greater than a given intensity threshold (percent base peak intensity) are also displayed. The idea is that this is part of interactive spectrum interpretation, i.e. once the origin of unmatched peaks is understood the ion generation parameters can be adjusted and applied to other peaks.

In order to ensure that isotope peaks stay with the monoisotopic peaks, the program only searches for 13C peaks for matched peaks. Identified isotope peaks are Peaks identified as isotopes can also match entries in the target ion list so other possibilities are shown. In the output lists each peak has an index as well as the index of a related monoisotopic peak; these are the same for actual monoisotopic peaks.

The peak list must be tab-delimited and have mass values but can also contain columns for Retention Time (RT) and Intensity; the function that reads the peak list tries to determine which columns are present.

Results can be saved in several ways including a simple mass/intensity list and more detailed lists. Text lists can be imported into PeakView as spectra and overlaid on the original data to visualize the matches and highlight unmatched peaks.

## Step 1 - Setup

A cell defines a common storage area that is shared between the CompoundCalculator and Match so generated target ion lists are immediately available. The code checks to see if the Notebook is running in CoLab and, if so, uses GDrive.
**This cell should be modified for the User's environment**

Other cells allows parameters to be set and read the target ion list and the peak list. The peak list is tab-delimited and must contain the mass but the other fields (intensity, retention time) are optional and will be stored internally as zero if absent. If the file has only one column it is assumed to contain masses otherwise the code assumes that the first column is Mass, the second is Inten and the RT is absent. If the file contains a header line it is used to define the order of the columns by looking for matches with common labels e.g. mz, m/z, Mass, etc. for masses. The RT column can only be used via a header line containing 'RT' or 'rt'.

The target ion list is read 'as is' and the conditions line printed so it can be checked.

## Step 2 - Matching

Matching compare the target ion and peak lists looking for matches within a mass tolerance (window) which is defined in amu (fixed) or ppm (increases with mass). If both are non-zero the ppm window is always calculated and compared with the amu window; the larger value is used. This allows the window to increase with mass but never we less than the amu_window value.

In a second step, the program looks for 13C isotope peaks corresponding to matched peaks. A separate window is used for this, since it can usually be smaller, and potential isotope peaks must also be within an RT tolerance window (if RT is present) and, optionally, be less intense than the matched ion.


## Step 3 - Print, review, save

Cells illustrate various ways to report the matches:

- print all or some of them inside the notebook; there is an option to show all matches, including redundant ones, or to simplify the output to only show the shortest
- print the matches grouped by the 'Root' field from the target ion list and optionally suppress isotopes
- count the number of peaks that have redundant matches and optionally print them
- print the unmatched peaks above a threshold (as a percentage of the base peak intensity)

Reviewing redundant peaks is useful as it can indicate that parameters need changing (if there are too many), for example: reduce the matching tolerance, reduce the number of target ions, etc.

The unmatched peak list is a good way to find peaks that still need to be explained and is the first step in further interpretation.

The detailed list of matches can be written to a file for use with the Interpret module or elsewhere and a final cell summarizes the parameters and results.

A cell summarizes the results and generates a single string that is written to the output file.

## Output

The Notebook can save matches to a tab-delimited file with a name corresponding to the input peak list with 'Matches' appended and optionally the date and time of the match. The first column is mass and the second intensity so the file can be opened in spectrum display software, for example PeakView, and overlaid on the spectrum used to generate the initial peak list. This provides a nice way to visualize the matches and identify unmatched peaks for further consideration.

By default, the output contains all match details but this can be changed bt setting *with_detail=False* in the line that saves the file.




