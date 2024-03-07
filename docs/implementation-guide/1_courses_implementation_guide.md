# 1. Implementation Guide

This document contains the REQUIRED and RECOMMENDED standard functionality that must be implemented by any DSEP Service Provider Platform a.k.a BPPs and DSEP Consumer Platform a.k.a BAPs.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [RFC2119].

## 1.1 Discovery of Course

### 1.1.1 Recommendations for BPPs
The following recommendations need to be considered when implementing discovery functionality for a DSEP BPP

- REQUIRED. The BPP MUST implement the `search` endpoint to receive an `Intent` object sent by BAPs
- REQUIRED. The BPP MUST return a catalog of courses on the `on_search` callback endpoint specified in the `context.bap_uri` field of the `search` request body.
- REQUIRED. The BPP MUST map its courses to the `Item` schema.
- REQUIRED. Any Instructor( DSEP service provider) related information like name, logo, short description must be mapped to the `Provider.descriptor` schema
- REQUIRED. Any form that must be filled before receiving a quotation must be mapped to the `XInput` schema
- REQUIRED. If the platform wants to group its courses under a specific category, it must map each category to the `Category` schema
- REQUIRED. Any course delivery related information MUST be mapped to the `Fulfillment` schema.
- REQUIRED. If the BPP does not want to respond to a search request, it MUST return a `ack.status` value equal to `NACK`
- RECOMMENDED. Upon receiving a `search` request, the BPP SHOULD return a catalog that best matches the intent. This can be done by indexing the catalog against the various probable paths in the `Intent` schema relevant to typical DSEP use cases

### 1.1.2 Recommendations for BAPs
- REQUIRED. The BAP MUST call the `search` endpoint of the BG to discover multiple BPPs on a network
- REQUIRED. The BAP MUST implement the `on_search` endpoint to consume the `Catalog` objects containing courses sent by BPPs.
- REQUIRED. The BAP MUST expect multiple catalogs sent by the respective DSEP Providers on the network
- REQUIRED. The courses can be found in the `Catalog.providers[].items[]` array in the `on_search` request
- REQUIRED. If the `catalog.providers[].items[].xinput` object is present, then the BAP MUST redirect the user to, or natively render the form present on the link specified on the `items[].xinput.form.url` field.
- REQUIRED. If the `catalog.providers[].items[].xinput.required` field is set to `"true"` , then the BAP MUST NOT fire a `select`, `init` or `confirm` call until the form is submitted and a successful response is received
- RECOMMENDED. If the `catalog.providers[].items[].xinput.required` field is set to `"false"` , then the BAP SHOULD allow the user to skip filling the form

### Discovery Flow
1. A Learner log-in to the BAP and make a search request by course name, category, etc.
2. BAP translate the learner's search into /search request and hit the Gateway Endpoint of search.
3. Gateway broadcast the search request to different BPPs.
4. BPP translate the search intent and send the filtered catalogue to the BAP using /on_search request.
5. BAP render the result in the UI, so that it become visible to the learner.

### Discovery Example
A search request for a course  may look like this
```
{
  "context": {
    "domain": "onest:courses",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "action": "search",
    "timestamp": "2022-12-15T05:23:03.443Z",
    "version": "1.1.0",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bap_id": "onest.becknprotocol.io",
    "ttl": "PT10M"
  },
  "message": {
    "intent": {
      "item": {
        "descriptor": {
          "name": "english"
        }
      }
    }
  }
}
```

An example catalog of courses may look like this
```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_search",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "7db91181-718f-4720-83de-88e36e9f854e",
    "ttl": "PT10M",
    "timestamp": "2023-02-22T10:30:18.145Z",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io"
  },
  "message": {
    "catalog": {
      "descriptor": {
        "name": "Catalog for English courses"
      },
      "providers": [
        {
          "id": "INFOSYS",
          "descriptor": {
            "name": "Infosys Springboard",
            "short_desc": "Infosys Springboard Digital literacy program",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
                "size_type": "sm"
              }
            ]
          },
          "categories": [
            {
              "id": "LANGUAGE-COURSES",
              "descriptor": {
                "code": "LANGUAGE-COURSES",
                "name": "Language Courses"
              }
            },
            {
              "id": "SKILL-DEVELOPMENT-COURSES",
              "descriptor": {
                "code": "SKILL-DEVELOPMENT-COURSES",
                "name": "Skill development Courses"
              }
            },
            {
              "id": "TECHNICAL-COURSES",
              "descriptor": {
                "code": "TECHNICAL-COURSES",
                "name": "Technical Courses"
              }
            }
          ],
          "items": [
            {
              "id": "d4975df5-b18c-4772-80ad-368669856d52",
              "quantity": {
                "maximum": 1
              },
              "descriptor": {
                "name": "Everyday Conversational English",
                "long_desc": "Everyday Conversational English",
                "images": [
                  {
                    "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
                  }
                ],
                "media": [
                  {
                    "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
                  }
                ]
              },
              "price": {
                "currency": "INR",
                "value": "0"
              },
              "category_ids": [
                "LANGUAGE-COURSES"
              ],
              "rating": "4.5",
              "rateable": true
            },
            {
              "id": "58449592-c4f2-4971-8e23-1da2515042c2",
              "quantity": {
                "maximum": 1
              },
              "descriptor": {
                "name": "English Grammar and Usage",
                "long_desc": "English Grammar and Usage",
                "images": [
                  {
                    "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/english-grammar-and-usage.png"
                  }
                ],
                "media": [
                  {
                    "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/english-grammar-and-usage/preview/"
                  }
                ]
              },
              "price": {
                "currency": "INR",
                "value": "0"
              },
              "category_ids": [
                "LANGUAGE-COURSES"
              ],
              "rating": "4.2",
              "rateable": true
            }
          ],
          "fulfillments": [
            {
              "agent": {
                "person": {
                  "id": "eng-01-prof",
                  "name": "Prof. Shipra Vaidya"
                }
              }
            }
          ]
        }
      ]
    }
  }
}
```

