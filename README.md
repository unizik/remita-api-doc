# Remita Payments API Documentation
Remita Payment API Documentation for Contractors and Internal Developers

This documentation explains how you can interact with Nnamdi Azikiwe Remita Gateway to generate and verify RRRs for your independent or individual applications.

## Getting Started
You will need to obtain the following from the MICTU:
- Contractor ID
- API Key
- API Secret

These details are unique and are used to identify each call to all our API endpoints including the Remita Payments API. Please register as a contractor to get the Contractor ID and apply for the API Key and API Secret. Once approved, we will send you instructions on how to access the details.

If you encounter any issues, please contact the MICTU Team for support.

### API Headers
This is an important part in any call to any of our endpoints. The headers must include the following custom headers:
```
  Nau-Contractor-ID:
  Nau-API-Key:
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
  - `Nau-Api-Secret` (*Used to hash the token only. Not sent as a header*)

### Generate RRR
Generate RRR for any payment using our `{{base-url}}/rrr/generate`
###### Method
``POST``
###### Headers
**Content-Type:** `application/json`
**Content-Type:** `application/json`
**Nau-Contractor-ID:** `{{contractorID}}`
**Nau-API-Key:** `{{apiKey}}`
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
--header 'Nau-API-Key: {{apiKey}}' \
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
**Content-Type:** `application/json`
**Nau-Contractor-ID:** `{{contractorID}}`
**Nau-API-Key:** `{{apiKey}}`
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
        "lineItemsId":"itemId1",
        "beneficiaryName":"Wissenschaft",
        "beneficiaryAccount":"5088270298",
        "bankCode":"058",
        "beneficiaryAmount":"10000",
        "deductFeeFrom":"1"
      },
      {
        "lineItemsId":"itemId2",
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
  code: 211,
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
--header 'Authorization: Nau-Contractor-ID={{contractorID}},Nau-API-Key={{apiKey}},Nau-Access-Token={{apiToken}}' \
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
Verify RRR for any payment using our `{{base-url}}/rrr/verify`
###### Method
``GET``
###### Headers
**Content-Type:** `application/json`
**Nau-Contractor-ID:** `{{contractorID}}`
**Nau-API-Key:** `{{apiKey}}`
**Nau-Access-Token:** `{{apiToken}}`

###### Body (raw)
```json
{
    "orderId": "{{orderId}}",
    "payerID": "{{payerID}}",
    "rrr": "{{rrr}}"
}
```

###### Response (json)
```
{
  code: 212,
  message: "RRR Paid",
  status: "success",
  data: {
    rrr: "{{generated_rrr}}",
    payerID: "{{payerID}}",
    orderId: "{{orderId}}",
    serviceTypeId: "{{serviceTypeId}}",
    amount: "totalAmount",
    date_generated: "{{date_generated}}"
  }
}
```

###### Example Request
```
curl --location -g --request GET '{{base-url}}/rrr/verify/{{rrr}}/{{payerID}}/{{orderID}}' \
--header 'Content-Type: application/json' \
--header 'Nau-Contractor-ID: {{contractorID}}' \
--header 'Nau-API-Key: {{apiKey}}' \
--header 'Nau-Access-Token: {{apiToken}}'
```

### Fields and Definitions
**Parameter**|**Type**|**Description**
:-----:|:-----:|:-----:
contractorID|String|`Required` Your UNIZIK Contractor ID. Visit `contractors.unizik.edu.ng` to get started
serviceTypeId|String|`Required` ID for your good/service on Remita. Contact MICTU to obtain this ID.
amount|String|`Required` Amount for Invoice
orderId|String|`Required` Your Unique reference for that Invoice
payerName|String|`Required` Payer Name
payerEmail|String|`Required` Payer Email
payerPhone|String|`Required` Payer Phone Number
description|String|`Required` Description
apiToken|String|`Required` SHA-512 hash of contractorID + apiKey + apiSecret
name|string|`Required` Name of Custom Field as you created it on Remita
value| |`Required` The value of that custom field that you have applied to said invoice. Data type determined by 'type' field below
type|String|`Required` See list of data types below. You would have selected a data type when creating your custom fields on Remita
lineItemsId|String|`Required` Your unique identifier for each line item. A line item being the relevant details of a beneficiary in the split payment
beneficiaryName|String|`Required` Name of Beneficiary in the split payment
beneficiaryAccount|String|`Required` Account Number to which Beneficiary will be receiving their share of the payment
bankCode|String|`Required` Bank code for Account Number of Beneficiary in the split payment. Click Here of Bank Codes
beneficiaryAmount|String|`Required` Amount from the Total Invoiced amount to be paid to beneficiary
deductFeeFrom|String|`Required` Value should be '1' or '0'. '1' if the benifiary in question will be bearing the transaction fees. Transaction fees can only be borne by one beneficiary.
apiKey|String|`Required` Your Unique API Key from UNIZIK.
amount|String|`Required` Amount for Invoice
