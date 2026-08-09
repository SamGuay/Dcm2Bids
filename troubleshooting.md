---
title: Troubleshooting Guide
---

# Troubleshooting Guide

This guide covers common issues you might encounter when using dcm2bids and provides solutions to help you resolve them.

## Table of Contents

1. [dcm2niix Not Found](#dcm2niix-not-found)
2. [Invalid Configuration File](#invalid-configuration-file)
3. [No Pairing Found](#no-pairing-found)
4. [BIDS Validation Errors](#bids-validation-errors)
5. [Overwriting Existing Files](#overwriting-existing-files)
6. [Auto Extract Entities and Do Not Reorder Entities Conflict](#auto-extract-entities-and-do-not-reorder-entities-conflict)

## dcm2niix Not Found

### Error Message

```
dcm2niix is not in your PATH or not installed.
```

### Cause

This error occurs when the `dcm2niix` tool is not installed or not found in your system's PATH.

### Solution

1. Install `dcm2niix` if you haven't already. You can find installation instructions at [https://github.com/rordenlab/dcm2niix](https://github.com/rordenlab/dcm2niix).
2. Ensure that the installation directory of `dcm2niix` is added to your system's PATH.
3. Verify the installation by running `dcm2niix --version` in your terminal.

## Invalid Configuration File

### Error Message

```
JSONDecodeError: Expecting value: line X column Y (char Z)
```

### Cause

This error occurs when the JSON configuration file is not properly formatted or contains syntax errors.

### Solution

1. Open your configuration file in a text editor.
2. Check for common JSON syntax errors, such as missing commas, unmatched brackets, or quotation marks.
3. Use a JSON validator tool to identify and fix any issues.
4. Ensure that all required fields are present in the configuration file.

## No Pairing Found

### Error Message

```
WARNING: No pairing was found. BIDS folder "{output_dir}" won't be created. Check your config file.
```

### Cause

This warning appears when dcm2bids cannot match any of your DICOM files with the descriptions in your configuration file.

### Solution

1. Review your configuration file and ensure that the descriptions accurately match your DICOM data.
2. Check if the `datatype` and `suffix` in your configuration file correspond to your DICOM files.
3. Verify that the `criteria` in your descriptions are correct and not too restrictive.
4. Double-check that the DICOM directories you provided contain the expected data.

## BIDS Validation Errors

### Error Message

Various BIDS validation errors reported by the BIDS Validator.

### Cause

These errors occur when the output does not conform to the BIDS specification.

### Solution

1. Review the specific errors reported by the BIDS Validator.
2. Check your configuration file to ensure it follows BIDS naming conventions.
3. Verify that all required metadata fields are present in your JSON sidecar files.
4. Make sure your folder structure adheres to the BIDS specification.
5. Update your configuration and rerun dcm2bids with any necessary corrections.

## Overwriting Existing Files

### Error Message

```
'{file_path}' already exists
Use --clobber option to overwrite
```

### Cause

dcm2bids found existing files in the output directory and is set to not overwrite them by default.

### Solution

1. If you want to overwrite existing files, use the `--clobber` option when running dcm2bids:
   ```
   dcm2bids -d /path/to/dicom -p 01 -c config.json --clobber
   ```
2. If you don't want to overwrite files, either remove the existing files manually or use a different output directory.

## Auto Extract Entities and Do Not Reorder Entities Conflict

### Error Message

```
ValueError: Auto extract entities is set to True and do not reorder entities is set to True. Please choose only one option.
```

### Cause

This error occurs when both `--auto_extract_entities` and `--do_not_reorder_entities` options are set to True, which are mutually exclusive.

### Solution

1. Choose only one of these options based on your requirements:
   - Use `--auto_extract_entities` if you want dcm2bids to automatically extract entity information.
   - Use `--do_not_reorder_entities` if you want to maintain the order of entities as defined in your configuration.
2. Remove or set to False the option you don't want to use.

Remember to check the dcm2bids log file for detailed information about any errors or warnings encountered during the conversion process. If you encounter issues not covered in this guide, please refer to the [dcm2bids GitHub repository](https://github.com/UNFmontreal/Dcm2Bids) for additional support or to report new issues.