## 1.2 Application for Course Enrollment 
This section provides recommendations for implementing the APIs related to applying for a course.

### 1.2.1 Recommendations for BPPs

#### 1.2.1.1 Selecting a course from the catalog
- REQUIRED. The BPP MUST implement the `select` endpoint on the url specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST check for a form submission at the URL specified on the `xinput.form.url` before acknowledging a `select` request.
- REQUIRED. If the DSEP provider has successfully received the information submitted by the DSEP consumer, the BPP must return an acknowledgement with `ack.status` set to `ACK` in response to the `select` request
- REQUIRED. If the DSEP provider has returned a successful acknowledgement to a `select` request, it MUST send the offer encapsulated in an `Order` object

#### 1.2.1.2 Initializing an enrolment for a course
- REQUIRED. The BPP MUST implement the `init` endpoint on the url specified in  the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.2.1.3 Confirming the enrolment for a course
- REQUIRED. The BPP MUST implement the `confirm` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 1.2.2 Recommendations for BAPs

#### 1.2.1.1 Selecting a course from the catalog
- REQUIRED. The BAP user MUST submit the form on the url received from  `on_search`  under `xinput.form.url`.
- REQUIRED. The BAP MUST implement the `on_select` endpoint on the url specified in the `context.bap_uri` field sent during `select`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.2.2.2  Initializing an enrolment for a course
- REQUIRED. The BAP MUST hit the `init` endpoint on the url specified in  the `context.bpp_uri` field sent during `on_search`. 
- REQUIRED. The BAP MUST implement the `on_init` endpoint on the url specified in  the `context.bap_uri` field sent during `init`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.2.2.3 Confirming the enrolment for a course
- REQUIRED. The BAP MUST hit the `confirm` endpoint on the url specified in  the `context.bpp_uri` field sent during `on_search`. 
- REQUIRED. The BAP MUST implement the `on_confirm` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `confirm`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 1.2.3 Example Workflow
1. Learner select a course from the catalog rendered from the search results.
2. BAPs send /select call to the BPP with the course id.
3. BPP return the complete course information in /on_select.
4. BAP renders the details in the Course detail page.
5. Learner start the enrolment process by providing billing details, application detail.
6. BAP send /init call to the BPP.
7. BPP send /on_init with the payment confirmation.
8. Learner confirm the application
9. BAP send /confirm call to the BPP.
10. BPP send /on_confirm with the course enrolment confirmation.

### 1.2.3 Example Requests

Below is an example of a `select` request
```
{
  "context": {
    "domain": "onest:courses",
    "action": "select",
    "version": "1.1.0",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "timestamp": "2022-12-12T09:55:41.161Z",
    "ttl": "PT10M",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io"
  },
  "message": {
    "order": {
      "provider": {
        "id": "INFOSYS"
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52"
        }
      ]
    }
  }
}
```

Below is an example of an `on_select` callback
```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_select",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
    "ttl": "PT10M",
    "timestamp": "2023-02-20T15:21:36.925Z"
  },
  "message": {
    "order": {
      "provider": {
        "id": "INFOSYS",
        "descriptor": {
          "name": "Infosys Springboard",
          "short_desc": "Infosys Springboard Digital literacy program",
          "images": [
            {
              "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
              "size_type": "sm"
            }
          ]
        },
        "categories": [
          {
            "id": "LANGUAGE-COURSES",
            "descriptor": {
              "code": "LANGUAGE-COURSES",
              "name": "Language Courses"
            }
          },
          {
            "id": "SKILL-DEVELOPMENT-COURSES",
            "descriptor": {
              "code": "SKILL-DEVELOPMENT-COURSES",
              "name": "Skill development Courses"
            }
          },
          {
            "id": "TECHNICAL-COURSES",
            "descriptor": {
              "code": "TECHNICAL-COURSES",
              "name": "Technical Courses"
            }
          }
        ]
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52",
          "quantity": {
            "maximum": 1
          },
          "descriptor": {
            "name": "Everyday Conversational English",
            "long_desc": "Everyday Conversational English",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
              }
            ]
          },
          "price": {
            "currency": "INR",
            "value": "0"
          },
          "category_ids": [
            "LANGUAGE-COURSES"
          ],
          "rating": "4.5",
          "tags": [
            {
              "descriptor": {
                "code": "course-highlights",
                "name": "Course Highlights"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-1"
                  },
                  "value": "Focusing on clear pronunciation and reducing any strong accents that may impede communication."
                },
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-2"
                  },
                  "value": "Expanding everyday vocabulary to facilitate common conversations."
                }
              ],
              "display": true
            },
            {
              "descriptor": {
                "code": "course-prerequisites",
                "name": "Course Prerequisites"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-1"
                  },
                  "value": "Should have a basic understanding of English"
                },
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-2"
                  },
                  "value": "Access to a computer or internet to access the course online"
                }
              ],
              "display": true
            }
          ],
          "rateable": true
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "0"
        }
      },
      "fulfillments": [
        {
          "agent": {
            "person": {
              "id": "eng-01-prof",
              "name": "Prof. Shipra Vaidya"
            }
          }
        }
      ],
      "type": "DEFAULT"
    }
  }
}
```


Below is an example of a `init` request
```
{
  "context": {
    "domain": "onest:courses",
    "action": "init",
    "version": "1.1.0",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "timestamp": "2022-12-12T09:55:41.161Z",
    "ttl": "PT10M",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io"
  },
  "message": {
    "order": {
      "provider": {
        "id": "INFOSYS"
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52"
        }
      ],
      "billing": {
        "name": "Namma Yatri",
        "organization": {
          "address": "Girija Building, Number 817, Ganapathi Temple Rd",
          "city": "Bengaluru",
          "state": "Karnataka",
          "contact": {
            "email": "nammayatri.support@juspay.in",
            "phone": "+91 80 68501060"
          }
        }
      },
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "Manjunath"
            },
            "contact": {
              "phone": "+91 9988764321",
              "email": "manjunath@gmail.com"
            }
          }
        }
      ]
    }
  }
}
```

