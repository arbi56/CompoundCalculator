# CompoundCalculator
Notebook to calculate the masses of a compound and its modified forms (metabolite, adducts...)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]( https://github.com/arbi56/CompoundCalculator/blob/msster/notebooks/CompoundCalculator.ipynb)
## Background


The intent is that this be used with the PCVG code which finds patterns that are correlated across a series of samples. In some cases (e.g. the roadside test data)
these are mainly from a single sample and correspond to compounds resulting from a drug (therapeutic, recreational or drug of abuse), i.e. the parent compound and metaboltes. In other cases the target compound may be known, for example the vinpocetin data where vinpocetin was administerd to different rats.

Thus a PCVG group may contain many peaks - metabolites, multimers, adduct forms, etc. - that arise from a single compound and the challenge is to identify them all when some may not be previously known. The idea is that once we have some evidence of a compound, for example ibuprofen in one of the roadside test samples, we can quickly calcluate the potential masses of metaboltes and adducts, etc. and compare them to the masses in the PCVG group. (If desired we can group the PCVG masses by retention time first).

## Approach
CompoundCalculator is based on 'compositions' and 'limits'.
- **Compositions** provide a name and a mass (note these are **not** elemental compositions!). They are used to define the base compound and possible modifications (metabolites and adducts). E.g. ('Na-H', 21.981944) specifies a adduct formed by sodium replacing a proton
- **Limits** define the maximum number for a particular modification, e.g (OH, 2) indicates there can be 1 or 2 hydroxy metabolites

New entities are formed from others by adding the masses and appending the names. There is no attempt to validate the chemistry nor to remove redundant species (forms with the same mass).
Entities are treated as neutral and the charge is provided by adding or deleting the mass of a proton. This works because we specifiy the adducts as replacement forms, i.e. (Na-H), which generates the regular forms, since:

    M + (Na - H) + H+  => [M + Na]+

but also means that we can specify multiple adduct additions without calculating unique masses for the 'charge carriers', e.g.

    M + 2(Na - H) + H+  => [M + 2Na - 2H + H]+  => [M + 2Na - H]+ 

which would otherwise need an unique mass for 2Na-H. The approach also means that the same list of compounds and adducts can be used for both positive and negative modes, the only difference being whether the proton mass is added or subtracted.

The core modification names and masses are stored in the Composition class as a dictionary, e.g. {....;'Na-H':21.981944;....} and initailized in the cell that defines the class: 

    Composition.Mods = {...}.
The dictionary can be modified to add different adducts and/or modifications.

The calculator generates two lists:
- **adduct** forms are calculated by taking combinations of the allowed adduct forms up to a maximum number of modifications so it can calculate mixed forms such as [M + Na + K -H]+
- **compounds** are calculated by successively adding new forms generated by applying modifications to all of the compounds already in the list. This is performed in different steps as described below

## Compound generation
Compounds are generated in steps, each of which extends the list by adding a different form of modification to those compounds already in the list. The stages are:

1. **Base_mods** These are used to add forms of the base compound that are not due to 'normal' metabolism. For example, vinpocetin undergoes a loss of C2H4, possibly following oxidation, generating apovinpocetin which then undergoes phase 1 and 2 metabolism.
2. **Phase 1 mods** Generally these are oxidative products such as -OH, -(OH)2,... etc. and are applied to the main compound and forms generated with 'base mods'. The default list of modifications includes 'COOH' which corresponds to the conversion of -CH3 to -COOH that occurs in ibuprofen. Multiple modifications of the same kind are calculated but different kinds are not combined.
3. **Phase 2 mods** These are conjugates such as glucuronides and sulphates and are applied to the base compounds and their phase 1 metabolites. As with phase 1 metabolites, multiple modifications of the same kind are calculated but different kinds are not combined.
4. **Multimers and heterodimers** Multimers for each compound in the extended list are calculated up to a user specified limit, typically 2 or 3. If desired, the program can also calculate 'heterodimers' that are dimers formed by combining two different compounds rather than two of the same kind, i.e. A + B cf. 2A.

## Ion generation
The final list of masses is generated by combining each of the compounds with each of the adducts and either adding or subtracting a proton.

As shown in the Notebook, it is convenient to specify the polarity and use it to determine whether to add or subtract the proton mass, but also to specify the metabolites and limits. This is useful since some metabolites, espcially glucronides and sulphates, are generally more intense in negative mode.

# Output
The Notebook can save masses less than an upper limit to a text file named according to the base name and the polarity; existing files will be overwritten. A parameter ('write_locally') determines whether the file will be in the same location as the notebook or in a user-specified directory. The former is useful with Google's 'CoLab' since the file exists in a local, temporary wokspace from which it can be downloaded. The cell that implements the user-specified path  uses the python 'os' library to generate the output directory path based on a list of directory and sub-directory names in a platform-indpendent manner.

The output can be in two forms:
1. mass, name: a simple tab-separated list
2. mass, xic width, name: a tab-delimited file that is designed for use with the PeakView 'Extract Ion Chormatograms' commmand which can accept external lists in the format (via 'Import')

# Comments
Since compound and adduct generation are combinatorial, the final list of compounds can easilly be in the thousands.

The program does not calculate isotope masses although this would not be difficult to implement.

Since the program has no chemical intelligence, there is no attempt to choose between forms that have the same mass or the same elemental composition. When matching, however, the form with the shortest name is often the best choice.

The program does not yet calculate multiply charged forms, nor does it correctly handle divalent species such as Ca++ if there is more than one atom present.

There may be future versions to refactor the notebook and address these limitations



