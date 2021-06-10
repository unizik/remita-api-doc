# Remita Payments API Documentation
Remita Payment API Documentation for Contractors and Internal Developers

This document explains how you can interact with Nnamdi Azikiwe Remita Gateway to generate and verify RRRs for your independent or individual applications.

## Getting Started
You will need to obtain the following from the MICTU:
- ContractorID
- APIKey
- APISecret

These details are unique and are used to identify each call to all our API endpoints including the Remita Payments API. Please register as a contractor to get the Contractor ID and apply for the API Key and API Secret. Once approved, we will send you instructions on how to access the details.

If you encounter any issues, please contact the MICTU Team for support.

### API Headers
This is an important part in any call to any of our endpoints. The headers must include the following custom headers:
```
  Nau-Contractor-ID:
  Nau-Api-Key:
  Nau-Access-Token:
```
### Base URL and Headers
```
BASE-URL: https://api.unizik.edu.ng/remita/v1
```
Header Requirements:
  - `Nau-Contractor-Id`
  - `Nau-Api-Key`
  - `Nau-Access-Token`

### Generate RRR
Generate RRR for any payment using our `{{base-url}}/rrr/generate`
###### Method
``POST``
###### Headers
**Content-Type:** `application/json`
**Nau-Contractor-ID:** `{{contractorID}}`
**Nau-Api-Key:** `{{apiKey}}`
**Nau-Access-Token:** `{{apiToken}}`

###### Body (raw)
```json
{
    "serviceTypeId": "{{serviceTypeId}}",
    "amount": "{{totalAmount}}",
    "orderId": "{{orderId}}",
    "userType": "{{userType}}",
    "paySession": "{{paySession}}",
    "payerID": "{{payerID}}",
    "payerName": "Ekene Ezeasor",
    "payerEmail": "ezeasorekene@unizik.edu.ng",
    "payerPhone": "08063961963",
    "description": "Hostel accommodation fee for Floor B Room 2"
}
```

###### Response (json)
```
{
  code: 200,
  message: "RRR generated successfully",
  status: "success",
  data: {
    rrr: "{{generated_rrr}}",
    payerID: "{{payerID}}",
    orderId: "{{orderId}}",
    serviceTypeId: "{{serviceTypeId}}",
    date_generated: "{{date_generated}}"
  }
}
```

###### Example Request
```
curl --location -g --request POST '{{base-url}}/rrr/generate' \
--header 'Content-Type: application/json' \
--header 'Nau-Contractor-ID: {{contractorID}}' \
--header 'Nau-Api-Key: {{apiKey}}' \
--header 'Nau-Access-Token: {{apiToken}}' \
--data-raw '{
    "serviceTypeId": "{{serviceTypeId}}",
    "amount": "{{totalAmount}}",
    "orderId": "{{orderId}}",
    "userType": "{{userType}}",
    "paySession": "{{paySession}}",
    "payerID": "{{payerID}}",
    "payerName": "Ekene Ezeasor",
    "payerEmail": "ezeasorekene@unizik.edu.ng",
    "payerPhone": "08063961963",
    "description": "Hostel accommodation fee for Floor B Room 2"
}'
```

### Generate RRR with Split Payment
Generate RRR for any payment using our `{{base-url}}/rrr/generate`. The payment will be split to different accounts specified
###### Method
``POST``
###### Headers
**Content-Type:** `application/json`
**Nau-Contractor-ID:** `{{contractorID}}`
**Nau-Api-Key:** `{{apiKey}}`
**Nau-Access-Token:** `{{apiToken}}`

###### Body (raw)
```json
{
    "serviceTypeId": "{{serviceTypeId}}",
    "amount": "{{totalAmount}}",
    "orderId": "{{orderId}}",
    "userType": "{{userType}}",
    "paySession": "{{paySession}}",
    "payerID": "{{payerID}}",
    "payerName": "Ekene Ezeasor",
    "payerEmail": "ezeasorekene@unizik.edu.ng",
    "payerPhone": "08063961963",
    "description": "Hostel accommodation fee for Floor B Room 2",
    "split_payment": [
      {
        "splitpaymentId":"beneficiary1",
        "beneficiaryName":"Wissenschaft",
        "beneficiaryAccount":"5088270298",
        "bankCode":"058",
        "beneficiaryAmount":"10000",
        "deductFeeFrom":"1"
      },
      {
        "splitpaymentId":"beneficiary2",
        "beneficiaryName":"CIRMS",
        "beneficiaryAccount":"5087029890",
        "bankCode":"057",
        "beneficiaryAmount":"50000",
        "deductFeeFrom":"1"
      }

    ]
}
```

