# CompoundCalculator Suite
20210626 Notebooks to implement Multi-Layered Analysis (MLA) of Complex Mass Spectra

Contains four Jupyter Notebooks that collectively provide tools for Multi-Layered Analysis (MLA), used to interpret Mass Spectra from one or more compounds with complex adduct patterns. The notebooks are written in Python 3 and can be used with a distribution such as Anaconda or Google’s ‘Colab’ (Collaborative laboratory) platform.

## MLA overview
MLA provides an alternative approach to the interpretation (annotation) of complex mass spectra, obtained from single compounds with complex adduct patterns or from mixed spectra (e.g. co-eluting compounds) with adducts. It can also be used to explore spectra from related compounds such as metabolites or degradation products.

The general approach is to successively generate a list of target masses, compare it to the input spectrum, analyze the matches and reprocess the residual spectrum with a different target list.
Target lists use only masses and labels and combinations are formed by adding masses and appending labels. A list of one or more compounds is extended by optionally modifying the compounds, e.g. generating hydroxy forms, and adding multimers and losses. Heterodimers (combinations of two different compounds) can also be calculated. Adducts are specified as a label and an “Effective Adduct Mass”, M<sub>A</sub>, which is equal to the mass of the adduct minus the mass of V hydrogen atoms, where V is the main valency of the adduct. For example, a sodium adduct is specified as (‘Na-H’, 21.98194) and calcium as (‘Ca-2H’, 37.94694). Combinations of adducts to a given number are calculated and each one is added to each compound to generate the final list. Protons are added to provide the final ion. 

Successive target lists are usually formed by adding compounds or adducts so that combinations with previous entities can be calculated, but any target list, such as a list of common contaminants, can also be used.
Matching is performed without prior deisotoping and uses a ‘many-to-many’ approach so one peak can match more than one target and vice versa. Potential isotopes are found once a peak is matched, so that matches and isotopes are kept together, and apparent isotopes that are in fact from other compounds can be detected. Matched and unmatched peaks are saved with the original intensities as both text and MGF files so they can be further processed or overlaid on the input spectrum for visualization.
MatchAnalyzer and Prospector are additional tools as summarized below.


## CompoundCalculator

[![Open Calculator In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://colab.research.google.com/github/arbi56/CompoundCalculator/blob/master/CompoundCalculator.ipynb)

Calculates target ion lists as described above and writes text files for use with Match. Lists can also be in a form that can be used with PeakView to generate Extracted Ion Chromatograms (EIC or XIC) by including a width value. Currently only singly charged ions are calculated but this will likely change in future versions.
## Match
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://colab.research.google.com/github/arbi56/CompoundCalculator/blob/master/Match.ipynb)

Compares a target ion list and an experimental peak list as described above. Peak lists are assumed to contain a mass value and intensity, but Retention Time can also be specified in a header line which the program parses using common terms. The output are text and MGF files for the matched and unmatched ions as well as a text results file. These results, including redundant matches to the same target or peak, can be examined inside the notebook.
Matching first looks for the masses as specified (12C) and then for 13C isotopes but future versions may look for actual patterns (if the elemental composition is known). 


## MatchAnalyzer
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://colab.research.google.com/github/arbi56/CompoundCalculator/blob/master/MatchAnalyzer.ipynb)

Reads a file of matched peaks, cleans it by removing duplicates and illustrates how to visualize the results using python packages (pandas, Seaborn, matplotlib). Particularly useful are heat maps that show how the number and form of adducts varies with number of monomers, and pivot tables which can also show the intensity and mass.
## Prospector
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://colab.research.google.com/github/arbi56/CompoundCalculator/blob/master/Prospector.ipynb)

Given a compound mass and ranges for charge and monomer count, calculates potential M<sub>A</sub> values for each peak in a list. The results are compared to a list of known M<sub>A</sub>  values to suggest possible adducts which can then be used with the Calculator (since Prospector doesn’t look for combinations of adducts).


