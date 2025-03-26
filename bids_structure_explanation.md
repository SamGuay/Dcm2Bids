# BIDS Structure Explanation

## Introduction

dcm2bids is a tool designed to reorganize NIfTI files from dcm2niix into the Brain Imaging Data Structure (BIDS) format. This document explains how dcm2bids organizes output files according to the BIDS specification, including information on directory structure and file naming conventions.

## BIDS Directory Structure

dcm2bids creates a BIDS-compliant directory structure for your neuroimaging data. The basic structure is as follows:

```
bids_root/
│
├── sub-<participant_label>/
│   ├── ses-<session_label>/
│   │   ├── anat/
│   │   ├── func/
│   │   ├── dwi/
│   │   └── ...
│   │
│   ├── anat/
│   ├── func/
│   ├── dwi/
│   └── ...
│
├── derivatives/
├── code/
├── dataset_description.json
└── README
```

- The `bids_root` is the main directory containing all BIDS-organized data.
- Each participant has a subdirectory named `sub-<participant_label>`.
- If sessions are used, each session has a subdirectory named `ses-<session_label>` within the participant's directory.
- Data modalities (e.g., `anat`, `func`, `dwi`) are organized into separate subdirectories.

## File Naming Conventions

dcm2bids follows the BIDS naming conventions for files. The general structure of a BIDS filename is:

```
sub-<participant_label>[_ses-<session_label>][_<entity-label>-<entity-value>]_<suffix>.<extension>
```

Where:
- `<participant_label>` is the unique identifier for the participant.
- `<session_label>` (optional) is the identifier for the session.
- `<entity-label>` and `<entity-value>` are additional descriptors (e.g., task, run, acquisition).
- `<suffix>` describes the type of data (e.g., T1w, bold, dwi).
- `<extension>` is the file extension (e.g., .nii.gz, .json).

Examples:
- `sub-01_ses-baseline_T1w.nii.gz`
- `sub-01_ses-baseline_task-rest_bold.nii.gz`
- `sub-01_ses-baseline_dwi.nii.gz`

## How dcm2bids Organizes Files

1. **Sidecar Pairing**: dcm2bids uses JSON sidecar files to match DICOM series with the appropriate BIDS structure. This is done in the `SidecarPairing` class in `sidecar.py`.

2. **Acquisition Building**: The `build_acquisitions` method in `SidecarPairing` creates `Acquisition` objects for each matched sidecar, determining the correct BIDS path and filename.

3. **Custom Entities**: dcm2bids supports custom entities, which can be extracted from DICOM tags using regular expressions defined in the configuration file.

4. **Run Handling**: The `find_runs` method in `SidecarPairing` detects duplicate destinations and adds run numbers to differentiate them.

5. **File Moving**: The `move` method in `Dcm2BidsGen` (from `dcm2bids_gen.py`) handles the actual file copying and renaming process, ensuring files are placed in the correct BIDS structure.

## Configuration

dcm2bids uses a configuration file to define how DICOM series should be matched and named in the BIDS structure. The configuration file allows you to specify:

- Criteria for matching DICOM series
- Output filename components (e.g., datatype, suffix)
- Custom entities to extract from DICOM metadata

Example configuration snippet:

```json
{
  "descriptions": [
    {
      "datatype": "anat",
      "suffix": "T1w",
      "criteria": {
        "SeriesDescription": "*T1*"
      }
    },
    {
      "datatype": "func",
      "suffix": "bold",
      "criteria": {
        "SeriesDescription": "*fMRI*"
      },
      "custom_entities": ["task"]
    }
  ]
}
```

## Conclusion

dcm2bids automates the process of organizing neuroimaging data into the BIDS format, ensuring consistent directory structures and file naming conventions. By understanding how dcm2bids structures the output, users can effectively configure the tool and work with the resulting BIDS-compliant datasets.