Below is an example of an `on_init` callback
```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_init",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
    "ttl": "PT10M",
    "timestamp": "2023-02-20T15:21:36.925Z"
  },
  "message": {
    "order": {
      "provider": {
        "id": "INFOSYS",
        "descriptor": {
          "name": "Infosys Springboard",
          "short_desc": "Infosys Springboard Digital literacy program",
          "images": [
            {
              "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
              "size_type": "sm"
            }
          ]
        }
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52",
          "quantity": {
            "maximum": 1
          },
          "descriptor": {
            "name": "Everyday Conversational English",
            "long_desc": "Everyday Conversational English",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
              }
            ]
          },
          "price": {
            "currency": "INR",
            "value": "0"
          },
          "category_ids": [
            "LANGUAGE-COURSES"
          ],
          "rating": "4.5",
          "tags": [
            {
              "descriptor": {
                "code": "course-highlights",
                "name": "Course Highlights"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-1"
                  },
                  "value": "Focusing on clear pronunciation and reducing any strong accents that may impede communication."
                },
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-2"
                  },
                  "value": "Expanding everyday vocabulary to facilitate common conversations."
                }
              ],
              "display": true
            },
            {
              "descriptor": {
                "code": "course-prerequisites",
                "name": "Course Prerequisites"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-1"
                  },
                  "value": "Should have a basic understanding of English"
                },
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-2"
                  },
                  "value": "Access to a computer or internet to access the course online"
                }
              ],
              "display": true
            }
          ],
          "rateable": true
        }
      ],
      "billing": {
        "name": "Namma Yatri",
        "organization": {
          "address": "Girija Building, Number 817, Ganapathi Temple Rd",
          "city": "Bengaluru",
          "state": "Karnataka",
          "contact": {
            "email": "nammayatri.support@juspay.in",
            "phone": "+91 80 68501060"
          }
        }
      },
      "quote": {
        "price": {
          "currency": "INR",
          "value": "0"
        }
      },
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "Manjunath"
            },
            "contact": {
              "phone": "+91 9988764321",
              "email": "manjunath@gmail.com"
            }
          },
          "agent": {
            "person": {
              "id": "eng-01-prof",
              "name": "Prof. Shipra Vaidya"
            }
          },
          "state": {
            "descriptor": {
              "code": "order-initialized",
              "name": "Order Initialized"
            }
          }
        }
      ],
      "payments": [
        {
          "params": {
            "amount": "0",
            "currency": "INR"
          },
          "status": "PAID"
        }
      ],
      "type": "DEFAULT"
    }
  }
}
```

Below is an example of a `confirm` request
```
{
  "context": {
    "domain": "onest:courses",
    "action": "confirm",
    "version": "1.1.0",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "timestamp": "2022-12-12T09:55:41.161Z",
    "ttl": "PT10M",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io"
  },
  "message": {
    "order": {
      "provider": {
        "id": "INFOSYS"
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52"
        }
      ],
      "billing": {
        "name": "Namma Yatri",
        "organization": {
          "address": "Girija Building, Number 817, Ganapathi Temple Rd",
          "city": "Bengaluru",
          "state": "Karnataka",
          "contact": {
            "email": "nammayatri.support@juspay.in",
            "phone": "+91 80 68501060"
          }
        }
      },
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "Manjunath"
            },
            "contact": {
              "phone": "+91 9988764321",
              "email": "manjunath@gmail.com"
            }
          }
        }
      ],
      "payments": [
        {
          "params": {
            "amount": "0",
            "currency": "INR"
          },
          "status": "PAID"
        }
      ]
    }
  }
}
```
Below is an example of an `on_confirm` callback
```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_confirm",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
    "ttl": "PT10M",
    "timestamp": "2023-02-20T15:21:36.925Z"
  },
  "message": {
    "order": {
      "id": "d4975df5",
      "provider": {
        "id": "INFOSYS",
        "descriptor": {
          "name": "Infosys Springboard",
          "short_desc": "Infosys Springboard Digital literacy program",
          "images": [
            {
              "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
              "size_type": "sm"
            }
          ]
        }
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52",
          "quantity": {
            "maximum": 1
          },
          "descriptor": {
            "name": "Everyday Conversational English",
            "long_desc": "Everyday Conversational English",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
              }
            ]
          },
          "price": {
            "currency": "INR",
            "value": "0"
          },
          "category_ids": [
            "LANGUAGE-COURSES"
          ],
          "rating": "4.5",
          "tags": [
            {
              "descriptor": {
                "code": "course-highlights",
                "name": "Course Highlights"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-1"
                  },
                  "value": "Focusing on clear pronunciation and reducing any strong accents that may impede communication."
                },
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-2"
                  },
                  "value": "Expanding everyday vocabulary to facilitate common conversations."
                }
              ],
              "display": true
            },
            {
              "descriptor": {
                "code": "course-prerequisites",
                "name": "Course Prerequisites"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-1"
                  },
                  "value": "Should have a basic understanding of English"
                },
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-2"
                  },
                  "value": "Access to a computer or internet to access the course online"
                }
              ],
              "display": true
            }
          ],
          "rateable": true
        },
        {
          "id": "c9461edd-628d-46f3-820e-bab42b57d143",
          "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
          "descriptor": {
            "name": "Everyday Conversational English - Chapter 1",
            "long_desc": "Everyday Conversational English - Chapter 1",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch1.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch1/"
              }
            ]
          }
        },
        {
          "id": "77223dc6-f6e4-48dd-bf0e-1e43841e651c",
          "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
          "descriptor": {
            "name": "Everyday Conversational English - Chapter 2",
            "long_desc": "Everyday Conversational English - Chapter 2",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch2.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch2/"
              }
            ]
          }
        },
        {
          "id": "eae312ed-5a2a-4b95-bed8-407a832b11b8",
          "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
          "descriptor": {
            "name": "Everyday Conversational English - Chapter 3",
            "long_desc": "Everyday Conversational English - Chapter 3",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch3.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch3/"
              }
            ]
          }
        }
      ],
      "billing": {
        "name": "Namma Yatri",
        "organization": {
          "address": "Girija Building, Number 817, Ganapathi Temple Rd",
          "city": "Bengaluru",
          "state": "Karnataka",
          "contact": {
            "email": "nammayatri.support@juspay.in",
            "phone": "+91 80 68501060"
          }
        }
      },
      "quote": {
        "price": {
          "currency": "INR",
          "value": "0"
        }
      },
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "Manjunath"
            },
            "contact": {
              "phone": "+91 9988764321",
              "email": "manjunath@gmail.com"
            }
          },
          "agent": {
            "person": {
              "id": "eng-01-prof",
              "name": "Prof. Shipra Vaidya"
            }
          },
          "state": {
            "descriptor": {
              "code": "order-confirmed",
              "name": "Order Confirmed"
            }
          }
        }
      ],
      "payments": [
        {
          "params": {
            "amount": "0",
            "currency": "INR"
          },
          "status": "PAID"
        }
      ],
      "type": "DEFAULT"
    }
  }
}
```

