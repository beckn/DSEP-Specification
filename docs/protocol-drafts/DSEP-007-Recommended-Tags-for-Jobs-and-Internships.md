
# Recommended Tags for Jobs and Internships
#### DSEP WG Working Draft - December 16, 2022

## Document Type

Protocol Draft

## Document Details
### This version
0.9


### Latest published version
[TODO]


### Latest editor's draft
[TODO]


### Implementation report
[TODO]


### Editors
Ravi Prakash (FIDE)


### Authors
Ravi Prakash (FIDE)


### Feedback

- Issues: TODO
- Discussions: TODO
- PRs: TODO


### Errata
No Errata exists as of now


## Context
[TODO]


## Problem

[TODO]


## Solution

### Tag Groups

DSEP enables the mapping of various attributes of **Jobs and Internships** to be represented using beckn protocol's generic schema. However, not all attributes are necessarily needed as named attributes in the core protocol, but merely for informational, display, or search optimization purposes.

**Tag Groups** allow various name-value pairs otherwise known as **Tags** to be grouped under a common heading. In the case of Jobs and Internships, every job and internship posting has various headings under which various details of a job can be listed. These Tags and Tag Groups can be used to index job catalogs on BPPs, and construct search filters on BAPs.

Some of the common Tag Groups listed as part of a Job / Internships Posting are listed below.

1. Educational Qualifications
2. Required Skills
3. Certifications
4. Experience
5. Work Environment
6. Benefits

The recommended code and name for each of the above tag groups are shown in the below table

| Name ( `TagGroup.name` )   | Code (`TagGroup.code`) | Description                                         |
|----------------------------|------------------------|-----------------------------------------------------|
| Educational Qualifications | education              | The Educational Qualifications Required for the Job |
| Required Skills            | skills                 | The skills required to perform this job             |
| Required Certifications    | certifications         | The certifications required to qualify for this job |
| Experience                 | experience             | The experience required to qualify for this job     |
| Work Environment           | work_env               | The work environment                                |
| Benefits                   | benefits               | Benefits of joining                                 |


Each of these Tag Groups have a list of standardized name-value pairs associated with them. It is recommended for all implementers of DSEP to use the same codes to preserve interoperability. 

### Enumerated Tags under **Educational Qualifications**

There are several tags that can be listed under educational qualifications. They are shown in the table below

| Name (`Tag.name` )    | Description                                                         | Code ( `Tag.code` ) | Type   | Validation | Allowed Values (Global Standard)                |
|-----------------------|---------------------------------------------------------------------|---------------------|--------|------------|-------------------------------------------------|
| Educational Degree    | The degree obtained by completing a school or a university course   | edu_degree          | string | ENUM       | btech,mtech,phd,postdoc,ba,ma,xiipass,xpass,mca |
| School Board          | The school board that issued the certificate                        | edu_board           | string | ENUM       | As specified by the local standards             |
| University            | The university the candidate got their higher education degree from | university          | string | ENUM       | As specified by the local standards             |
| Latest Academic Score | The academic score                                                  | score_latest        | string | Regex      | TODO                                              

### Enumerated Tags under **Required Skills**

### Enumerated Tags under **Certifications**

### Enumerated Tags under **Required Experience**

### Enumerated Tags under **Work Environment**

### Enumerated Tags under **Benefits**


The following Tag Groups

### Recommendations for BPPs

#### An Example Job Posting represented by the Item Schema

### Recommendations for BAPs

### Recommendations for NFOs

## Acknowledgements

The authors would like to thank the following people for their support and contributions to this document. 

* Pramod Varma (Beckn Foundation)
* Akash Shah (Shikshalokam)
* Joffin Joy (Shikshalokam)
* Sujith Nair (FIDE)
