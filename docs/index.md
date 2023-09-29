# NACC Data Collection Requirements and Recommendations

This is a reference for projects that are planning on submitting new data types to NACC.

## Requirements on Data Format

NACC is able to accept and manage files in any format provided there are tools available for

1. QC of submitted files.
   If you are making a proposal to submit data, you should be able to provide software or schema that support QC checks of data files.
   NACC also needs to be able to maintain or rely on maintained software implementations or schema definitions.

2. manipulation of the data, either by NACC to derive data for release, or for analysis by external researchers, if the raw data may be released as is.

## Requirements on Metadata

Data should be accompanied with metadata that describes the following concepts

|Concept|Description|
|-------|-----------|
|Study|The research activity for which data was collected|
|Center|The site where data was collected|
|Observer|The person who collected the data|
|Participant|The person observed to collect the data|
|Visit|The participant's visit to the center for data collection|
|Acquisition|The activity that collected the data|
|Instrument|The instrument/mechanism used to collect data|

The goal of the metadata is to describe the provenance of the data (how the data was collected and what has been done to it since), but minimally the metadata should define the following

- center id
- participant id
- visit number
- acquisition attributes
- instrument id
- observer identification

See the discussion about [metadata](metadata.md) for more details.

The exact expectations can be adjusted based on how the data is collected

### Expectations on Participant IDs

ADRC participants can be identified by the ADCID (NACC assigned center ID) with the ADRC assigned participant ID, the NACCID, or the NIA GUID.

### Expectations on Dates

Dates should be given in the form year-month-day with month and day zero padded; e.g.,`20220817`. Which follows [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339), and matches `%Y%m%d`.

### Expectations on File naming

When data is separated from metadata, data files should be given accession numbers to allow a unique mapping from identifying information to the file.

### On identifying studies

Much of the data that is collected by NACC is for the ADRC program, and the study hasn't historicially been captured within the data.
We assume that data is submitted in the context of a particular study and so the study can be understood.

However, the study is a short-hand for the participant consent with subsequent limitations on use.
If your data requires different consent than the ADRC program, the study should be identified.

### Manifest format

We will be supporting a metadata format capturing provenance based on the [PROV-DM](https://www.w3.org/TR/prov-dm/), which would allow capturing more detail about how files are created.

## File Organization

NACC ultimately stores data using the following hierarchy relative to the concepts above

```bash
Center
└── Study
    └── Participant
        └── Visit
            └── Acquisition
                └── Data
```

Using a file structure matching this hierarchy will mean fewer transformations when the data is submitted.

We will support data to be submitted using one of these organizations:

1. Hierarchical - files are organized by the full hierarchy above
2. Tabular metadata - files are stored independently with accession numbers, with metadata in tabular form
3. Tabular data - data and metadata are stored in tabular form

### Hierarchical

A fully hierarchical organization can be used in a situation where data is captured as individual files from an acquisition within a visit.
In this case, the full hierarchy is elaborated with a `manifest.json` file containing the metadata.
An example of the hierarchical structure where  there are multiple voice recordings is

```bash
adc-[ADCID]/ 
└── study-[NAME]/ 
    └── ptid-[PTID]/ 
      └── visit-[DATE]/ 
          └── acquisition-[ID]/
              ├── manifest.json 
              ├── voice-recording-241.wav 
              ...
              └── voice-recording-735.wav 
```

where `[ADCID]` is the center ADCID, `[PTID]` is the ADRC assigned participant ID, and `[DATE]` is an [RFC3339](https://datatracker.ietf.org/doc/html/rfc3339) format date as described above.
The PTID could be swapped out for one of the other participant IDs discussed above.

### Tabular metadata

An alternative to the hierarchical organization is to put the metadata in tabular form, which references the data files.

```bash
adc-[ADCID]/ 
└── study-[NAME]/ 
    ├── manifest.csv 
    └── files/
        ├── voice-recording-241.wav 
        ...
        └── voice-recording-735.wav 
```

### Organization for tabular data

Alternatively, when the data is tabular, metadata may also be integrated as columns in the data file.

```bash
adc-[ADCID]/ 
└── participant-data-00129.csv
```

This should generally not be done if the file is exported from an instrument such as for biomarker analysis.
In this case, a separate manifest file should be used to avoid the metadata being deleted by exports from the instrument.

> Data that is capture using REDCap can be handled in different ways

## Data Submission

We expect most data submissions will be handled by upload to an AWS S3 bucket.
