# The definition of the standard

## Glossary

There are three levels of definitions in this standard:

- **Must**: all diagnostic have to meet this conditions to comply with the standard.
- **Recommend**: diagnostics are encouraged to meet them but they are not mandatory. Nevertheless, if a list of compatible diagnostics is compiled, those who will meet them will be highlighted.
- **Suggest**: they will never ve enforced, but the team consider them as nice to have. They mostly correspond to quality of life improvements.

In the same way, when defining the content of the yaml files, three different levels of definitions are  used:

- **Required**: they must allways be present with the defined meaning and allowed values.
- **Reserved**: they can be omitted, but in the case that are present, they must have the defined meaning and allowed values.
- **Custom**: in all files, extra options can be added as needed by the diagnostics or the tools. The only requirement is that they do not collide with any `required` or `reserved` option.


## The command line interface

The diagnostic must be implemented as a command line tool that accepts a path to a yaml file as the only parameter.

It is recommended that the diagnostic executable can also be called with the `--help` option. The diagnostic should then print on the console the following help information:

- A small description of what the diagnostic does (reccomended)
- A list of the options defined by the diagnostic to configure it. It is recommended that this list comes with a small explanation of it's meaning and effects, the type of the expected values and any default values taken when they are not explivitly defined (recommended).
- A list of the mandatory common options required for the diagnostic to work (recommended).
- A link to the online documentation, if available (suggested)

## The diagnostic configuration file

This section will define the syntax fot he main configuration file. It must be a yaml file containing a dictionary that contains a dictionary with all the options needed by a diagnostic

### Required options

The following options must be present in the configuration file passed to the diagnostic.

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


## The data definition file

The data definition file must be a yaml file containing a dictionary of dictionaries. This first dictionary will have as keys the path to the data files and as values the dictionary containg the metadata of each file.

There is no limit to the number of file that can be provided on a single file, although it is suggested

### Required options

- **alias**: `str`. Unique name for the dataset. Any (`alias`, `variable`) pair must be unique
- **filename**: `str`. Absoulute path to the data file.
- **variable**: `str`. Variable name to be used by the diagnostic.

### Reserved options

#### Related to the dataset

- **project**: `str`. Name of the project at which this dataset belongs. It can be a real project, like `CMIP5`, `CMIP6` or a tool defined agrupation of datasets like `Observations` or `Reanalysis`
- **activity**: `str`. CMIP activity or analog for the current data
- **institute**: `list(str)`. List of institutes associated to the current data.
- **dataset**: `str`. Name of the dataset, being it the CMIP model indetifier, the name of an observation
- **ensemble**: `str`. Ensemble identifier for the current data

#### Related to the variable

- **mip**: `str`. Original mip of the current variable
- **frequency**: `str`. Original frequency of the data used
- **modeling_realm**: `list(str)`. List of realms at which the variable belonh
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
