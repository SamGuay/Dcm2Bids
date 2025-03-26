# Getting Started with dcm2bids

## Introduction

dcm2bids is a powerful tool for reorganizing NIfTI files from dcm2niix into the Brain Imaging Data Structure (BIDS) format. This guide will help you get started with dcm2bids, covering installation, basic usage, and an overview of its main features.

## Installation

To install dcm2bids, you'll need Python 3.7 or higher. You can install it using pip:

```
pip install dcm2bids
```

Make sure you have dcm2niix installed on your system as well, as dcm2bids relies on it for DICOM to NIfTI conversion.

## Basic Usage

Here's a basic example of how to use dcm2bids:

```
dcm2bids -d /path/to/dicom/directory -p participant_id -c /path/to/config.json
```

Let's break down the command:

- `-d`: Specifies the DICOM directory or archive
- `-p`: Sets the participant ID
- `-c`: Points to the JSON configuration file

## Configuration File

The configuration file is crucial for dcm2bids. It defines how your DICOM files should be organized into the BIDS structure. Here's a simple example:

```json
{
    "descriptions": [
        {
            "datatype": "anat",
            "suffix": "T1w",
            "criteria": {
                "SeriesDescription": "*T1W_3D*"
            }
        },
        {
            "datatype": "func",
            "suffix": "bold",
            "customLabels": "task-rest",
            "criteria": {
                "SeriesDescription": "rs_fMRI"
            }
        }
    ]
}
```

This configuration tells dcm2bids to look for T1-weighted anatomical scans and resting-state functional MRI scans, and how to label them in the BIDS structure.

## Main Features

### 1. Flexible Matching Criteria

dcm2bids allows you to use various criteria to match your DICOM files, including SeriesDescription, EchoTime, and even custom sidecar filename patterns.

### 2. Custom Labeling

You can add custom labels to your BIDS filenames using the `customLabels` field in your configuration.

### 3. Automatic Entity Extraction

Use the `--auto_extract_entities` flag to automatically extract entity information based on the suffix and datatype.

### 4. Post-processing Operations

dcm2bids supports post-processing operations. For example, you can automatically deface T1w images:

```json
"post_op": [{
    "cmd" : "pydeface --outfile dstFile srcFile",
    "datatype": "anat",
    "suffix": ["T1w", "MP2RAGE"]
}]
```

### 5. BIDS Validation

Use the `--bids_validate` flag to check if your output folder is BIDS-compliant after conversion.

## Advanced Usage

For more advanced usage, you can:

- Specify a session ID with the `-s` flag
- Use multiple DICOM directories or archives
- Skip dcm2niix conversion with `--skip_dcm2niix` if you already have NIfTI files

## Logging

dcm2bids provides detailed logs of its operations. You can set the log level using the `-l` flag:

```
dcm2bids ... -l DEBUG
```

Logs are saved in the output directory under `tmp_dcm2bids/log/`.

## Conclusion

This guide should help you get started with dcm2bids. For more detailed information, refer to the full documentation and the example configuration files provided with the package. Happy BIDS converting!