## 1.3 Fulfillment of courses
This section contains recommendations for implementing the APIs related to delivering courses.

### 1.3.1 Recommendations for BPPs

#### 1.3.1.1 Sending status updates
- REQUIRED. The BPP MUST implement the `status` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.3.1.2 Updating an enrolment for Courses
- REQUIRED. The BPP MUST implement the `update` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.3.1.3 Cancelling an enrolment for Course
- REQUIRED. The BPP MUST implement the `cancel` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST implement the `get_cancellation_reasons` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.3.1.4 Real-time tracking
- REQUIRED. The BPP MUST implement the `track` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 1.3.2 Recommendations for BAPs

#### 1.3.2.1 Receiving status updates
- REQUIRED. The BAP MUST implement the `on_status` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `status`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.3.2.2 Updating an enrolment for Courses
- REQUIRED. The BAP MUST implement the `on_update` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `update`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.3.2.3 Cancelling an enrolment for Courses
- REQUIRED. The BAP MUST implement the `on_cancel` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `cancel`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BAP MUST implement the `cancellation_reasons` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `get_cancellation_reasons`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.3.2.4 Real-time tracking
- REQUIRED. The BAP MUST implement the `on_track` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `track`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 1.3.3 Example Workflow
### 1.3.3.1 Status Workflow
1. The BAP receives new updates on the course enrolment from unsolicted /on_status call.
2. The Learner requested for a status update.
3. The BAP calls /status endpoint of the BPP.
4. The BPP provides the updated course enrolment information in /on_status call.

### 1.3.3.2 Update Workflow
1. The Learner choose to update the course enrolment information ( For eg. Applicant details, billing address or course completion status).
2. The BAP calls /update endpoint of the BPP.
3. The BPP update the course enrolment information and return updated object in /on_update call.

### 1.3.3.3 Cancel Workflow
1. The Learner choose to cancel the course enrolment.
2. The BAP call /get_cancellation_reason endpoint of the BPP.
3. The BPP respond with cancellation reasons using /cancellation_reason endpoint.
4. The BAP renders the reasons and learner chooses one and proceed with cancellation.
5. The BAP calls /cancel endpoint of the BPP.
6. The BPP cancels the course enrolment and sends cancellation confirmation in /on_cancel endpoint.

### 1.3.3.4 Tracking Workflow
1. The learner requested to track the completion of the course
2. The BAP calls /track endpoint of the BPP.
3. The BPP respond with the tracking url using /on_track endpoint.
4. The BAP render the tracking url in the UI.
5. The learner is able to track the course completion.

### 1.3.4 Example Requests

Below is an example of a `status` request
```
{
  "context": {
    "domain": "onest:courses",
    "action": "status",
    "version": "1.1.0",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "$bb579fb8-cb82-4824-be12-fcbc405b6608",
    "timestamp": "2022-12-12T09:55:41.161Z",
    "ttl": "PT10M",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io"
  },
  "message": {
    "order_id": "d4975df5"
  }
}
```

