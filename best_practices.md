---
title: Best Practices for Using dcm2bids
---

# Best Practices for Using dcm2bids

This guide outlines best practices for effectively using dcm2bids to convert DICOM files to BIDS format. Following these recommendations will help you organize your input data, create efficient configuration files, and optimize performance.

## 1. Organizing Input Data

### 1.1 DICOM Directory Structure

- Keep your DICOM files organized in a clear directory structure.
- Use meaningful folder names that reflect the study structure (e.g., by participant, session, or scan type).

Example structure:
```
study_root/
├── dicom/
│   ├── sub-001/
│   │   ├── session1/
│   │   └── session2/
│   └── sub-002/
│       ├── session1/
│       └── session2/
└── bids_output/
```

### 1.2 Consistency in Naming Conventions

- Use consistent naming conventions for your DICOM folders and files.
- This will make it easier to create patterns in your configuration file.

## 2. Creating Efficient Configuration Files

### 2.1 Use the Appropriate Search Method

- Set the `search_method` in your configuration file based on your needs:
  - Use `fnmatch` for simple pattern matching (default).
  - Use `re` for more complex regular expressions.

Example:
```json
{
    "search_method": "fnmatch",
    // ... other configuration options
}
```

### 2.2 Optimize Description Criteria

- Be as specific as possible in your criteria to avoid misclassification.
- Use multiple criteria when necessary to uniquely identify a scan type.

Example:
```json
{
    "descriptions": [
        {
            "datatype": "anat",
            "suffix": "T1w",
            "criteria": {
                "SeriesDescription": "*T1W_3D*",
                "EchoTime": 0.00275
            }
        }
    ]
}
```

### 2.3 Leverage Custom Labels

- Use `customLabels` to add additional information to your file names when needed.

Example:
```json
{
    "datatype": "func",
    "suffix": "bold",
    "customLabels": "task-rest",
    "criteria": {
        "SeriesDescription": "rs_fMRI"
    }
}
```

### 2.4 Utilize Sidecar Changes

- Use `sidecar_changes` to modify metadata in the output JSON sidecar files.

Example:
```json
{
    "datatype": "func",
    "suffix": "bold",
    "criteria": {
        "SeriesDescription": "rs_fMRI"
    },
    "sidecar_changes": {
        "SeriesDescription": "rsfMRI"
    }
}
```

## 3. Optimizing Performance

### 3.1 Use dcm2niix Options

- Customize dcm2niix options in your configuration file to balance between conversion speed and output file size.

Example:
```json
{
    "dcm2niixOptions": "-b y -ba y -z y -f '%3s_%f_%p_%t'"
}
```

### 3.2 Parallel Processing

- If converting large datasets, consider running multiple dcm2bids instances in parallel on different subsets of your data.

### 3.3 Skip dcm2niix When Appropriate

- Use the `--skip_dcm2niix` option if you've already run dcm2niix and want to rerun the BIDS conversion with a different configuration.

## 4. Post-conversion Operations

### 4.1 Leverage Post-op Commands

- Use the `post_op` configuration to perform additional processing on specific file types after conversion.

Example:
```json
{
    "post_op": [
        {
            "cmd": "pydeface --outfile dstFile srcFile",
            "datatype": "anat",
            "suffix": ["T1w", "MP2RAGE"]
        }
    ]
}
```

### 4.2 Validate Your BIDS Output

- Always use the `--bids_validate` option to ensure your output adheres to the BIDS specification.

## 5. Version Control and Documentation

- Keep your configuration files under version control.
- Document any custom scripts or workflows you use alongside dcm2bids.
- Maintain a README file explaining your project structure and conversion process.

By following these best practices, you'll be able to use dcm2bids more effectively, resulting in a well-organized BIDS dataset and a more streamlined conversion process.