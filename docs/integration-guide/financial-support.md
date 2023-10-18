**Domain:** Financial Support

**Use case:** Vidyasarathi is a platform for applying to various scholarships provided by corporates and namma yatri as seeker platform will help the user to avail scholarships through its platform.

**Flow Diagram:**

![financial support flow diagram](../assets/financial-support-flow.jpeg?raw=true)

**Flow Explaination:** 

1. The driver sees the “Benefits” menu in the bottom tray of the app. He selects “Benefits” and can see various options on the page. This page has a section for *Insurance* and other benefits at the top, followed by the “Scholarships” section with different scholarships for children under it.
   1. **search API -** Request is made with filter like scholarship name, gender etc.
   1. **on\_search API -** Response with the list of scholarships matching filter criteria will be received.
1. Based on the selected scholarship, the screen will be populated with a form to collect the details, upload documents.
   1. **init API -** Request is made with order id, provider id, fulfillment id, customer details. After receiving init API, BPP will create the user in their system.
   1. **on\_init API -** Response is received with scholarship initiation details and form url details. 
      1. BAP will make a GET API call using the form url by attaching transaction id to it, to get the pre filled form. GET API url, authorization token details are fetched from the on\_search API.
         Ex: GET: http://path/to/form?transaction\_id: {{context.transaction\_id}} 
      1. BPP will validate the authorization token, gets the customer details using the transaction id and creates a prefilled form and sent to BAP as GET API response.
      1. BAP will render the form on the UI, collects details, does basic field validations and sends the details to BPP.
      1. BPP will validate the details and send the response.
      1. If there are more than one form to be filled, each form details are sent in a separate on\_inti API by BPP. This process is repeated until all the details are collected.
1. Final confirmation to apply for scholarship screen is displayed to the user.
   1. **confirm API -** Request is made with order id, provider id, fulfillment id, customer details.
   1. **on\_confirm API -**  Response with confirmation details is received.
1. User can fetch the scholarship processing status in the namma yatri application. When user clicks on ‘view status’, status is fetched and displayed in the application.
   1. **status API -** Request is made with order id.
   1. **on\_status API -** Response with scholarship processing status is received.

