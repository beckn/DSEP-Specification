

# Trainings and Courses on DSEP
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

### Example Catalog for Trainings and Courses

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
                "name": "BPP #1",
                "code": "bpp-1",
                "symbol": "<i class=\"fas fa-vihara\"></i>",
                "short_desc": "Omnis explicabo a eum corrupti.",
                "long_desc": "Blanditiis numquam esse reiciendis officiis. Quisquam dolorum sed voluptas libero facilis omnis quia odio non.",
                "images": [
                    "https://shikshalokam.org/wp-content/uploads/2021/06/elevate-logo.png"
                ],
                "audio": "string",
                "video": "string",
                "3d_render": "string"
            },
            "bpp/categories": [
                {
                    "id": "1",
                    "description": "Numquam quia inventore odit quis.",
                    "descriptor": {
                        "name": "Educational Management",
                        "code": "ID"
                    }
                },
                {
                    "id": "2",
                    "description": "Et accusantium odit omnis alias.",
                    "descriptor": {
                        "name": "Co-curricular Improvement",
                        "code": "IN"
                    }
                },
                {
                    "id": "3",
                    "description": "Sint nostrum reprehenderit repudiandae nihil.",
                    "descriptor": {
                        "name": "Educational Leadership",
                        "code": "UT"
                    }
                },
                {
                    "id": "4",
                    "description": "Dolorem ut suscipit in perferendis.",
                    "descriptor": {
                        "name": "Administrative Activities",
                        "code": "UT"
                    }
                }
            ],
            "providers": [
                {
                    "descriptor": {
                        "name": "ICSE"
                    },
                    "categories": [],
                    "items": [
                        {
                            "id": "1",
                            "descriptor": {
                                "name": "Grade X Computer Science",
                                "code": "X-COMPUTER-SCIENCE-ICSE",
                                "short_desc": "Molestiae minus illum ut deleniti.",
                                "long_desc": "Omnis veritatis vel. Qui illo omnis facere commodi.",
                                "images": [
                                    "https://loremflickr.com/640/480/abstract?random=4gysnfpakw",
                                    "https://loremflickr.com/640/480/abstract?random=7wpk9941yh",
                                    "https://loremflickr.com/640/480/abstract?random=tyj2svrvwx"
                                ]
                            },
                            "fulfillment_id": "1",
                            "price": {
                                "value": "0"
                            },
                            "tags": [
                                {
                                    "code" : "recommendations",
                                    "name" : "Recommended for",
                                    "list" : [
                                        {
                                            "code" : "occupation",
                                            "value" : "Teachers"
                                        },
                                        {
                                            "code" : "occupation",
                                            "value" : "Headmasters"
                                        },
                                        {
                                            "code" : "occupation",
                                            "value" : "PTA"
                                        },
                                        {
                                            "code" : "occupation",
                                            "value" : "Office Staffs"
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
                    "language": [
                        "hi",
                        "en"
                    ],
                    "descriptor": {
                        "name": "Full-time"
                    },
                    "tags": {
                        "status": "Live/Published",
                        "timeZone": ""
                    },
                    "agent": {
                        "name": "Mrs. Crystal Klocko",
                        "image": "https://cloudflare-ipfs.com/ipfs/Qmd3W5DuhgHirLHGVixi6V76LhCkZUz6pnFt5AJBiyvHye/avatar/386.jpg",
                        "gender": "F",
                        "tags": [
                            {
                                "code" : "subjects",
                                "name" : "Subjects",
                                "list" : [
                                    {
                                        "code" : "subject",
                                        "value" : "Mathematics"
                                    },
                                    {
                                        "code" : "grade",
                                        "value" : "X"
                                    },
                                    {
                                        "code" : "grade",
                                        "value" : "XI"
                                    },
                                    {
                                        "code" : "board",
                                        "value" : "CBSE"
                                    },
                                    {
                                        "code" : "board",
                                        "value" : "ICSE"
                                    }
                                ]
                            },
                            {
                                "code" : "instruction_mode",
                                "name" : "Mode of Instruction",
                                "list" : [
                                    {
                                        "code" : "lang",
                                        "value" : "English"
                                    },
                                    {
                                        "code" : "lang",
                                        "value" : "Hindi"
                                    }
                                ]
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

* Pramod Varma (EkStep)
* Chakshu Gautam (Affinidi)
* Sujith Nair (FIDE)
