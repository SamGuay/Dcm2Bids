# Configuration Guide for dcm2bids

This guide will help you create and customize configuration files for dcm2bids. The configuration file is a JSON file that defines how DICOM files should be converted to BIDS format.

## Table of Contents

1. [Basic Structure](#basic-structure)
2. [Search Method](#search-method)
3. [Post-Operations](#post-operations)
4. [Descriptions](#descriptions)
   - [Datatype and Suffix](#datatype-and-suffix)
   - [Criteria](#criteria)
   - [Custom Labels](#custom-labels)
   - [Sidecar Changes](#sidecar-changes)
   - [IntendedFor](#intendedfor)
5. [Examples](#examples)
   - [Anatomical Images](#anatomical-images)
   - [Functional Images](#functional-images)
   - [Diffusion Images](#diffusion-images)
   - [Field Maps](#field-maps)
6. [Advanced Configuration](#advanced-configuration)

## Basic Structure

A dcm2bids configuration file is a JSON file with the following main sections:

```json
{
    "search_method": "fnmatch",
    "post_op": [],
    "descriptions": []
}
```

## Search Method

The `search_method` key specifies how dcm2bids should match the criteria in the descriptions. The default and recommended value is `"fnmatch"`, which uses Unix shell-style wildcards. Alternatively, you can use `"re"` for regular expressions.

```json
"search_method": "fnmatch"
```

## Post-Operations

The `post_op` section allows you to define operations to be performed after the conversion. This is typically used for de-identification of anatomical images.

```json
"post_op": [
    {
        "cmd": "pydeface --outfile dstFile srcFile",
        "datatype": "anat",
        "suffix": ["T1w", "MP2RAGE"]
    }
]
```

## Descriptions

The `descriptions` section is an array of objects, each describing a type of image acquisition. Each description object can have the following properties:

### Datatype and Suffix

```json
{
    "datatype": "anat",
    "suffix": "T1w"
}
```

### Criteria

The `criteria` object specifies how to identify the correct DICOM files for this description.

```json
"criteria": {
    "SeriesDescription": "*T1W_3D*"
}
```

### Custom Labels

You can add custom labels to further specify the image:

```json
"customLabels": "task-rest"
```

### Sidecar Changes

The `sidecar_changes` object allows you to modify the content of the JSON sidecar file:

```json
"sidecar_changes": {
    "SeriesDescription": "rsfMRI"
}
```

### IntendedFor

For fieldmap images, you can specify which functional images they are intended to correct:

```json
"IntendedFor": 5
```

## Examples

### Anatomical Images

```json
{
    "datatype": "anat",
    "suffix": "T1w",
    "criteria": {
        "SeriesDescription": "*T1W_3D*"
    }
}
```

### Functional Images

```json
{
    "datatype": "func",
    "suffix": "bold",
    "customLabels": "task-rest",
    "criteria": {
        "SeriesDescription": "rs_fMRI"
    },
    "sidecar_changes": {
        "SeriesDescription": "rsfMRI"
    }
}
```

### Diffusion Images

```json
{
    "datatype": "dwi",
    "suffix": "dwi",
    "criteria": {
        "SeriesDescription": "*DWI*"
    }
}
```

### Field Maps

```json
{
    "datatype": "fmap",
    "suffix": "fmap",
    "criteria": {
        "SidecarFilename": "*echo-4*"
    },
    "IntendedFor": 5
}
```

## Advanced Configuration

For more complex matching, you can use nested criteria:

```json
"criteria": {
    "SeriesDescription": {
        "any": ["*TSE*", "*FLAIR*"]
    },
    "EchoTime": {
        "gt": 0.1
    }
}
```

This configuration will match series descriptions containing either "TSE" or "FLAIR", and with an echo time greater than 0.1.

Remember to test your configuration thoroughly to ensure it correctly identifies and converts your DICOM files to the desired BIDS structure.