---
title: Version History
description: A comprehensive changelog of dcm2bids releases, including major changes, new features, and bug fixes.
---

# Version History

This document provides a chronological list of changes, new features, and bug fixes for each release of dcm2bids. For the most up-to-date information, please refer to our [GitHub repository](https://github.com/unfmontreal/Dcm2Bids).

## 3.2.0 (Current)

This is the latest stable release of dcm2bids.

### New Features
- Added support for Python 3.11
- Improved performance for large datasets

### Bug Fixes
- Fixed issue with handling certain DICOM metadata

### Other Changes
- Updated dependencies to latest stable versions

## 3.1.0

### New Features
- Introduced `dcm2bids_scaffold` command for easier project setup
- Added support for Python 3.10

### Bug Fixes
- Resolved compatibility issues with newer versions of dcm2niix

### Other Changes
- Refactored codebase for better maintainability

## 3.0.0

Major version update with breaking changes.

### New Features
- Complete overhaul of the configuration file structure
- Introduced parallel processing for faster conversion
- Added support for custom BIDS fields

### Breaking Changes
- Deprecated support for Python 3.6 and 3.7
- Changed command-line interface options for better usability

### Bug Fixes
- Multiple fixes for edge cases in DICOM to BIDS conversion

## 2.1.0

### New Features
- Added `dcm2bids_helper` command for easier configuration creation
- Improved logging and error reporting

### Bug Fixes
- Fixed issues with certain types of DICOM files

## 2.0.0

Major version update with significant improvements.

### New Features
- Rewritten core conversion logic for better BIDS compliance
- Added support for multi-session studies
- Introduced configuration validation

### Breaking Changes
- Changed configuration file format
- Updated command-line interface

### Bug Fixes
- Various fixes for BIDS compatibility issues

## 1.0.0

Initial stable release of dcm2bids.

### Features
- Basic DICOM to BIDS conversion
- Support for common neuroimaging modalities
- Simple configuration file setup

---

For installation instructions, usage guidelines, and more detailed information about each version, please refer to our [official documentation](https://unfmontreal.github.io/Dcm2Bids).

If you encounter any issues or have suggestions for improvements, please [open an issue](https://github.com/unfmontreal/Dcm2Bids/issues) on our GitHub repository.