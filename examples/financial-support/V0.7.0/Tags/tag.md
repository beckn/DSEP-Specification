# Tag Groups

Tag groups are labels under which various tags can be grouped. In the Financial support sector, Tag Groups can contains various eligibility-specific information. Tag Groups can be standardized at a domain-level or a network-level depending upon implementer demand for standardizations. Some examples of Tag Groups are shown below. 

Note: The below list is NOT a recommended or required standard. This list will be standardized on the basis of adoption by implementers. 

## Examples:
| Example Name `(groups/[index]/descriptor.name)` | Example Code `groups/[index]/descriptor.code` | Description                                                                                            |
|-----------------------------------------|---------------------------------------|--------------------------------------------------------------------------------------------------------|
| Social Eligibility                            | soc-elg                                | The Eligible social class for the scholarship. |
| Gender Eligibility                             | gen-elg                           | The Eligible genders for the scholarship.|
| Financial Eligibility                     | fin-elg                          | The maximum family income needed, for the student to apply for a scholarship |
| Academic Eligibility                       | acad-elg                            | The Academic eligibility criteria for the student to apply for a scholarship|
| Documents Required                       | docs-reqd                            | Documents required to apply for the Scholarship.|
| Additional Information                       | add-info                            | Additional information related to the scholarship.|

# Tag

The Tag schema contains a key-value pair containing the actual extended metadata to be communicaated.


## Examples of Additional Information Tag Group:
| Example Name `(groups/[index]/list[index].descriptor.name)` | Example Code `groups/[index]/list[index].descriptor.code` | Description                                                                                            |
|-----------------------------------------|---------------------------------------|--------------------------------------------------------------------------------------------------------|
| Frequently Asked Questions's URL | faq-url                                | Contains the URL of the Frequently asked questions in beckn standarized format which is given [here](./faq.json) |
| Terms and Conditions's URL | tnc-url                                | Contains the URL of the Terms and conditions. |