### Sample JSON schemas:
**search API:**
**Search by gender:**
```json
{
    "context": {
        "domain": "onest:financial-support",
        "location": {
            "city": {
                "name": "Bangalore",
                "code": "std:080"
            },
            "country": {
                "name": "India",
                "code": "IND"
            }
        },
        "action": "search",
        "version": "1.1.0",
        "bap_id": "vidyasaarathi.io",
        "bap_uri": "https://vidyasaarathi.io/",
        "transaction_id": "a9aaecca-10b7-4d19-b640-022723112309",
        "message_id": "a9aaecca-10b7-4d19-b640-b047a7c60009",
        "timestamp": "2023-02-06T09:55:41.161Z",
        "ttl": "PT10M"
    },
    "message": {
        "intent": {
            "fulfillment": {
                "customer": {
                    "person": {
                        "gender": "Female"
                    }
                }
            }
        }
    }
}
```
**Search by scholarship name:**
```json
{
    "context": {
        "domain": "onest:financial-support",
        "location": {
            "city": {
                "name": "Bangalore",
                "code": "std:080"
            },
            "country": {
                "name": "India",
                "code": "IND"
            }
        },
        "action": "search",
        "version": "1.1.0",
        "bap_id": "vidyasaarathi.io",
        "bap_uri": "https://vidyasaarathi.io/",
        "transaction_id": "a9aaecca-10b7-4d19-b640-022723112309",
        "message_id": "a9aaecca-10b7-4d19-b640-b047a7c60009",
        "timestamp": "2023-02-06T09:55:41.161Z",
        "ttl": "PT10M"
    },
   "message": {
      "intent": {
         "item": {
            "descriptor": {
               "name": "scholarship for undergraduate"
            }
         }
      }
   }
}
```
**Search by gender and name:**
```json
{
    "context": {
        "domain": "onest:financial-support",
        "location": {
            "city": {
                "name": "Bangalore",
                "code": "std:080"
            },
            "country": {
                "name": "India",
                "code": "IND"
            }
        },
        "action": "search",
        "version": "1.1.0",
        "bap_id": "vidyasaarathi.io",
        "bap_uri": "https://vidyasaarathi.io/",
        "transaction_id": "a9aaecca-10b7-4d19-b640-022723112309",
        "message_id": "a9aaecca-10b7-4d19-b640-b047a7c60009",
        "timestamp": "2023-02-06T09:55:41.161Z",
        "ttl": "PT10M"
    },
   "message": {
      "intent": {
         "provider": {
            "categories": [
               {
                  "descriptor": {
                     "code": "scholarship for undergraduate"
                  }
               }
            ]
         },
         "fulfillment": {
            "customer": {
               "person": {
                  "gender": "Female"
               }
            }
         }
      }
   }
}
```
**on\_search API:**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "on_search",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
   "message": {
      "catalog": {
         "descriptor": {
            "name": "Protean DSEP Scholarships and Grants BPP Platform"
         },
         "providers": [
            {
               "id": "BX213573733",
               "descriptor": {
                  "name": "XYZ Education Foundation",
                  "short_desc" : "Short Description about the Foundation",
                  "images": [
                     {
                        "url" : "url of the image of the provider"
                     }
                  ]
               },
               "categories": [
                  {
                     "id": "DSEP_CAT_1",
                     "descriptor": {
                        "code": "ug",
                        "name": "Under Graduate"
                     }
                  }
               ],
               "fulfillments": [
                  {
                     "id": "DSEP_FUL_63587501",
                     "type": "SCHOLARSHIP",
                     "tracking": false,
                     "contact": {
                        "phone": "9876543210",
                        "email": "maryg@xyz.com"
                     },
                     "stops": [
                        {
                           "type": "APPLICATION-START",
                           "time": {
                              "timestamp": "2022-09-01T00:00:00.000Z"
                           }
                        },
                        {
                           "type": "APPLICATION-END",
                           "time": {
                              "timestamp": "2022-10-31T00:00:00.000Z"
                           }
                        }
                     ]
                  }
               ],
               "items": [
                  {
                     "id": "SCM_63587501",
                     "descriptor": {
                        "name": "XYZ Education Scholarship for Undergraduate Students",
                        "long_desc": "XYZ Education Scholarship for Undergraduate Students"
                     },
                     "price": {
                        "currency": "INR",
                        "value": "Upto RS.1000 per year"
                     },
                     "rateable": false,
                     "tags": [
                        {
                           "display": true,
                           "descriptor": {
                              "code": "benefits",
                              "name": "Benefits"
                           },
                           "list": [
                              {
                                 "descriptor": {
                                    "code": "scholarship-amount",
                                    "name": "Scholarship Amount"
                                 },
                                 "value": "Upto Rs.25000 per year",
                                 "display": true
                              }
                           ]
                        }
                     ],
                     "category_ids": ["DSEP_CAT_1"],
                     "fulfillment_ids": ["DSEP_FUL_63587501"]
                  }
               ],
               "rateable": false
            }
         ]
      }
   }
}
```
**init API:**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "init",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
    "message": {
        "order": {
            "items": [
                {
                    "id": "SCM_63587501"
                }
            ],
            "provider": {
                "id": "BX213573733"
            },
            "billing": {
                "name": "Manjunath",
                "organization": {
                    "descriptor": {
                        "name": "Namma Yatri",
                        "code": "nammayatri.in"
                    },
                    "contact": {
                        "phone": "+91-8888888888",
                        "email": "scholarships@nammayatri.in"
                    }
                },
                "address": "No 27, XYZ Lane, etc",
                "phone": "+91-9999999999"
            },
            "fulfillments": [
                {
                    "customer": {
                        "id": "aadhaar:798677675565",
                        "person": {
                            "name": "Jane Doe",
                            "age": "13",
                            "gender": "female"
                        },
                        "contact": {
                            "phone": "+91-9123456789",
                            "email": "jane.doe@example.com"
                        }
                    }
                }
            ],
            "payment" : [
                {
                    "bank_code": "IFSC_Code_Of_the_bank",
                    "bank_account_number" :"121212121212",
                    "bank_account_name" : "Account Holder Name"
                }
            ]

        }
    }
}
```
**on\_init API:**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "on_init",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
    "message": {
        "order": {
            "provider": {
                "id": "471",
                "descriptor": {
                    "name": "XYZ Education Foundation",
                    "short_desc" : "Short Description about the Foundation",
                    "images": [
                        {
                            "url" : "url of the image of the provider"
                        }
                    ]
                },
                "rateable": false
            },
            "items": [
                {
                    "id": "SCM_63587501",
                    "descriptor": {
                        "name": "XYZ Education Scholarship for Undergraduate Students",
                        "long_desc": "XYZ Education Scholarship for Undergraduate Students"
                    },
                    "price": {
                        "currency": "INR",
                        "value": "Upto RS.1000 per year"
                    },
                    "xinput": {
                        "required": true,
                        "head": {
                            "descriptor": {
                                "name": "Application Form"
                            },
                            "index": {
                                "min": 0,
                                "cur": 0,
                                "max": 2
                            },
                            "headings": [
                                "Personal Details",
                                "Educational Details",
                                "Financial Information"
                            ]
                        },
                        "form": {
                            "mime_type": "text/html",
                            "url": "https://6vs8xnx5i7.vidyasaarathi.co.in/loans-kyc/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                            "resubmit": false,
                            "auth": {
                                "descriptor": {
                                    "code": "jwt"
                                },
                                "value": "eyJhbGciOiJIUzI.eyJzdWIiOiIxMjM0NTY3O.SflKxwRJSMeKKF2QT4"
                            }
                        }
                    },
                    "rateable": false,
                    "tags": [
                        {
                            "display": true,
                            "descriptor": {
                                "code": "eligibility-criteria",
                                "name": "Eligibility Criteria"
                            },
                            "list": [
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "SC",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "ST",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "OB",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "NT",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "gender_criteria",
                                        "name": "Gender Criteria"
                                    },
                                    "value": "ALL",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Thane",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Nagpur",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Yavatmal",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Ahmed Nagar",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Solapur",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Pune",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "state_criteria",
                                        "name": "State Criteria"
                                    },
                                    "value": "MAHARASHTRA",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "qualification_criteria",
                                        "name": "Qualification Criteria"
                                    },
                                    "value": "Class 10|Min. score=50|Max. score=null",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "qualification_criteria",
                                        "name": "Qualification Criteria"
                                    },
                                    "value": "Class 12|Min. score=60|Max. score=null",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "financial_income_criteria",
                                        "name": "Financial Income Criteria"
                                    },
                                    "value": "Max Family Income - Rs.500000.00",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "financial_year",
                                        "name": "Financial Year"
                                    },
                                    "value": "2023-2024",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "certificate_instructions",
                                        "name": "Certificate Instructions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                }
                            ]
                        },
                        {
                            "display": true,
                            "descriptor": {
                                "code": "additional_info",
                                "name": "Additional Info"
                            },
                            "list": [
                                {
                                    "descriptor": {
                                        "code": "faq",
                                        "name": "Frequently Asked Questions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "terms_conditions",
                                        "name": "Terms and Conditions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                }
                            ]
                        }
                    ],
                    "category_ids": [
                        "VSP_CAT_4"
                    ],
                    "fulfillment_ids": [
                        "VSP_FUL_1113"
                    ]
                }
            ],
            "billing": {
                "name": "Manjunath",
                "organization": {
                    "descriptor": {
                        "name": "Namma Yatri",
                        "code": "nammayatri.in"
                    },
                    "contact": {
                        "phone": "+91-8888888888",
                        "email": "scholarships@nammayatri.in"
                    }
                },
                "address": "No 27, XYZ Lane, etc",
                "phone": "+91-9999999999"
            },
            "fulfillments": [
                {
                    "state" : {
                        "descriptor" : {
                            "code": "APPLICATION-STARTED"
                        }
                    },
                    "id": "VSP_FUL_1113",
                    "type": "SCHOLARSHIP",
                    "tracking": false,
                    "contact": {
                        "phone": "9876543210",
                        "email": "maryg@xyz.com"
                    },
                    "customer": {
                        "id": "aadhaar:798677675565",
                        "person": {
                            "name": "Jane Doe",
                            "age": "13",
                            "gender": "female"
                        },
                        "contact": {
                            "phone": "+91-9663088848",
                            "email": "jane.doe@example.com"
                        }
                    },
                    "stops": [
                        {
                            "type": "APPLICATION-START",
                            "time": {
                                "timestamp": "2023-07-14T18:30:00.000Z"
                            }
                        },
                        {
                            "type": "APPLICATION-END",
                            "time": {
                                "timestamp": "2025-07-13T18:30:00.000Z"
                            }
                        }
                    ]
                }
            ],
            "payment" : [
                {
                    "bank_code": "IFSC_Code_Of_the_bank",
                    "bank_account_number" :"121212121212",
                    "bank_account_name" : "Account Holder Name"
                }
            ]
        }
    }
}
```
**confirm API**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "confirm",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
    "message": {
        "order": {
            "items": [
                {
                    "id": "SCM_63587501"
                }
            ],
            "provider": {
                "id": "BX213573733"
            },
            "billing": {
                "name": "Manjunath",
                "organization": {
                    "descriptor": {
                        "name": "Namma Yatri",
                        "code": "nammayatri.in"
                    },
                    "contact": {
                        "phone": "+91-8888888888",
                        "email": "scholarships@nammayatri.in"
                    }
                },
                "address": "No 27, XYZ Lane, etc",
                "phone": "+91-9999999999"
            },
            "fulfillments": [
                {
                    "customer": {
                        "id": "aadhaar:798677675565",
                        "person": {
                            "name": "Jane Doe",
                            "age": "13",
                            "gender": "female"
                        },
                        "contact": {
                            "phone": "+91-9663088848",
                            "email": "jane.doe@example.com"
                        }
                    }
                }
            ],
            "payment" : [
                {
                    "bank_code": "IFSC_Code_Of_the_bank",
                    "bank_account_number" :"121212121212",
                    "bank_account_name" : "Account Holder Name"
                }
            ]
        }
    }
}
```
1. **on\_confirm API:**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "on_confirm",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
    "message": {
        "order": {
            "id": "12424kh",
            "provider": {
                "id": "471",
                "descriptor": {
                    "name": "XYZ Education Foundation",
                    "short_desc" : "Short Description about the Foundation",
                    "images": [
                        {
                            "url" : "url of the image of the provider"
                        }
                    ]
                },
                "rateable": false
            },
            "items": [
                {
                    "id": "SCM_63587501",
                    "descriptor": {
                        "name": "XYZ Education Scholarship for Undergraduate Students",
                        "long_desc": "XYZ Education Scholarship for Undergraduate Students"
                    },
                    "price": {
                        "currency": "INR",
                        "value": "Upto RS.1000 per year"
                    },
                    "xinput": {
                        "required": true,
                        "head": {
                            "descriptor": {
                                "name": "Application Form"
                            },
                            "index": {
                                "min": 0,
                                "cur": 0,
                                "max": 2
                            },
                            "headings": [
                                "Personal Details",
                                "Educational Details",
                                "Financial Information"
                            ]
                        },
                        "form": {
                            "mime_type": "text/html",
                            "url": "https://6vs8xnx5i7.vidyasaarathi.co.in/loans-kyc/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                            "resubmit": false,
                            "auth": {
                                "descriptor": {
                                    "code": "jwt"
                                },
                                "value": "eyJhbGciOiJIUzI.eyJzdWIiOiIxMjM0NTY3O.SflKxwRJSMeKKF2QT4"
                            }
                        }
                    },
                    "rateable": false,
                    "tags": [
                        {
                            "display": true,
                            "descriptor": {
                                "code": "eligibility-criteria",
                                "name": "Eligibility Criteria"
                            },
                            "list": [
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "SC",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "ST",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "OB",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "NT",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "gender_criteria",
                                        "name": "Gender Criteria"
                                    },
                                    "value": "ALL",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Thane",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Nagpur",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Yavatmal",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Ahmed Nagar",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Solapur",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Pune",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "state_criteria",
                                        "name": "State Criteria"
                                    },
                                    "value": "MAHARASHTRA",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "qualification_criteria",
                                        "name": "Qualification Criteria"
                                    },
                                    "value": "Class 10|Min. score=50|Max. score=null",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "qualification_criteria",
                                        "name": "Qualification Criteria"
                                    },
                                    "value": "Class 12|Min. score=60|Max. score=null",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "financial_income_criteria",
                                        "name": "Financial Income Criteria"
                                    },
                                    "value": "Max Family Income - Rs.500000.00",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "financial_year",
                                        "name": "Financial Year"
                                    },
                                    "value": "2023-2024",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "certificate_instructions",
                                        "name": "Certificate Instructions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                }
                            ]
                        },
                        {
                            "display": true,
                            "descriptor": {
                                "code": "additional_info",
                                "name": "Additional Info"
                            },
                            "list": [
                                {
                                    "descriptor": {
                                        "code": "faq",
                                        "name": "Frequently Asked Questions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "terms_conditions",
                                        "name": "Terms and Conditions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                }
                            ]
                        }
                    ],
                    "category_ids": [
                        "VSP_CAT_4"
                    ],
                    "fulfillment_ids": [
                        "VSP_FUL_1113"
                    ]
                }
            ],
            "billing": {
                "name": "Manjunath",
                "organization": {
                    "descriptor": {
                        "name": "Namma Yatri",
                        "code": "nammayatri.in"
                    },
                    "contact": {
                        "phone": "+91-8888888888",
                        "email": "scholarships@nammayatri.in"
                    }
                },
                "address": "No 27, XYZ Lane, etc",
                "phone": "+91-9999999999"
            },
            "fulfillments": [
                {
                    "state" : {
                        "descriptor" : {
                            "code": "APPLICATION-SUBMITTED"
                        }
                    },
                    "id": "VSP_FUL_1113",
                    "type": "SCHOLARSHIP",
                    "tracking": false,
                    "contact": {
                        "phone": "9876543210",
                        "email": "maryg@xyz.com"
                    },
                    "customer": {
                        "id": "aadhaar:798677675565",
                        "person": {
                            "name": "Jane Doe",
                            "age": "13",
                            "gender": "female"
                        },
                        "contact": {
                            "phone": "+91-9663088848",
                            "email": "jane.doe@example.com"
                        }
                    },
                    "stops": [
                        {
                            "type": "APPLICATION-START",
                            "time": {
                                "timestamp": "2023-07-14T18:30:00.000Z"
                            }
                        },
                        {
                            "type": "APPLICATION-END",
                            "time": {
                                "timestamp": "2025-07-13T18:30:00.000Z"
                            }
                        }
                    ]
                }
            ],
            "docs": [
                {
                    "descriptor":{
                        "name" :"Application Details",
                        "short_desc" : "To open this document, enter the password sent to your email mayan****@***.com"
                    },
                    "url" : "https://link-to-the-document.com",
                    "mime_type" : "application/pdf"
                }
            ],
            "payment" : [
                {
                    "bank_code": "IFSC_Code_Of_the_bank",
                    "bank_account_number" :"121212121212",
                    "bank_account_name" : "Account Holder Name"
                }
            ]
        }
    }
}
```
**status API:**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "status",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
    "message": {
        "order_id" : "12424kh"
    }
}
```
**on\_status API:**
```json
{
   "context": {
      "domain": "onest:financial-support",
      "location": {
         "city": {
            "name": "Bangalore",
            "code": "std:080"
         },
         "country": {
            "name": "India",
            "code": "IND"
         }
      },
      "action": "on_status",
      "timestamp": "2023-08-02T07:21:58.448Z",
      "ttl": "PT10M",
      "version": "1.1.0",
      "bap_id": "vidyasaarathi.io",
      "bap_uri": "https://vidyasaarathi.io/",,
      "bpp_id": "nammayatri.io",
      "bpp_uri": "https://nammayatri.io/",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c60008",
      "message_id": "f6a7d7ea-a23e-4419-b07e-a3412fdffecf"
   },
    "message": {
        "order": {
            "id": "12424kh",
            "provider": {
                "id": "471",
                "descriptor": {
                    "name": "XYZ Education Foundation",
                    "short_desc" : "Short Description about the Foundation",
                    "images": [
                        {
                            "url" : "url of the image of the provider"
                        }
                    ]
                },
                "rateable": false
            },
            "items": [
                {
                    "id": "SCM_63587501",
                    "descriptor": {
                        "name": "XYZ Education Scholarship for Undergraduate Students",
                        "long_desc": "XYZ Education Scholarship for Undergraduate Students"
                    },
                    "price": {
                        "currency": "INR",
                        "value": "Upto RS.1000 per year"
                    },
                    "xinput": {
                        "required": true,
                        "head": {
                            "descriptor": {
                                "name": "Application Form"
                            },
                            "index": {
                                "min": 0,
                                "cur": 0,
                                "max": 2
                            },
                            "headings": [
                                "Personal Details",
                                "Educational Details",
                                "Financial Information"
                            ]
                        },
                        "form": {
                            "mime_type": "text/html",
                            "url": "https://6vs8xnx5i7.vidyasaarathi.co.in/loans-kyc/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                            "resubmit": false,
                            "auth": {
                                "descriptor": {
                                    "code": "jwt"
                                },
                                "value": "eyJhbGciOiJIUzI.eyJzdWIiOiIxMjM0NTY3O.SflKxwRJSMeKKF2QT4"
                            }
                        }
                    },
                    "rateable": false,
                    "tags": [
                        {
                            "display": true,
                            "descriptor": {
                                "code": "eligibility-criteria",
                                "name": "Eligibility Criteria"
                            },
                            "list": [
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "SC",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "ST",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "OB",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "course_category_criteria",
                                        "name": "Course Category Criteria"
                                    },
                                    "value": "NT",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "gender_criteria",
                                        "name": "Gender Criteria"
                                    },
                                    "value": "ALL",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Thane",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Nagpur",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Yavatmal",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Ahmed Nagar",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Solapur",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "city_criteria",
                                        "name": "City Criteria"
                                    },
                                    "value": "Pune",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "state_criteria",
                                        "name": "State Criteria"
                                    },
                                    "value": "MAHARASHTRA",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "qualification_criteria",
                                        "name": "Qualification Criteria"
                                    },
                                    "value": "Class 10|Min. score=50|Max. score=null",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "qualification_criteria",
                                        "name": "Qualification Criteria"
                                    },
                                    "value": "Class 12|Min. score=60|Max. score=null",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "financial_income_criteria",
                                        "name": "Financial Income Criteria"
                                    },
                                    "value": "Max Family Income - Rs.500000.00",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "financial_year",
                                        "name": "Financial Year"
                                    },
                                    "value": "2023-2024",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "certificate_instructions",
                                        "name": "Certificate Instructions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                }
                            ]
                        },
                        {
                            "display": true,
                            "descriptor": {
                                "code": "additional_info",
                                "name": "Additional Info"
                            },
                            "list": [
                                {
                                    "descriptor": {
                                        "code": "faq",
                                        "name": "Frequently Asked Questions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                },
                                {
                                    "descriptor": {
                                        "code": "terms_conditions",
                                        "name": "Terms and Conditions"
                                    },
                                    "value": "ZMT Education 15JUL23",
                                    "display": true
                                }
                            ]
                        }
                    ],
                    "category_ids": [
                        "VSP_CAT_4"
                    ],
                    "fulfillment_ids": [
                        "VSP_FUL_1113"
                    ]
                }
            ],
            "billing": {
                "name": "Manjunath",
                "organization": {
                    "descriptor": {
                        "name": "Namma Yatri",
                        "code": "nammayatri.in"
                    },
                    "contact": {
                        "phone": "+91-8888888888",
                        "email": "scholarships@nammayatri.in"
                    }
                },
                "address": "No 27, XYZ Lane, etc",
                "phone": "+91-9999999999"
            },
            "fulfillments": [
                {
                    "state" : {
                        "descriptor" : {
                            "code": "SCHOLARSHIP APPROVED"
                        }
                    },
                    "id": "VSP_FUL_1113",
                    "type": "SCHOLARSHIP",
                    "tracking": false,
                    "contact": {
                        "phone": "9876543210",
                        "email": "maryg@xyz.com"
                    },
                    "customer": {
                        "id": "aadhaar:798677675565",
                        "person": {
                            "name": "Jane Doe",
                            "age": "13",
                            "gender": "female"
                        },
                        "contact": {
                            "phone": "+91-9663088848",
                            "email": "jane.doe@example.com"
                        }
                    },
                    "stops": [
                        {
                            "type": "APPLICATION-START",
                            "time": {
                                "timestamp": "2023-07-14T18:30:00.000Z"
                            }
                        },
                        {
                            "type": "APPLICATION-END",
                            "time": {
                                "timestamp": "2025-07-13T18:30:00.000Z"
                            }
                        }
                    ]
                }
            ],
            "docs": [
                {
                    "descriptor":{
                        "name" :"Application Details",
                        "short_desc" : "To open this document, enter the password sent to your email mayan****@***.com"
                    },
                    "url" : "https://link-to-the-document.com",
                    "mime_type" : "application/pdf"
                }
            ],
            "payment" : [
                {
                    "bank_code": "IFSC_Code_Of_the_bank",
                    "bank_account_number" :"121212121212",
                    "bank_account_name" : "Account Holder Name"
                }
            ]
        }
    }
}
```