Below is an example of an `on_status` callback
```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_status",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
    "ttl": "PT10M",
    "timestamp": "2023-02-20T15:21:36.925Z"
  },
  "message": {
    "order": {
      "id": "d4975df5",
      "provider": {
        "id": "INFOSYS",
        "descriptor": {
          "name": "Infosys Springboard",
          "short_desc": "Infosys Springboard Digital literacy program",
          "images": [
            {
              "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
              "size_type": "sm"
            }
          ]
        }
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52",
          "quantity": {
            "maximum": 1
          },
          "descriptor": {
            "name": "Everyday Conversational English",
            "long_desc": "Everyday Conversational English",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
              }
            ]
          },
          "price": {
            "currency": "INR",
            "value": "0"
          },
          "category_ids": [
            "LANGUAGE-COURSES"
          ],
          "rating": "4.5",
          "tags": [
            {
              "descriptor": {
                "code": "course-highlights",
                "name": "Course Highlights"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-1"
                  },
                  "value": "Focusing on clear pronunciation and reducing any strong accents that may impede communication."
                },
                {
                  "descriptor": {
                    "code": "highlight",
                    "name": "highlight-2"
                  },
                  "value": "Expanding everyday vocabulary to facilitate common conversations."
                }
              ],
              "display": true
            },
            {
              "descriptor": {
                "code": "course-prerequisites",
                "name": "Course Prerequisites"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-1"
                  },
                  "value": "Should have a basic understanding of English"
                },
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "prerequisite-2"
                  },
                  "value": "Access to a computer or internet to access the course online"
                }
              ],
              "display": true
            }
          ],
          "rateable": true
        },
        {
          "id": "c9461edd-628d-46f3-820e-bab42b57d143",
          "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
          "descriptor": {
            "name": "Everyday Conversational English - Chapter 1",
            "long_desc": "Everyday Conversational English - Chapter 1",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch1.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch1/"
              }
            ]
          }
        },
        {
          "id": "77223dc6-f6e4-48dd-bf0e-1e43841e651c",
          "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
          "descriptor": {
            "name": "Everyday Conversational English - Chapter 2",
            "long_desc": "Everyday Conversational English - Chapter 2",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch2.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch2/"
              }
            ]
          }
        },
        {
          "id": "eae312ed-5a2a-4b95-bed8-407a832b11b8",
          "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
          "descriptor": {
            "name": "Everyday Conversational English - Chapter 3",
            "long_desc": "Everyday Conversational English - Chapter 3",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch3.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch3/"
              }
            ]
          }
        }
      ],
      "billing": {
        "name": "Namma Yatri",
        "organization": {
          "address": "Girija Building, Number 817, Ganapathi Temple Rd",
          "city": "Bengaluru",
          "state": "Karnataka",
          "contact": {
            "email": "nammayatri.support@juspay.in",
            "phone": "+91 80 68501060"
          }
        }
      },
      "quote": {
        "price": {
          "currency": "INR",
          "value": "0"
        }
      },
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "Manjunath"
            },
            "contact": {
              "phone": "+91 9988764321",
              "email": "manjunath@gmail.com"
            }
          },
          "agent": {
            "person": {
              "id": "eng-01-prof",
              "name": "Prof. Shipra Vaidya"
            }
          },
          "state": {
            "descriptor": {
              "code": "course-completed",
              "name": "Course completed",
              "short_desc": "The course Everyday Conversational English has been completed"
            }
          }
        }
      ],
      "payments": [
        {
          "params": {
            "amount": "0",
            "currency": "INR"
          },
          "status": "PAID"
        }
      ],
      "type": "DEFAULT"
    }
  }
}
```

Below is an example of a `update` request
```
{
    "context": {
        "domain": "onest:learning-experiences",
        "version": "1.1.0",
        "action": "update",
        "bap_id": "onest.becknprotocol.io",
        "bap_uri": "https://onest-network.becknprotocol.io/",
        "bpp_id": "infosys.springboard.io",
        "bpp_uri": "https://infosys.springboard.io",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
        "ttl": "PT10M",
        "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "order": {
          "id": "12424kh",
          "items": [
            {
              "id": "d4975df5-b18c-4772-80ad-368669856d52",
              "fulfillments": [
                {
                  "state" : {
                    "descriptor" : {
                        "code": "COMPLETED",
                        "name" : "COMPLETED"
                    },
                    "updated_at" : "2023-02-06T09:55:41.161Z"
                  }
                }
            ]
            }
          ]
        },
        "update_target": "order.items[0].fulfillments[0].state"
    }
}
```
Below is an example of a `on_update` callback

