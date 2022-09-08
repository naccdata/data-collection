# NACC (Non-Form) Data Collection

These are recommendations for management of files that contains non-form data collected during a visit by a participant.

## File Organization

Ultimately, we expect all data will be organized in a hierarchy by center, participant and visit.
Assuming that your data is eventually archived at NACC, matching this hierarchy will mean fewer transformations when the data is submitted. 

You can build a directory structure matching this hierarchy like the following: 

```bash
adc-[ADCID]/ 
└── ptid-[PTID]/ 
    └── visit-[DATE]/ 
        ├── metadata.json 
        ├── file_1 
        ...
        └── file_n 
```
 
where `[ADCID]` is the center ADCID, `[PTID]` is the participant ID (or NACCID), and `[DATE]` is a standard representation of the date (e.g., `2022-08-17`).
The metadata file should be consistently named across the project, and all data files you want us to keep should be stored in the directory for the visit in which they were acquired. 
For example, for a visit where a voice recording is captured, the file might be 

```bash
adc-00/ 
└── ptid-1/ 
    └── visit-2022-08-17/ 
        ├── visit.json 
        └── voice-recording-241.wav 
```

In this example, the metadata is named `visit.json` and there is one data file named `voice-recording-241.wav`.
This data file name indicates the type of data and includes an accession number that uniquely identifies the file.

We suggest using a naming scheme that gives readable short file names that describe the type of content and include a unique accession number.
Having short meaningful names will help when viewing data in a directory.

## Accession numbers

Accession numbers can help with tracking of files when they are stored separately from the metadata, which might be desirable to avoid storing files in proximity of identifying information.

## Metadata

We will expect any metadata about the visit and the list of files for the visit to be provided in the metadata file. We will be requiring a JSON or YAML file for the metadata on submission. 
Example: 



Where visit.json contains 
{ 
    “adcid”: 00, 
   “ptid”: 1, 
    “files”:  