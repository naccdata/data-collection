# NACC (Non-Form) Data Collection

These are recommendations for management of files that contains non-form data collected during a visit of a participant.

## File Organization

We expect all data coming from a center to be organized in a hierarchy: center, participant, visit.
Assuming that the data for the project is eventually stored at NACC, matching this hierarchy will mean fewer transformations when the data is submitted.

A directory structure matching this hierarchy would look like:

```bash
adc-[ADCID]/ 
└── ptid-[PTID]/ 
    └── visit-[DATE]/ 
        ├── manifest.json 
        ├── file_1 
        ...
        └── file_n 
```

 
where `[ADCID]` is the center ADCID, `[PTID]` is the participant ID (or NACCID), and `[DATE]` is a standard representation of the date (e.g., `2022-08-17` ).
The manifest file should contain metadata for the files, and should be consistently named across the project.
And, all data files should be stored in the directory for the visit in which they were acquired. 
For example, for a visit where a voice recording is captured, the file might be 

```bash
adc-00/ 
└── ptid-1/ 
    └── visit-2022-08-17/ 
        ├── manifest.json 
        └── voice-recording-241.wav 
```

In this example, there is one data file named `voice-recording-241.wav` .
This data file name indicates the type of data and includes an accession number that uniquely identifies the file.

We suggest using a naming scheme that gives readable short file names that describe the type of content and include a unique accession number.
Having short meaningful names will help when viewing data in a directory.

## Accession numbers

Accession numbers can help with tracking of files when they are stored separately from the metadata, which might be desirable to avoid storing files in proximity of identifying information.

## Manifest

We will expect metadata about the visit and the list of files for the visit to be provided in the manifest file. 
We will be requiring a JSON or YAML file for the metadata on submission. 

The `manifest.json` file for the data might be 

```json
{
    "centerId": "00",
    "participantId": "1",
    "visitDate": "2022-08-17",
    "files": [
        {
            "name": "voice-recording-241.wav",
            "title": "entity",
            "attributedTo": {
                "investigator": "AZK",
                "title: "agent"
            }
        }
    ]
}
```

to capture that the the file was from an interview done by an investigator with initials `AZK` .

We will be supporting a metadata format capturing provencance based on the [PROV-DM](https://www.w3.org/TR/prov-dm/), which would allow capturing more detail about how files are created.
This example is (roughly) based on the [PROV-JSON](https://www.w3.org/Submission/prov-json/) schema, which uses the `"title"` tag to identify the type of object.