```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_update",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
    "ttl": "PT10M",
    "timestamp": "2023-02-20T15:21:36.925Z"
  },
  "message": {
    "order": {
      "id": "12424kh",
      "provider": {
        "id": "INFOSYS",
        "descriptor": {
          "name": "Infosys Springboard",
          "short_desc": "Infosys Springboard Digital literacy program",
          "images": [
            {
              "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
              "size_type": "sm"
            }
          ]
        },
        "categories": [
          {
            "id": "LANGUAGE-COURSES",
            "descriptor": {
              "code": "LANGUAGE-COURSES",
              "name": "Language Courses"
            }
          },
          {
            "id": "SKILL-DEVELOPMENT-COURSES",
            "descriptor": {
              "code": "SKILL-DEVELOPMENT-COURSES",
              "name": "Skill development Courses"
            }
          },
          {
            "id": "TECHNICAL-COURSES",
            "descriptor": {
              "code": "TECHNICAL-COURSES",
              "name": "Technical Courses"
            }
          },
          {
            "id": "SELF-PACED-COURSES",
            "descriptor": {
              "code": "SELF-PACED-COURSES",
              "name": "Self Paced Courses"
            }
          }
        ]
      },
      "items": [
        {
          "id": "d4975df5-b18c-4772-80ad-368669856d52",
          "quantity": {
            "maximum": 1
          },
          "descriptor": {
            "name": "Everyday Conversational English",
            "short_desc": "Elevate your daily conversations with confidence through our 'Everyday Conversational English' course.",
            "long_desc": "<p><strong>Course Overview:</strong><br>Welcome to 'Everyday Conversational English,' your key to mastering essential language skills for real-life communication. Tailored for all levels, this course offers:</p><ol><li><strong>Practical Vocabulary:</strong><br>Learn everyday expressions for seamless communication.</li><li><strong>Interactive Role-Playing:</strong><br>Apply knowledge through immersive exercises for real-world scenarios.</li><li><strong>Cultural Insights:</strong><br>Gain cultural nuances to connect authentically in conversations.</li><li><strong>Real-Life Scenarios:</strong><br>Navigate common situations with confidence-building tools.</li><li><strong>Quiz Assessments:</strong><br>Reinforce learning through quizzes for ongoing skill development.</li></ol><p><strong>Why Take This Course:</strong></p><ul><li><strong>Personal & Professional Growth:</strong><br>Enhance personal connections and gain a professional edge.</li><li><strong>Cultural Fluency:</strong><br>Understand and engage with diverse cultures confidently.</li><li><strong>Life-Long Skill:</strong><br>Develop a valuable skill applicable across various life stages.</li></ul><p>Join 'Everyday Conversational English' and elevate your communication for meaningful connections and success.</p>",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
              }
            ],
            "media": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
              }
            ]
          },
          "creator": {
            "descriptor": {
              "name": "Prof. Emma Sullivan",
              "short_desc": "Experienced language educator dedicated to fostering practical conversational skills and cultural fluency",
              "long_desc": "Hello, I'm Prof. Emma Sullivan, your guide in 'Everyday Conversational English.' With over a decade of experience, I'm here to make language learning dynamic and culturally enriching. Let's explore practical communication skills together for personal and professional growth. Join me on this exciting journey!",
              "images": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/ins/1.png"
                }
              ]
            }
          },
          "price": {
            "currency": "INR",
            "value": "150"
          },
          "category_ids": [
            "LANGUAGE-COURSES",
            "SELF-PACED-COURSES"
          ],
          "rating": "4.5",
          "rateable": true,
          "add-ons": [
            {
              "id": "course-outline",
              "descriptor": {
                "name": "Course Outline",
                "long_desc": "Outline for the course",
                "media": [
                  {
                    "mimetype": "application/pdf",
                    "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/outline.pdf"
                  }
                ]
              }
            },
            {
              "id": "prelim-quiz",
              "descriptor": {
                "name": "Preliminary Quiz",
                "long_desc": "Take this preliminary quiz to see if you will benefit from the course!",
                "media": [
                  {
                    "mimetype": "text/html",
                    "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/prelim-quiz"
                  }
                ]
              }
            }
          ],
          "tags": [
            {
              "descriptor": {
                "code": "content-metadata",
                "name": "Content metadata"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "learner-level",
                    "name": "Learner level"
                  },
                  "value": "Beginner"
                },
                {
                  "descriptor": {
                    "code": "learning-objective",
                    "name": "Learning objective"
                  },
                  "value": "By the end of the course, learners will confidently navigate everyday conversations, demonstrating improved fluency, cultural awareness, and effective communication skills."
                },
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "Prerequisite"
                  },
                  "value": "Should have a basic understanding of English"
                },
                {
                  "descriptor": {
                    "code": "prerequisite",
                    "name": "Prerequisite"
                  },
                  "value": "Access to a computer or internet to access the course online"
                },
                {
                  "descriptor": {
                    "code": "lang-code",
                    "name": "Language code"
                  },
                  "value": "en"
                },
                {
                  "descriptor": {
                    "code": "course-duration",
                    "name": "Course duration"
                  },
                  "value": "P20H"
                }
              ],
              "display": true
            }
          ]
        }
      ],
      "fulfillments": [
        { 
          "state" : {
            "descriptor" : {
                "code": "COMPLETED",
                "name" : "Completed"
            },
            "updated_at" : "2023-02-06T09:55:41.161Z"
          },
          "agent": {
            "person": {
              "name": "Infosys Springboard"
            },
            "contact": {
              "email": "support@infy.com"
            }
          },
          "customer": {
            "person": {
              "name": "Jane Doe",
              "age": "13",
              "gender": "female",
              "tags": [
                {
                  "descriptor": {
                    "code": "professional-details",
                    "name": "Professional Details"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "code": "profession",
                        "name": "profession"
                      },
                      "value": "student"
                    }
                  ],
                  "display": true
                }
              ]
            },
            "contact": {
              "phone": "+91-9663088848",
              "email": "jane.doe@example.com"
            }
          },
          "stops": [
            {
              "id": "0",
              "instructions": {
                "name": "content-video-1",
                "long_desc": "Description About the Content",
                "media": [
                  {
                    "mimetype": "video/mp4",
                    "url": "https://embedded-video-player-url/play"
                  }
                ]
              }
            },
            {
              "id": "1",
              "instructions": {
                "name": "content-video-2",
                "long_desc": "Description About the Content",
                "media": [
                  {
                    "mimetype": "video/mp4",
                    "url": "https://embedded-video-player-url/play"
                  }
                ]
              }
            },
            {
              "id": "2",
              "instructions": {
                "name": "content-pdf",
                "long_desc": "Description About the Content",
                "media": [
                  {
                    "mimetype": "application/pdf",
                    "url": "https://link-to-the-document/"
                  }
                ]
              }
            }
          ],
          "tags": [
            {
              "descriptor": {
                "code": "course-completion-details",
                "name": "Content Completion Details"
              },
              "list": [
                {
                  "descriptor": {
                    "code": "course-certificate",
                    "name": "Course certificate"
                  },
                  "value": "https://link-to-certificate"
                },
                {
                  "descriptor": {
                    "code": "course-badge",
                    "name": "Course Badge"
                  },
                  "value": "https://link-to-badge"
                }
              ],
              "display": true
            }
          ]
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "150"
        }
      },
      "billing": {
        "name": "Jane Doe",
        "phone": "+91-9663088848",
        "email": "jane.doe@example.com",
        "address": "No 27, XYZ Lane, etc"
      },
      "payments": [
        {
          "params": {
            "amount": "150",
            "currency": "INR"
          },
          "type": "PRE-ORDER",
          "status": "PAID",
          "collected_by": "bpp"
        }
      ]
    }
  }
}
```

