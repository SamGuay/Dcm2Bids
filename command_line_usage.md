# Command Line Usage

This documentation provides detailed information on how to use dcm2bids from the command line. It covers all available commands, options, and their usage.

## dcm2bids

The main command for converting DICOM files to BIDS-compliant NIfTI files.

### Usage

```
dcm2bids -d <dicom_dir> -p <participant> -c <config> [options]
```

### Required Arguments

- `-d`, `--dicom_dir`: DICOM directory(ies) or archive(s). Multiple directories can be specified.
- `-p`, `--participant`: Participant ID.
- `-c`, `--config`: JSON configuration file (see example/config.json).

### Optional Arguments

- `-s`, `--session`: Session ID. (Default: None)
- `-o`, `--output_dir`: Output BIDS directory. (Default: current directory)
- `--auto_extract_entities`: Automatically extract entity information [task, dir, echo] based on the suffix and datatype.
- `--do_not_reorder_entities`: Do not reorder entities according to the BIDS specification.
- `--bids_validate`: Check if the output folder is BIDS valid after conversion.
- `--force_dcm2bids`: Overwrite previous temporary dcm2bids output if it exists.
- `--skip_dcm2niix`: Skip dcm2niix conversion. Option -d should contain NIFTI and JSON files.
- `--clobber`: Overwrite output if it exists.
- `-l`, `--log_level`: Set logging level to the console. (Choices: DEBUG, INFO, WARNING, ERROR, CRITICAL; Default: INFO)
- `-v`, `--version`: Report dcm2bids version and the BIDS version.

## dcm2bids_helper

Converts DICOM files to NIfTI files including their JSON sidecars in a temporary directory, which can be used to create a dcm2bids config file.

### Usage

```
dcm2bids_helper -d <dicom_dir> [options]
```

### Required Arguments

- `-d`, `--dicom_dir`: DICOM directory(ies) or archive(s). Multiple directories can be specified.

### Optional Arguments

- `-o`, `--output_dir`: Output directory. (Default: ./tmp_dcm2bids/helper)
- `-n`, `--nest`: Nest a directory in the output directory. Useful for multiple helper runs.
- `--force`: Force command to overwrite existing output files.
- `-l`, `--log_level`: Set logging level to the console. (Choices: DEBUG, INFO, WARNING, ERROR, CRITICAL; Default: INFO)

## dcm2bids_scaffold

Creates basic BIDS files and directories.

### Usage

```
dcm2bids_scaffold [options]
```

### Optional Arguments

- `-o`, `--output_dir`: Output BIDS directory. (Default: current directory)
- `--force`: Force command to overwrite existing output files.

## Examples

1. Convert DICOM files to BIDS format:

```
dcm2bids -d /path/to/dicom/folder -p 01 -c config.json
```

2. Use dcm2bids_helper to prepare for config file creation:

```
dcm2bids_helper -d /path/to/dicom/folder -o /path/to/output
```

3. Create a BIDS scaffold:

```
dcm2bids_scaffold -o /path/to/bids/directory
```

For more detailed information on each command and its options, please refer to the respective sections above.