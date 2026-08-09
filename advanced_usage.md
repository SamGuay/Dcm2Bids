# Advanced Usage

This guide covers advanced usage scenarios for dcm2bids, including custom entity extraction, post-processing operations, and integrating dcm2bids into larger workflows.

## Custom Entity Extraction

dcm2bids supports custom entity extraction from DICOM tags. This feature allows you to add additional BIDS entities to your output files based on information in the DICOM headers.

### Configuration

To use custom entity extraction, you need to modify your configuration file. Here's an example of how to set it up:

```json
{
  "extractors": {
    "SeriesDescription": [
      "(?P<dir>(AP|PA))",
      "(?P<task>[a-zA-Z0-9]+)"
    ]
  },
  "descriptions": [
    {
      "datatype": "func",
      "suffix": "bold",
      "custom_entities": ["dir", "task"]
    }
  ]
}
```

In this example:
- We define extractors for the `SeriesDescription` DICOM tag.
- We use regular expressions to extract `dir` and `task` entities.
- In the `descriptions` section, we specify which custom entities to use for a particular datatype and suffix.

### Auto-extraction of Entities

dcm2bids also supports automatic extraction of entities. To enable this feature, set `auto_extract_entities` to `True` when initializing `Dcm2BidsGen`:

```python
dcm2bids_gen = Dcm2BidsGen(
    ...,
    auto_extract_entities=True,
    ...
)
```

This will automatically extract entities based on predefined patterns for certain datatypes and suffixes.

## Post-processing Operations

dcm2bids allows you to define post-processing operations that are applied to your files after they've been organized into the BIDS structure.

### Configuration

Post-processing operations are defined in the configuration file under the `post_op` key. Here's an example:

```json
{
  "post_op": [
    {
      "cmd": "pydeface --outfile dst_file src_file",
      "datatype": "anat",
      "suffix": ["T1w", "MP2RAGE"]
    }
  ]
}
```

In this example:
- We define a post-processing operation using the `pydeface` tool.
- This operation will be applied to anatomical images with suffixes "T1w" or "MP2RAGE".
- `src_file` and `dst_file` are placeholders that will be replaced with actual file paths during execution.

### Custom Entities in Post-processing

You can also use custom entities in your post-processing operations:

```json
{
  "post_op": [
    {
      "cmd": "custom_tool --param1 value1 --outfile dst_file src_file",
      "datatype": "func",
      "suffix": "bold",
      "custom_entities": ["task-rest"]
    }
  ]
}
```

This operation will only be applied to functional images with the "task-rest" entity.

## Integrating dcm2bids into Larger Workflows

dcm2bids can be easily integrated into larger neuroimaging workflows. Here are some tips for effective integration:

1. **Automation**: Use shell scripts or Python scripts to automate the execution of dcm2bids across multiple subjects or sessions.

2. **Error Handling**: Implement robust error handling in your scripts to catch and log any issues that occur during the conversion process.

3. **Parallel Processing**: For large datasets, consider implementing parallel processing to run dcm2bids on multiple subjects simultaneously.

4. **Quality Control**: Integrate automatic quality control checks after the BIDS conversion to ensure the output meets your standards.

5. **Version Control**: Keep your dcm2bids configuration files under version control to track changes over time.

Here's a simple Python script that demonstrates how to integrate dcm2bids into a larger workflow:

```python
import os
from dcm2bids import Dcm2BidsGen

def process_subject(subject_id, dicom_dir, output_dir, config_file):
    dcm2bids_gen = Dcm2BidsGen(
        dicom_dir=dicom_dir,
        participant=subject_id,
        config=config_file,
        output_dir=output_dir,
        auto_extract_entities=True
    )
    dcm2bids_gen.run()

def main():
    base_dicom_dir = "/path/to/dicom/data"
    output_dir = "/path/to/bids/output"
    config_file = "/path/to/config.json"

    subject_list = ["sub-01", "sub-02", "sub-03"]  # Add your subject IDs here

    for subject_id in subject_list:
        dicom_dir = os.path.join(base_dicom_dir, subject_id)
        process_subject(subject_id, dicom_dir, output_dir, config_file)

if __name__ == "__main__":
    main()
```

This script processes multiple subjects sequentially. For parallel processing, you could use Python's `multiprocessing` module or a job scheduling system appropriate for your computing environment.

Remember to customize the paths and subject IDs according to your specific setup and needs.

By leveraging these advanced features and integration techniques, you can create powerful and flexible DICOM to BIDS conversion workflows tailored to your specific research needs.