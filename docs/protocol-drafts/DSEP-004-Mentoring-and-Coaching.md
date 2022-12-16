# Mentoring and Coaching on DSEP
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

#### Example Catalog

```
{
    "context": {
        "domain": "dsep:courses",
        "country": "IND",
        "city": "std:080",
        "action": "on_search",
        "bpp_id": "dev.elevate-apis.shikshalokam.org/bpp-1",
        "bpp_uri": "https://dev.elevate-apis.shikshalokam.org/bpp-1",
        "timestamp": "2022-12-14T05:23:03.443Z",
        "transaction_id": "ed5407ce-f7f9-42d3-8526-c36902e0762f",
        "message_id": "f445780f-eb7a-490d-a668-539ccf7b9c1d"
    },
    "message": {
        "catalog": {
            "descriptor": {
                "name": "Mentoring BPP"
            },
            "providers": [
                {
                    "descriptor": {
                        "name": "The Mentor Group"
                    },
                    "items": [
                        {
                            "id": "1",
                            "descriptor": {
                                "name": "Dr Asthana (MD)",
                                "short_desc": "A 1 hour session with Dr Asthana on Anatomy",
                                "images": [
                                    "https://loremflickr.com/640/480/abstract?random=4gysnfpakw"
                                ]
                            },
                            "fulfillment_id": "1",
                            "price": {
                                "value": "200",
                                "currency" : "INR"
                            },
                            "tags": [
                                {
                                    "code" : "info",
                                    "name" : "Session Information",
                                    "list" : [
                                        {
                                            "code" : "lang",
                                            "value" : "English"
                                        },
                                        {
                                            "code" : "duration",
                                            "value" : "60 min"
                                        },
                                        {
                                            "code" : "topic",
                                            "value" : "anatomy"
                                        },
                                        {
                                            "code" : "category",
                                            "value" : "Medicine"
                                        }
                                    ]
                                }
                            ],
                            "matched": true
                        }
                    ]
                }
            ],
            "fulfillments": [
                {
                    "id": "1",
                    "type": "ONLINE",
                    "agent": {
                        "name": "Dr. Abhinav Asthana",
                        "image": "https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHirLHGVixi6V76LhCkZUz6pnFt5AJBiyvHye/avatar/386.jpg",
                        "gender": "F",
                        "tags": [
                            {
                                "code" : "skills",
                                "name" : "Area of Expertise",
                                "list" : [
                                    {
                                        "code" : "topic",
                                        "value" : "Anatomy"
                                    },
                                    {
                                        "code" : "topic",
                                        "value" : "Urology"
                                    }
                                ]
                            },
                            {
                                "code" : "edu_qualifications",
                                "name" : "Educational Qualifications",
                                "list" : [
                                    {
                                        "code" : "degree",
                                        "value" : "MD"
                                    },
                                    {
                                        "code" : "institution",
                                        "value" : "John Hopkins Universoity"
                                    }
                                ]
                            },
                            {
                                "code" : "edu_qualifications",
                                "name" : "Educational Qualifications",
                                "list" : [
                                    {
                                        "code" : "degree",
                                        "value" : "MBBS"
                                    },
                                    {
                                        "code" : "institution",
                                        "value" : "AIIMS"
                                    }
                                ]
                            }
                        ],
                        "creds" : [
                            {
                                "url" : "<VC URL>",
                                "id" : ""
                            },
                            {
                                "url" : "<VC URL>",
                                "id" : ""
                            },
                            {
                                "url" : "<VC URL>",
                                "id" : ""
                            }
                        ]
                    },
                    "start": {
                        "time": {
                            "timestamp": "2022-12-21T17:01:40Z"
                        }
                    },
                    "end": {
                        "time": {
                            "timestamp": "2022-12-21T18:01:40Z"
                        }
                    }
                }
            ]
        }
    }
}
```

### Recommendations for BPPs

### Recommendations for BAPs

### Recommendations for NFOs

## Acknowledgements

The authors would like to thank the following people for their support and contributions to this document. 

* Pramod Varma (Beckn Foundation)
* Akash Shah (Shikshalokam)
* Joffin Joy (Shikshalokam)
* Sujith Nair (FIDE)
