# The definition of the standard

As stated in the motivation, this standard aims to facilitate sharing the actual pieces
of code that analyze the data and produce the final results, hereafter called
'diagnostic scripts' or in short 'diagnostics' or 'scripts'. It is assumed that
those are able to be wrapped as standalone codes that can be run as independent
processes, an approach that has been successfully tested in different tools like
ESMValTool and CLIMAF.

## Glossary

There are three levels of definitions in this standard:

- **Must**: all diagnostic have to meet this conditions to comply with the standard.
- **Recommend**: diagnostics are encouraged to meet them but they are not mandatory. Nevertheless, if a list of compatible diagnostics is compiled, those who will meet them will be highlighted.
- **Suggest**: they will never be enforced, but they are considered nice to have. They mostly correspond to quality of life improvements.

In the same way, when defining the content of the yaml files, three different levels of definitions are used:

- **Required**: must always be present with the defined meaning and allowed values.
- **Reserved**: can be omitted, but if present, must have the defined meaning and allowed values.
- **Custom**: in all files, extra options can be added as needed by the diagnostics or the tools. The only requirement is that they do not collide with any `required` or `reserved` option.


## The command line interface

The diagnostic must be implemented as a command line tool that accepts a path to a yaml file as the only parameter.

It is recommended that the diagnostic executable can also be called with the `--help` option. The diagnostic should then print on the console the following help information:

- A small description of what the diagnostic does (recommended)
- A list of the options defined by the diagnostic to configure it. It is recommended that this list comes with a small explanation of its meaning and effects, the type of the expected values, and any default values used when the option is not explicitly defined (recommended).
- A list of the mandatory common options required for the diagnostic to work (recommended).
- A link to the online documentation, if available (suggested)

## The diagnostic configuration file

This section will define the syntax for the main configuration file. It must be a YAML file containing a mapping that contains a mapping with all the options needed by a diagnostic

### Required options

The following options must be present in the configuration file passed to the diagnostic.

- **diagnostic_path** : `str`. Path to the diagnostic executable. It may look redundant, but this way the file contains all the information required to run the diagnostic.
- **input_files**: `list`. List of absolute paths to the metadata files describing the data to be used by the diagnostic. The format of this files is defined later.
- **tool**: `str`. Name of the tool used to prepare the data for the diagnostic.
- **version**: `str` Version of the tool used to prepare the data for the diagnostic.
- **run_dir**: `str`. Path to the folder to use as current path for diagnostic execution. Must be writable by the diagnostic.
- **data_dir**: `str`. Path to the folder to store the  diagnostic's generated data on. Must be writable by the diagnostic.
- **plot_dir**: `str`. Path to the folder to store the  diagnostic's generated figures on. Must be writable by the diagnostic.

It is suggested to provide different folders for `run_dir`, `data_dir` and `plot_dir`, as it will be easier for the users to interact with the results, but the same folder can be used.

### Reserved options

The options listed in this file are not required for the diagnostics to implement, but if they do they must follow the following guidelines:

- **write_plots**: `boolean`, default value is `true`. A flag to control if the diagnostic generates the associated figures with the results or not.
- **write_data**: `boolean`, default value is `true`. A flag to control if the diagnostic generates the associated data files with the results or not.
- **log_level**: `str`, default values is `info`. Sets the granularity of the diagnostic logs and console output. If used, diagnostic must accept at least `error` `warning`, `info` and `debug`
- **auxiliary_data_dir** : `str`. Path to the auxiliary directory where the diagnostic may find further input data.
- **max_proc_number** : `integer`, default value is **1**. The maximum number of processors that the tool allows the diagnostic to request


## The data definition file

The data definition file must be a YAML file containing a mapping of mappings. The top-level mapping will have as keys the path to the data files and as values, the mapping containing the metadata of each file.

There is no limit to the number of input data files that can be provided on a single file, although it is suggested.

### Required options

- **alias**: `str`. Unique name for the dataset. Any (`alias`, `variable`) pair must be unique
- **filename**: `str`. Absolute path to the data file.
- **variable**: `str`. Variable name to be used by the diagnostic. It does not have to match any of the CF conventions names in the file

### Reserved options

#### Related to the dataset

- **project**: `str`. Name of the project to which this dataset belongs. It can be a real project, like `CMIP5`, `CMIP6` or a tool defined grouping of datasets like `Observations` or `Reanalysis`
- **activity**: `str`. CMIP activity or analog for the current data
- **institute**: `list(str)`. List of institutes associated to the current data.
- **dataset**: `str`. Name of the dataset. For example: the CMIP model identifier, the name of an observational dataset, etc.
- **ensemble**: `str`. Ensemble identifier for the current data

#### Related to the variable

- **table**: `str`. Original MIP table of the current variable
- **frequency**: `str`. Original frequency of the data used
- **modeling_realm**: `str`. Realms to which the variable belongs
- **grid**: `str`. Original grid of the variable
- **units**: `str`. Units of the data
- **short_name**: `str`. Variable name used in the file.
- **standard_name**: `str`. Standard name of the variable.
- **long_name**: `str`. Long name of the variable

#### Others

- **start**: `str`. Starting date of the data, in the format YYYYMMDD
- **end**: `str`, Ending date of the data, in the format YYYYMMDD
- **start_year**: `int`. Starting year of the data
- **end_year**: `int`. End year of the data
- **reference_dataset**: `str`. Dataset to be used as the reference to compare the current dataset with, given as the alias.

## The script formal description file

It is recommended that each script comes with a description file in YAML syntax which includes:
- **script_name**: `str`. A label for the name of the script, which uniquely identifies it, and is delivered by an authoritative entity (such as an ESGF, ENES, or WCRP entity)
- **script_interface_version**: `str`. A label for distinguishing among various version of the script interface
- **mandatory_keys**: `list(str)`. the list of reserved and custom keys for which the tool must provide values
- **input_type**: `str`. which can take values 'member', 'ensemble' and 'any', and which allows the tool to check that values provided to the script are of the right type. 'ensemble' means : an ensemble with more than one member, while 'any' allows for an ensemble of size 1. If the type differs among the variables, this entry can be a dictionary with key = variable name
- **outputs**:`dict`. A dictionary of patterns  which allows the tool to discover every output file and to assign a label to it. The patterns and the labels used as dictionary keys  can make use of keywords 'variable', 'dataset' and 'reference_dataset'; the tool may instantiate the keywords with all possible values to form file basenames and test whether a corresponding file exists in scripts output directories; dictionary values can be pairs, in which case, the second element of the pair allows for providing a pattern for indicating the output variable's short_name
- **can_select** : `bool`. Tells the tool whether the script is able to select data in input data files, in term of variable selection and time period selection. Defaults to **False**