###### Response (json)
```
{
  code: 200,
  message: "RRR generated successfully",
  status: "success",
  data: {
    rrr: "{{generated_rrr}}",
    payerID: "{{payerID}}",
    orderId: "{{orderId}}",
    serviceTypeId: "{{serviceTypeId}}",
    date_generated: "{{date_generated}}"
  }
}
```

###### Example Request
```
curl --location -g --request POST '{{base-url}}/rrr/generate' \
--header 'Content-Type: application/json' \
--header 'Authorization: Nau-Contractor-ID={{contractorID}},Nau-Api-Key={{apiKey}},Nau-Access-Token={{apiToken}}' \
--data-raw '{
    "serviceTypeId": "{{serviceTypeId}}",
    "amount": "{{totalAmount}}",
    "orderId": "{{orderId}}",
    "userType": "{{userType}}",
    "paySession": "{{paySession}}",
    "payerID": "{{payerID}}",
    "payerName": "Ekene Ezeasor",
    "payerEmail": "ezeasorekene@unizik.edu.ng",
    "payerPhone": "08063961963",
    "description": "Hostel accommodation fee for Floor B Room 2"
}'
```

### Verify RRR
Verify RRR for any payment using our `{{base-url}}/rrr/verify/{{rrr}}/{{payerID}}/{{orderID}}/{{serviceTypeId}}`
###### Method
``GET``
###### Headers
**Nau-Contractor-ID:** `{{contractorID}}`
**Nau-Api-Key:** `{{apiKey}}`
**Nau-Access-Token:** `{{apiToken}}`

###### GET Parameters
```
GET {{base-url}}/rrr/verify/{{rrr}}/{{payerID}}/{{orderID}}/{{serviceTypeId}}
```

###### Response (json)
```
{
  code: 200,
  message: "RRR Paid",
  status: "success",
  data: {
    rrr: "{{generated_rrr}}",
    payerID: "{{payerID}}",
    orderId: "{{orderId}}",
    serviceTypeId: "{{serviceTypeId}}",
    debit_date: "{{debit_date}}",
    transaction_date: "{{transaction_date}}"
  }
}
```

###### Example Request
```
curl --location -g --request GET '{{base-url}}/rrr/verify/{{rrr}}/{{payerID}}/{{orderID}}/{{serviceTypeId}}' \
--header 'Nau-Contractor-Id: {{contractorID}}' \
--header 'Nau-Api-Key: {{apiKey}}' \
--header 'Nau-Access-Token: {{apiToken}}'
```

### Errors
We use standard http error codes sent as a header. However, each error code also has an extended message to explain the error returned as a json object.

##### Example Error Codes
###### 503 Service Unavailable
```
{
  code: 503,
  message: "RRR was generated but could not be saved. Please try again or contact support",
  status: "failed"
}
```
###### 424 Failed Dependency
```
{
  code: 424,
  message: "RRR could not be generated",
  status: "failed"
}
```


### Fields and Definitions
**Parameter**|**Type**|**Description**
:-----:|:-----:|:-----:
contractorID|String|`Required` Your UNIZIK Contractor ID. Visit [contractors.unizik.edu.ng](contractors.unizik.edu.ng) to get started
serviceTypeId|String|`Required` ID for your good/service on Remita. Contact MICTU to obtain this ID.
amount|String|`Required` Amount for Invoice
orderId|String|`Required` Your Unique reference for that Invoice
payerName|String|`Required` Payer Name
payerEmail|String|`Required` Payer Email
payerPhone|String|`Required` Payer Phone Number
description|String|`Required` Description
apiToken|String|`Required` SHA-512 hash of ContractorID + APIKey + APISecret
name|string|`Required` Name of Custom Field as you created it on Remita
value| |`Required` The value of that custom field that you have applied to said invoice. Data type determined by 'type' field below
type|String|`Required` See a list of data types below. You would have selected a data type when creating your custom fields on Remita
splitpaymentId|String|`Required` Your unique identifier for each split payment detail
beneficiaryName|String|`Required` Name of Beneficiary in the split payment
beneficiaryAccount|String|`Required` Account Number to which Beneficiary will be receiving their share of the payment
bankCode|String|`Required` Bank code for Account Number of Beneficiary in the split payment. Click Here for Bank Codes
beneficiaryAmount|String|`Required` Amount from the Total Invoiced amount to be paid to the beneficiary
deductFeeFrom|String|`Required` Value should be '1' or '0'. '1' if the beneficiary in question will be bearing the transaction fees. Transaction fees can only be borne by one beneficiary.
apiKey|String|`Required` Your Unique API Key from UNIZIK.
amount|String|`Required` Amount for Invoice
