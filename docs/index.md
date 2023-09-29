# NACC Data Collection

These are recommendations for management of files that contains non-form data collected during a visit of a participant.

## Forms

Data from forms, UDS and otherwise, are submitted to NACC in a tabular format.
Each UDS form has a header that has metadata describing the data

- Form ID (implicit)
- Center ID
- Participant ID
- Visit number
- Date of form completion
- Investigator initials
- Language used during visit
- Mode of completion
  - in person
  - remote -- phone or video, with reason
  - not completed, with reason

This data identifies 

- which participant the data applies to (Participant ID)
- attributes of the data collection
  - where (Center ID)
  - when the data was collected (Visit number, Date of completion)
  - who collected the data (investigator)
  - how the data was collected: language and mode

Non-form data should similarly have metadata that describes what the data represents and how it was collected.

## Non-form data

### Identifying participants

### Identifying data

### 





## File Organization

NACC ultimately organizes all data coming from a center in a hierarchy: center, participant, visit.
Using a file structure matching this hierarchy will mean fewer transformations when the data is submitted.
However, data that naturally convert to a tabular form could be submitted using a CSV format.

### Hierarchical organization

A directory structure matching the data hierarchy would look like:

```bash
adc-[ADCID]/ 
└── ptid-[PTID]/ 
    └── visit-[DATE]/ 
        ├── manifest.json 
        ├── file_1 
        ...
        └── file_n 
```

where `[ADCID]` is the center ADCID, `[PTID]` is the participant ID (or NACCID), and `[DATE]` is in [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339) date format as `%Y%m%d` (year-month-day with month and day zero padded); e.g., `20220817`.
The manifest file should contain metadata for the files, and should be consistently named across the project.
And, all data files should be stored in the directory for the visit in which they were acquired.
For example, for a visit where a voice recording is captured, the file might be

```bash
adc-00/ 
└── ptid-1/ 
    └── visit-20220817/ 
        ├── manifest.json 
        └── voice-recording-241.wav 
```

In this example, there is one data file named `voice-recording-241.wav` .
This data file name indicates the type of data and includes an accession number that uniquely identifies the file.
(This is to avoid confusion among multiple files named `voice-recording.wav`.)

We suggest using a naming scheme that gives readable short file names that describe the type of content and include a unique accession number.
Having short meaningful names will help when viewing data in a directory.

The `manifest.json` file for the data might be 

```json
{
    "center_id": "00",
    "participant_id": "1",
    "visit_date": "2022-08-17",
    "files": [
        {
            "name": "voice-recording-241.wav",
            "title": "entity",
            "attributed_to": {
                "investigator": "AZK",
                "title": "agent"
            }
        }
    ]
}
```

### Tabular organization

NACC can also accept data stored in a 

## Accession numbers
Accession numbers help with tracking of files when they are stored separately from the metadata, which might be desirable to avoid storing files in proximity of identifying information.


## Manifest

We will expect metadata about the visit and the list of files for the visit to be provided in the manifest file.
We will be requiring a JSON or YAML file for the metadata on submission. 

The `manifest.json` file for the data might be 

```json
{
    "center_id": "00",
    "participant_id": "1",
    "visit_date": "2022-08-17",
    "files": [
        {
            "name": "voice-recording-241.wav",
            "title": "entity",
            "attributed_to": {
                "investigator": "AZK",
                "title": "agent"
            }
        }
    ]
}
```

to capture that the the file was from an interview done by an investigator with initials `AZK` .

We will be supporting a metadata format capturing provencance based on the [PROV-DM](https://www.w3.org/TR/prov-dm/), which would allow capturing more detail about how files are created.
This example is (roughly) based on the [PROV-JSON](https://www.w3.org/Submission/prov-json/) schema, which uses the `"title"` tag to identify the type of object.
