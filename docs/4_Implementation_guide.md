# 4. Implementation Guide

This document contains the REQUIRED and RECOMMENDED standard functionality that must be implemented by any DSEP Service Provider Platform a.k.a BPPs and DSEP Consumer Platform a.k.a BAPs.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 [RFC2119].

## 4.1 Discovery of Education & Skilling Services

### 4.1.1 Recommendations for BPPs
The following recommendations need to be considered when implementing discovery functionality for a DSEP BPP

- REQUIRED. The BPP MUST implement the `search` endpoint to receive an `Intent` object sent by BAPs
- REQUIRED. The BPP MUST return a catalog of DSEP products on the `on_search` callback endpoint specified in the `context.bap_uri` field of the `search` request body.
- REQUIRED. The BPP MUST map its education & skilling service products to the `Item` schema.
- REQUIRED. Any DSEP service provider-related information like name, logo, short description must be mapped to the `Provider.descriptor` schema
- REQUIRED. Any form that must be filled before receiving a quotation must be mapped to the `XInput` schema
- REQUIRED. If the platform wants to group its products under a specific category, it must map each category to the `Category` schema
- REQUIRED. Any service fulfillment-related information MUST be mapped to the `Fulfillment` schema.
- REQUIRED. If the BPP does not want to respond to a search request, it MUST return a `ack.status` value equal to `NACK`
- RECOMMENDED. Upon receiving a `search` request, the BPP SHOULD return a catalog that best matches the intent. This can be done by indexing the catalog against the various probable paths in the `Intent` schema relevant to typical DSEP use cases

### 4.1.2 Recommendations for BAPs
- REQUIRED. The BAP MUST call the `search` endpoint of the BG to discover multiple BPPs on a network
- REQUIRED. The BAP MUST implement the `on_search` endpoint to consume the `Catalog` objects containing DSEP Products sent by BPPs.
- REQUIRED. The BAP MUST expect multiple catalogs sent by the respective DSEP Providers on the network
- REQUIRED. The DSEP products can be found in the `Catalog.providers[].items[]` array in the `on_search` request
- REQUIRED. If the `catalog.providers[].items[].xinput` object is present, then the BAP MUST redirect the user to, or natively render the form present on the link specified on the `items[].xinput.form.url` field.
- REQUIRED. If the `catalog.providers[].items[].xinput.required` field is set to `"true"` , then the BAP MUST NOT fire a `select`, `init` or `confirm` call until the form is submitted and a successful response is received
- RECOMMENDED. If the `catalog.providers[].items[].xinput.required` field is set to `"false"` , then the BAP SHOULD allow the user to skip filling the form


### Example
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

## 4.2 Application for DSEP 
This section provides recommendations for implementing the APIs related to applying for a course.

### 4.2.1 Recommendations for BPPs

#### 4.2.1.1 Selecting a DSEP service from the catalog
- REQUIRED. The BPP MUST implement the `select` endpoint on the url specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST check for a form submission at the URL specified on the `xinput.form.url` before acknowledging a `select` request.
- REQUIRED. If the DSEP provider has successfully received the information submitted by the DSEP consumer, the BPP must return an acknowledgement with `ack.status` set to `ACK` in response to the `select` request
- REQUIRED. If the DSEP provider has returned a successful acknowledgement to a `select` request, it MUST send the offer encapsulated in an `Order` object

#### 4.2.1.2 Initializing an enrolment for a course
- REQUIRED. The BPP MUST implement the `init` endpoint on the url specified in  the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.2.1.3 Confirming the enrolment for a course
- REQUIRED. The BPP MUST implement the `confirm` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 4.2.2 Recommendations for BAPs

#### 4.2.1.1 Selecting a DSEP service from the catalog
- REQUIRED. The BAP user MUST submit the form on the url received from  `on_search`  under `xinput.form.url`.
- REQUIRED. The BAP MUST implement the `on_select` endpoint on the url specified in the `context.bap_uri` field sent during `select`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.2.2.2  Initializing an enrolment for a course
- REQUIRED. The BAP MUST hit the `init` endpoint on the url specified in  the `context.bpp_uri` field sent during `on_search`. 
- REQUIRED. The BAP MUST implement the `on_init` endpoint on the url specified in  the `context.bap_uri` field sent during `init`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 5.2.2.3 Confirming the enrolment for a course
- REQUIRED. The BAP MUST hit the `confirm` endpoint on the url specified in  the `context.bpp_uri` field sent during `on_search`. 
- REQUIRED. The BAP MUST implement the `on_confirm` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `confirm`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 5.2.3 Example Workflow

### 5.2.3 Example Requests

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

## 4.3 Fulfillment of DSEP Services
This section contains recommendations for implementing the APIs related to fulfilling a DSEP service

### 4.3.1 Recommendations for BPPs

#### 4.3.1.1 Sending status updates
- REQUIRED. The BPP MUST implement the `status` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.3.1.2 Updating an enrolment for Courses
- REQUIRED. The BPP MUST implement the `update` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.3.1.3 Cancelling an enrolment for Course
- REQUIRED. The BPP MUST implement the `cancel` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST implement the `get_cancellation_reasons` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.3.1.4 Real-time tracking
- REQUIRED. The BPP MUST implement the `track` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 4.3.2 Recommendations for BAPs

#### 4.3.2.1 Receiving status updates
- REQUIRED. The BAP MUST implement the `on_status` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `status`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.3.2.2 Updating an enrolment for Courses
- REQUIRED. The BAP MUST implement the `on_update` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `update`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.3.2.3 Cancelling an enrolment for Courses
- REQUIRED. The BAP MUST implement the `on_cancel` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `cancel`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BAP MUST implement the `cancellation_reasons` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `get_cancellation_reasons`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.3.2.4 Real-time tracking
- REQUIRED. The BAP MUST implement the `on_track` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `track`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 5.3.3 Example Workflow

### 5.3.4 Example Requests

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

## 4.4 Post-fulfillment of DSEP Services
This section contains recommendations for implementing the APIs after fulfilling a DSEP service

### 4.4.1 Recommendations for BPPs

#### 4.4.1.1 Rating and Feedback
- REQUIRED. The BPP MUST implement the `rating` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BPP MUST implement the `get_rating_categories` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.4.1.2 Providing Support
- REQUIRED. The BPP MUST implement the `support` endpoint on the url specified in URL specified in the `context.bpp_uri` field sent during `on_search`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 4.4.2 Recommendations for BAPs

#### 4.4.2.1 Rating and Feedback
- REQUIRED. The BAP MUST implement the `on_rating` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `rating`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry
- REQUIRED. The BAP MUST implement the `rating_categories` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `get_rating_categories`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

#### 4.4.2.2 Providing Support
- REQUIRED. The BAP MUST implement the `on_support` endpoint on the url specified in URL specified in the `context.bap_uri` field sent during `support`. In case of permissioned networks, this URL MUST match the `Subscriber.url` present on the respective entry in the Network Registry

### 4.4.3 Example Workflow

### 4.4.4 Example Requests

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