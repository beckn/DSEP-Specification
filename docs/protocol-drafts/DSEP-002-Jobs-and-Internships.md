
# Jobs and Internships on DSEP
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

Issues: TODO
Discussions: TODO
PRs: TODO


### Errata
No Errata exists as of now


## Context
[TODO]


## Problem

[TODO]


## Solution

### Recommendations for BPPs

#### Example Job Catalog
```
{
    "context": {
        "action": "on_search",
        "bap_id": "phonepe.com",
        "bap_uri": "https://api.phonepe.com/dsep/v0/",
        "bpp_id": "affinidi.com",
        "bpp_uri": "http://DSEP-nlb-d3ed9a3f85596080.elb.ap-south-1.amazonaws.com",
        "city": "std:080",
        "core_version": "1.0.0",
        "country": "IND",
        "domain": "dsep:jobs",
        "message_id": "65aceace-aaf7-4087-b133-be6c0482e04a",
        "timestamp": "0001-01-01T00:00:00",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62195"
    },
    "message": {
        "catalog": {
            "descriptor": {
                "name": "Affindi"
            },
            "providers": [
                {
                    "id": "2",
                    "descriptor": {
                        "name": "Tata Consultancy Services"
                    },
                    "items": [
                        {
                            "descriptor": {
                                "name": "Android application Developer"
                            },
                            "id": "1",
                            "location_ids": [
                                "1",
                                "2"
                            ],
                            "tags": [
                                {
                                    "name": "Educational Qualifications",
                                    "code": "edu_qualifications",
                                    "list": [
                                        {
                                            "code": "edu_degree",
                                            "name": "Bachelor of Technology",
                                            "value": "btech"
                                        },
                                        {
                                            "code": "edu_degree",
                                            "name": "Master of Computer Applications",
                                            "value": "MCA"
                                        },
                                        {
                                            "code": "course",
                                            "name": "Computer Science",
                                            "value": "comp_sc"
                                        }
                                    ]
                                },
                                {
                                    "name": "Technical Skills",
                                    "code": "technical_skills",
                                    "list": [
                                        {
                                            "code": "programming_langs",
                                            "name": "Android",
                                            "value": "android"
                                        },
                                        {
                                            "code": "programming_langs",
                                            "name": "Flutter",
                                            "value": "flutter"
                                        },
                                    ]
                                },
                                {
                                    "name": "Soft Skills",
                                    "code": "soft_skills",
                                    "list": [
                                        {
                                            "code": "management",
                                            "name": "Managerial Skills",
                                            "value": "Leadership, Team Building"
                                        },
                                        {
                                            "code": "communication",
                                            "name": "Communication Skills",
                                            "value": "Fluency in English and Hindi"
                                        },
                                    ]
                                }
                            ]
                        },
                        {
                            "descriptor": {
                                "name": "iOS application Developer"
                            },
                            "id": "2",
                            "location_ids": [
                                "2"
                            ],
                            "tags": [
                                {
                                    "name": "Educational Qualifications",
                                    "code": "edu_qualifications",
                                    "list": [
                                        {
                                            "code": "edu_degree",
                                            "name": "Bachelor of Technology",
                                            "value": "btech"
                                        },
                                        {
                                            "code": "edu_degree",
                                            "name": "Master of Computer Applications",
                                            "value": "MCA"
                                        },
                                        {
                                            "code": "course",
                                            "name": "Computer Science",
                                            "value": "comp_sc"
                                        }
                                    ]
                                },
                                {
                                    "name": "Technical Skills",
                                    "code": "technical_skills",
                                    "list": [
                                        {
                                            "code": "programming_langs",
                                            "name": "Swift Programming",
                                            "value": "swift"
                                        },
                                        {
                                            "code": "programming_langs",
                                            "name": "Flutter",
                                            "value": "flutter"
                                        },
                                    ]
                                }
                            ]
                        }
                    ],
                    "locations": [
                        {
                            "id": "1",
                            "city": {
                                "name": "Pune"
                            }
                        },
                        {
                            "id": "2",
                            "city": {
                                "name": "Bangalore"
                            }
                        }
                    ]
                }
            ]
        }
    }
}
```

### Recommendations for BAPs

### 

## Acknowledgements

The authors would like to thank the following people for their support and contributions to this document. 

* Pramod Varma (Beckn Foundation)
* Giriraj Daga (Affinidi)
* Sanjay Kumar (Affinidi)
* Sujith Nair (FIDE)