Below is an example of a `cancel` request
```
{
    "context": {
        "domain": "onest:learning-experiences",
        "version": "1.1.0",
        "action": "cancel",
        "bap_id": "onest.becknprotocol.io",
        "bap_uri": "https://onest-network.becknprotocol.io/",
        "bpp_id": "infosys.springboard.io",
        "bpp_uri": "https://infosys.springboard.io",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
        "ttl": "PT10M",
        "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "order_id" : "d4975df5",
        "cancellation_reason_id" :"1",
        "descriptor": {
            "short_desc" :"Not Satisfied"
        }
    }
}
```
Below is an example of a `on_cancel` callback
```
{
    "context": {
      "domain": "onest:learning-experiences",
      "version": "1.1.0",
      "action": "on_cancel",
      "bap_id": "onest.becknprotocol.io",
      "bap_uri": "https://onest-network.becknprotocol.io/",
      "bpp_id": "infosys.springboard.io",
      "bpp_uri": "https://infosys.springboard.io",
      "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
      "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
      "ttl": "PT10M",
      "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
      "order": {
        "id": "d4975df5",
        "provider": {
          "id": "INFOSYS",
          "descriptor": {
            "name": "Infosys Springboard",
            "short_desc": "Infosys Springboard Digital literacy program",
            "images": [
              {
                "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/app_logos/landing-new.png",
                "size_type": "sm"
              }
            ]
          }
        },
        "items": [
          {
            "id": "d4975df5-b18c-4772-80ad-368669856d52",
            "quantity": {
              "maximum": 1
            },
            "descriptor": {
              "name": "Everyday Conversational English",
              "long_desc": "Everyday Conversational English",
              "images": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english.png"
                }
              ],
              "media": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/preview/"
                }
              ]
            },
            "price": {
              "currency": "INR",
              "value": "0"
            },
            "category_ids": [
              "LANGUAGE-COURSES"
            ],
            "rating": "4.5",
            "tags": [
              {
                "descriptor": {
                  "code": "course-highlights",
                  "name": "Course Highlights"
                },
                "list": [
                  {
                    "descriptor": {
                      "code": "highlight",
                      "name": "highlight-1"
                    },
                    "value": "Focusing on clear pronunciation and reducing any strong accents that may impede communication."
                  },
                  {
                    "descriptor": {
                      "code": "highlight",
                      "name": "highlight-2"
                    },
                    "value": "Expanding everyday vocabulary to facilitate common conversations."
                  }
                ],
                "display": true
              },
              {
                "descriptor": {
                  "code": "course-prerequisites",
                  "name": "Course Prerequisites"
                },
                "list": [
                  {
                    "descriptor": {
                      "code": "prerequisite",
                      "name": "prerequisite-1"
                    },
                    "value": "Should have a basic understanding of English"
                  },
                  {
                    "descriptor": {
                      "code": "prerequisite",
                      "name": "prerequisite-2"
                    },
                    "value": "Access to a computer or internet to access the course online"
                  }
                ],
                "display": true
              }
            ],
            "rateable": true
          },
          {
            "id": "c9461edd-628d-46f3-820e-bab42b57d143",
            "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
            "descriptor": {
              "name": "Everyday Conversational English - Chapter 1",
              "long_desc": "Everyday Conversational English - Chapter 1",
              "images": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch1.png"
                }
              ],
              "media": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch1/"
                }
              ]
            }
          },
          {
            "id": "77223dc6-f6e4-48dd-bf0e-1e43841e651c",
            "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
            "descriptor": {
              "name": "Everyday Conversational English - Chapter 2",
              "long_desc": "Everyday Conversational English - Chapter 2",
              "images": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch2.png"
                }
              ],
              "media": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch2/"
                }
              ]
            }
          },
          {
            "id": "eae312ed-5a2a-4b95-bed8-407a832b11b8",
            "parent_item_id": "d4975df5-b18c-4772-80ad-368669856d52",
            "descriptor": {
              "name": "Everyday Conversational English - Chapter 3",
              "long_desc": "Everyday Conversational English - Chapter 3",
              "images": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/assets/images/infosysheadstart/everyday-conversational-english-ch3.png"
                }
              ],
              "media": [
                {
                  "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english-ch3/"
                }
              ]
            }
          }
        ],
        "billing": {
          "name": "Namma Yatri",
          "organization": {
            "address": "Girija Building, Number 817, Ganapathi Temple Rd",
            "city": "Bengaluru",
            "state": "Karnataka",
            "contact": {
              "email": "nammayatri.support@juspay.in",
              "phone": "+91 80 68501060"
            }
          }
        },
        "quote": {
          "price": {
            "currency": "INR",
            "value": "0"
          }
        },
        "fulfillments": [
          {
            "customer": {
              "person": {
                "name": "Manjunath",
                "creds": {
                  "type": "VerifiableCredential",
                  "url": "https://infyspringboard.onwingspan.com/web/courses/infosysheadstart/everyday-conversational-english/certificate"
                }
              },
              "contact": {
                "phone": "+91 9988764321",
                "email": "manjunath@gmail.com"
              }
            },
            "agent": {
              "person": {
                "id": "eng-01-prof",
                "name": "Prof. Shipra Vaidya"
              }
            },
            "state": {
              "descriptor": {
                "code": "course-cancelled",
                "name": "Course has been cancelled"
              }
            }
          }
        ],
        "payments": [
          {
            "params": {
              "amount": "0",
              "currency": "INR"
            },
            "status": "PAID"
          }
        ],
        "type": "DEFAULT"
      }
    }
  }
```

Below is an example of a `track` request
```
{
    "context": {
        "domain": "onest:courses",
        "version": "1.1.0",
        "action": "track",
        "bap_id": "onest.becknprotocol.io",
        "bap_uri": "https://onest-network.becknprotocol.io/",
        "bpp_id": "infosys.springboard.io",
        "bpp_uri": "https://infosys.springboard.io",
        "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
        "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
        "ttl": "PT10M",
        "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "order_id" : "d4975df5"
    }
}
```
Below is an example of a `on_track` callback
```
{
  "context": {
    "domain": "onest:courses",
    "version": "1.1.0",
    "action": "on_track",
    "bap_id": "onest.becknprotocol.io",
    "bap_uri": "https://onest-network.becknprotocol.io/",
    "bpp_id": "infosys.springboard.io",
    "bpp_uri": "https://infosys.springboard.io",
    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
    "ttl": "PT10M",
    "timestamp": "2023-02-20T15:21:36.925Z"
  },
  "message": {
    "tracking" : {
      "id" : "66666ahhf9u385",
      "url" : "tracking-url-for-course-progress",
      "status" : "active"
    }
  }
}
```

