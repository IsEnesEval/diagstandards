# Definition of the standard

This document intends to define a standard
with the purpose of facilitating the sharing of source codes used to
analyze climate data and produce evaluation results and figures. 
Said codes will hereafter be referred as 'diagnostic scripts', or in short 'diagnostics'
or 'scripts'. 

It is assumed that diagnostic scripts are able to be wrapped
as standalone codes that run as independent processes, an approach that
has been successfully tested in tools such as ESMValTool and CLiMAF.
It also assumes that diagnostic scripts are able to read CF compliant NetCDF
data files and to produce output files with formats such as NetCDF, CVS, txt,
or excel spreadsheets; as well as graphic files.

## Glossary

This standard defines three tiers of enforceability for the guidelines to share diagnostics:

- **Must**: guidelines that all diagnostics have to meet to comply with the standard.
- **Recommend**: guidelines that all diagnostics are encouraged to meet, but are not mandatory.
Nevertheless, if a list of compatible diagnostics is compiled, diagnostic scripts meeting
recommended guidelines will be highlighted.
- **Suggest**: guidelines that are not required, but are considered to improve the quality of the diagnostics.

Diagnostic scripts are expected to be contributed along with a YAML file containing configuration options.
This standard defines three different levels of enforceability for the contents of this YAML file:

- **Required**: parameters that must always be present, have a defined meaning and that can only take allowed values.
- **Reserved**: parameters that can be omitted, but if present, must have a defined meaning and allowed values.
- **Custom**: extra parameters can be defined as needed by the diagnostics or the tools.
They must not collide with neither `required` nor `reserved` parameters.

## The diagnostic configuration file

The diagnostic's configuration file *must* consist of YAML file containing key-value mappings for all the parameters needed to run the diagnostic.

### Required options

The following options *must* be present in the configuration file:

- **diagnostic_path** : `str`. Path to the diagnostic executable.
- **input_files**: `list`. List of absolute paths to the metadata files describing the data to be used by the diagnostic.
The format of these files is especified in section [The data definition file](the-data-definition-file).
- **tool**: `str`. Name of the tool used to prepare the data for the diagnostic.
- **version**: `str` Version of the tool used to prepare the data for the diagnostic.
- **run_dir**: `str`. Path to the directory to use as current path for the diagnostic execution. *Must* be writable by the diagnostic.
- **data_dir**: `str`. Path to the directory to store the  diagnostic's generated data on. *Must* be writable by the diagnostic.
- **plot_dir**: `str`. Path to the directory to store the  diagnostic's generated figures on. *Must* be writable by the diagnostic.
- **interface_version**: `str`. Version of the diagnostic interface.

It is *suggested* to provide different directory names for `run_dir`, `data_dir` and `plot_dir`, as it makes the interaction with the results
easier. However, using the same directory for all purposes is accepted.

### Reserved options

The following options are not required, however if present they must follow the definitions listed below:

- **log_level**: `str`, default value is `info`. Sets the granularity of the diagnostic logs and console output.
If used, the logger must include `error` `warning`, `info` and `debug` levels .
- **auxiliary_data_dir** : `str`. Path to the auxiliary directory to store further input data files needed by the diagnostic, such as shapefiles.


## The data definition file

The data definition file *must* consist of a YAML file containing a mapping of mappings. The top-level mapping will have as keys the path to the data files. And as values, the mapping containing the metadata of each file.


### Required options

- **alias**: `str`. Unique name for the dataset. Each (`alias`, `variable`) pair *must* be unique.
- **filename**: `str`. Absolute path to the data file.
- **variable**: `str`. Variable name to be used in the diagnostic. It is not necessary to follow CF conventions naming for variables.

### Reserved options

#### Related to the dataset

- **project**: `str`. Name of the project to which a dataset belongs. It can be a real project, (e.g. `CMIP5`, `CMIP6`) or a tool-defined grouping of datasets
(e.g. `Observations` or `Reanalysis`).
- **activity**: `str`. CMIP activity, or analog, of a dataset.
- **institute**: `str`. Name of the institute producing a dataset.
- **dataset**: `str`. Name of the dataset. For example: the CMIP model identifier, the name of an observational dataset, etc.
- **experiment**: `str`. Label to identify the experiment a dataset belongs to. (e.g `historical`, `piControl`).
- **ensemble**: `str`. Ensemble identifier of a dataset.

#### Related to the variable

- **table**: `str`. Variable's original MIP table.
- **original_frequency**: `str`. Variable's original frequency.
- **frequency**: `str`. Variable's frequency when entering the diagnostic.
- **modeling_realm**: `str`. Realms a variable belongs to.
- **grid**: `str`. If present, label to the variable's grid given by the CMIP6 grid label.
- **units**: `str`. Variable's units.
- **short_name**: `str`. Variable's name used in the file as given by the CMIP data request.
- **standard_name**: `str`. Standard name of the variable as given by CF conventions.
- **long_name**: `str`. Long name of the variable as given by the CMIP data request.

#### Others

- **start**: `str`. Starting date of the data, given in ISO 8601 format for dates (YYYYMMDD).
- **end**: `str`, Ending date of the data, in ISO 8601 format for dates (YYYYMMDD).
- **start_year**: `int`. Start year of the data.
- **end_year**: `int`. End year of the data.
- **reference_dataset**: `str`. Alias of the dataset to be used as the reference for comparisons.

## The script formal description file

It is *recommended* that each script comes with a description file in YAML syntax which includes information regarding the documentation,
and configuration of the script.

The following labels should be provided in order to complete the documentation section:

- **title**: `str`. Label for the title of the diagnostic.
- **description** `str`. A short description of the diagnostic.
- **references** `list(str)`. List of references to be cited if using the diagnostic.
- **authors** `list(str)`. List of authors that developed the diagnostic.
- **read_more**: `url`. An URL to the online documentation.

The labels listed below should be provided in order to complete the configuration of the script:

- **script_name**: `str`. Name of the script, which uniquely identifies it.
- **script_interface_version**: `str`. Label to specify the version of the script interface.
- **mandatory_keys**: `list(str)`. List of reserved and custom keys for which the tool *must* provide values. A schema should be
provided in order to check the validity of the keys' values.
- **optional_keys**: `list(str)`. List of optional custom keys for the tool. 


## The command line interface

The diagnostic *must* be implemented as a command line tool that accepts the path to the
YAML configuration file as a parameter. 

It is *recommended* to include in the diagnostic executable the possibility to use a `--help` flag that prints the information defined in section [The script formal description file](#the-script-formal-description-file).

Additionally, it is possible to include additional flags to complement the execution of the diagnostics.
For example, a flag could be provided when re-running a diagnostic in order to indicate whether the output
directory should be emptied from previous runs or not.