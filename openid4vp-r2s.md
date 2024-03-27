# Request to sign based on OpenID4VP

# Introduction

This specification utilize the mechanisms of OpenID for Verfifiable Presentation [@!OPENID4VP] by an option to provide input data to a holder (wallet), which it must digitally sign and present it to back to the verifier. The motivation for this extension is to include contextual data provided by the verifier that must be linked dynamically to a credential and its cryptographic proof as it is required by strong customer authentication (SCA) for example.

# Requirements Notation and Conventions

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL NOT**",
"**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**NOT RECOMMENDED**", "**MAY**", and
"**OPTIONAL**" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@!RFC8174]
when, and only when, they appear in all capitals, as shown here.

# Flow



```mermaid

title R2S

participant user as u
participant wallet as w
participant verifier as v

v->w: auth request inc.\n request2sign
w->u: present request2sign
u->w: consent
w->w: sign
w->v: auth resp

```

# Authorization request

The authorization request as defined in [@!OPENID4VP, section 5] MUST be transported as OAuth 2.0 rich authorization request to include a container holding the data for the signing request in the request parameter `authorization_details` as defined in [@!RFC9396, section 2]. The `authorization_details` array hereby MUST include a JSON object with the `type` field set to the value of `request2sign`. In addition, this specification defines the following fields for the `request2sign` object:

`payload`: REQUIRED. JSON object holding the actual data that must be signed by the wallet.

Example of the `authorization_details`:

```json
[{
  "type": "request2sign",
  "payload": "...",
}]

```

## Presentation definition

The `presentation_definition` used in the authorization request MUST

- request a self-attested credential using the `subject_is_issuer` property of the relational constraint feature described [@!DIF.PresentationExchange, section 7.3]. 
- process the `request2sign` payload from the authorization request as user-entered data for a self-attested submission for the `input_descriptor` `field` with the id `request2sign_payload`. The JSON Schema `filter` object MAY be used by the wallet to generate a dynamic form to display the content of the payload to the user.    

Example of a presentation definition requesting a self-attested credential issued by `did:example:sd5sde`:

```json
{
  "presentation_definition": {
    "id": "32f54163-7166-48f1-93d8-ff217bdb0653",
    "input_descriptors": [
      {
        "id": "request2sign_input",
        "constraints": {
          "subject_is_issuer": "required",
          "fields": [
            {
              "id": "request2sign_payload",
              "path": [
                "$.credentialSubject.request2sign_payload"
              ],
              "filter": {
                "$schema": "http://json-schema.org/draft-04/schema#",
                "type": "object",
                "properties": {
                  "transaction": {
                    "type": "string"
                  },
                  "name": {
                    "type": "string"
                  },
                  "employeeID": {
                    "type": "string"
                  }
                },
                "required": [
                  "transaction",
                  "name",
                  "employeeID"
                ]
              }
            },
            {
              "id": "issuer",
              "path": [
                "$.iss"
              ],
              "filter": {
                "type": "string",
                "const": "did:example:sd5sde"
              }
            },
            {
              "id": "subject",
              "path": [
                "$.sub"
              ],
              "filter": {
                "type": "string",
                "const": "did:example:sd5sde"
              }
            }
          ]
        }
      }
    ]
  }
}
```