## 1.4 Post-fulfillment of Courses
This section contains recommendations for implementing the APIs after delivery of courses.

### 1.4.1 Recommendations for BPPs

#### 1.4.1.1 Rating and Feedback
- REQUIRED. The BPP MUST implement the `rating` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST implement the `get_rating_categories` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.4.1.2 Providing Support
- REQUIRED. The BPP MUST implement the `support` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 1.4.2 Recommendations for BAPs

#### 1.4.2.1 Rating and Feedback
- REQUIRED. The BAP MUST implement the `on_rating` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `rating`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BAP MUST implement the `rating_categories` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `get_rating_categories`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 1.4.2.2 Providing Support
- REQUIRED. The BAP MUST implement the `on_support` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `support`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 1.4.3 Example Workflow
#### 1.4.3.1 Rating & Feedback Workflow
1. Learner wants to give rating to the Course/Instructor.
2. BAP calls /get_rating_categories API.
3. BPP provides the available rating categories using /rating_categories API to the BAP.
4. BAP render the rating category.
5. Learner provide the rating on a particular category (i.e, Order, Item or Provider).
6. BAP send the rating to the BPP using /rating API.
7. BPP acknowledge the rating and provide feedback link to the BAP using /on_rating API.
8. BAP might choose to fill the feedback form.

### 1.4.3.2 Support Workflow
1. Learner faces any issue w.r.t to the order or delivery.
2. Learner creates a support request.
3. BAP send /support call to the BPP.
4. BPP acknowledge the support request and send support contact details using /on_support API call.

### 1.4.4 Example Requests

Below is an example of a `get_rating_categories` request
```
{
    "context": {
      "domain": "onest:courses",
	    "version": "1.1.0",
	    "action": "get_rating_categories",
	    "bap_id": "onest.becknprotocol.io",
	    "bap_uri": "https://onest-network.becknprotocol.io/",
	    "bpp_id": "infosys.springboard.io",
	    "bpp_uri": "https://infosys.springboard.io",
	    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
	    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
	    "ttl": "PT10M",
	    "timestamp": "2023-02-20T15:21:36.925Z"
    }
}
```

Below is an example of an `rating_categories` callback
```
{
    "context": {
        "domain": "onest:courses",
	    "version": "1.1.0",
	    "action": "rating_categories",
	    "bap_id": "onest.becknprotocol.io",
	    "bap_uri": "https://onest-network.becknprotocol.io/",
	    "bpp_id": "infosys.springboard.io",
	    "bpp_uri": "https://infosys.springboard.io",
	    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
	    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
	    "ttl": "PT10M",
	    "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "rating_categories" : [
            "Item",
            "provider",
            "Order"
        ]
    }
}
```

Below is an example of a `rating` request

```
{
    "context": {
        "domain": "onest:courses",
	    "version": "1.1.0",
	    "action": "rating",
	    "bap_id": "onest.becknprotocol.io",
	    "bap_uri": "https://onest-network.becknprotocol.io/",
	    "bpp_id": "infosys.springboard.io",
	    "bpp_uri": "https://infosys.springboard.io",
	    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
	    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
	    "ttl": "PT10M",
	    "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "rating" : {
            "rating_category": "Provider",
            "id" : "471",
            "value" :"3"
        }  
    }
}
```
Below is an example of an `on_rating` callback
```
{
    "context": {
        "domain": "onest:courses",
	    "version": "1.1.0",
	    "action": "on_rating",
	    "bap_id": "onest.becknprotocol.io",
	    "bap_uri": "https://onest-network.becknprotocol.io/",
	    "bpp_id": "infosys.springboard.io",
	    "bpp_uri": "https://infosys.springboard.io",
	    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
	    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
	    "ttl": "PT10M",
	    "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "feedback_form" : {
            "form" : {
                "url" : "url of the feedback form",
                "mime_type" :"text/html"
            },
            "required" : true
        } 
    }
}
```

Below is an example of a `support` request
```
{
    "context": {
        "domain": "onest:courses",
	    "version": "1.1.0",
	    "action": "support",
	    "bap_id": "onest.becknprotocol.io",
	    "bap_uri": "https://onest-network.becknprotocol.io/",
	    "bpp_id": "infosys.springboard.io",
	    "bpp_uri": "https://infosys.springboard.io",
	    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
	    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
	    "ttl": "PT10M",
	    "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "support" : {
            "order_id" : "12424kh",
            "callback_phone" : "+91-8858150053"
        }  
    }
}
```

Below is an example of an `on_support` callback
```
{
    "context": {
        "domain": "onest:courses",
	    "version": "1.1.0",
	    "action": "on_support",
	    "bap_id": "onest.becknprotocol.io",
	    "bap_uri": "https://onest-network.becknprotocol.io/",
	    "bpp_id": "infosys.springboard.io",
	    "bpp_uri": "https://infosys.springboard.io",
	    "transaction_id": "a9aaecca-10b7-4d19-b640-b047a7c62196",
	    "message_id": "d514a38f-e112-4bb8-a3d8-b8e5d8dea82d",
	    "ttl": "PT10M",
	    "timestamp": "2023-02-20T15:21:36.925Z"
    },
    "message": {
        "support" : {
            "ref_id" : "Abjhjh13773",
            "order_id" : "12424kh",
            "callback_phone" : "+91-8858150053",
            "email" : "support@ekstep.com",
            "phone" : "+91-965676879",
            "url" : "chat-url-for-support",
            "docs": [
                {
                    "descriptor":{
                        "name" :"FAQs",
                        "short_desc" : "Frequently asked questions and common issues"
                    },
                    "url" : "https://link-to-the-document.com",
                    "mime_type" : "application/pdf"
                }
            ]

        }  
    }
}
```