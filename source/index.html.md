---
title: Get Covered Policy API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the **Get Covered** API Documentation page.  The following sections will detail the process and options required to bind a policy for users on an external service.  In order to proceed you will need an **Access Token** provided by **Get Covered**.

All requests accept JSON and respond in kind.  Most endpoints are RESTful, although a few outliers do exist and are detailed in the sections below.

# Authentication

**Coming Soon**

# Insurables
## Introduction 

The *Insurable* model represents anything for which a policy can be issued to cover.  It may be a residential unit, house, car, business or other type of asset.  The documentation at this time will focus on properties that have a physical address.  They will be expanded as our API opens up to include more functionality.

## Get or Create 

# Policy Types
## Introduction 

Get Covered supports multiple products through a single API.  To differentiate between offerings, the *Policy Type* model helps our system designate which rules and integrations need to be called to bind the correct product.

## Usage 

A *Policy Type* is specified on a *Policy Application*, *Policy Quote* and a *Policy* but its **ID**.  Use the following reference to select which product the request is relevant to.

ID | Title | Description
--------- | ------- | ----------- 
1 | Renters | An HO4 product covering renters at a residential address
2 | Master Policy | TLL product
3 | Master Policy Coverage | Coverage issued on behalf of a Master Policy
4 | Commercial | Business Owner Policy
4 | Rent Guarantee | A stupid product
5 | Renters Deposit Bond | A product I don't know how to describe

# Policy Applications

## Introduction 

A *Policy Application* is the data structure used to store information required to generate a [*Policy Quote*](#policy-quote).  All *Policy Applications* will need information regarding the effective / expiration date, some information about the users who will be covered and data on the *Insurable* that will be covered.

## Get New Policy Application

```shell
curl -X POST \ 
     -H "Content-Type: application/json" \
     -d '{ "policy_application" : { "agency_id" : 1, "account_id" : 1, "policy_type_id" : 1, "policy_insurables_attributes" : [{ "id" : 1 }] }}' \
     "http://api.getcoveredinsurance.com/v2/policy-applications/new"
```

```ruby
include HTTParty

new_policy_application_params = {
  "policy_application": {
    "agency_id": 1,
    "account_id": 1, 
    "policy_type_id": 1, 
    "policy_insurables_attributes": [
      { "id": 1 } 
    ]
  }
}

new_policy_application_request = HTTParty.post("http://api.getcoveredinsurance.com/v2/policy-applications/new",
                                               body: new_policy_application_params.to_json,
                                               headers: {})
```

> The above command returns JSON structured like this:

```json
{
    "reference":null,
    "external_reference":null,
    "effective_date":null,
    "expiration_date":null,
    "status":"started",
    "status_updated_on":null,
    "fields":[
        {
            "title":"Number of Insured",
            "value":1,
            "options":[
                1,
                2,
                3,
                4,
                5,
                6,
                7,
                8
            ],
            "answer_type":"INTEGER",
            "default_answer":1
        }
    ],
    "questions":[

    ],
    "coverage_selections": [
    
    ],
    "carrier_id":5,
    "policy_type_id":1,
    "agency_id":1,
    "account_id":1,
    "billing_strategy_id": null,
    "policy_insurables_attributes": [
        { "id": 1 }
    ],
    "users_attributes":{
        "primary":true,
        "spouse":false,
        "user_attributes":{
            "email":null,
            "profile_attributes":{
                "first_name":null,
                "last_name":null,
                "contract_phone":null,
                "birth_date":null,
                "job_title":null
            }
        }
    }
}
```

This endpoint retrieves a new Policy Application.

### HTTP Request

`POST http://api.getcoveredinsurance.com/v2/policy-applications/new`

### Policy Request Application Parameters

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
agency_id | true | ID provided for each **Agency** in Get Covered's system | 
account_id | false | ID for property management organization which has ownership of the insurable to be covered |
policy_type_id | true | ID of the **Policy Type** from the [Policy Types](#policy-types) section | 
policy_insurables_attributes | true | array of IDs of **Insurables** to be covered by policy.  Provided in the [Get or Create](#get-or-create) request |

### Policy Response Application Parameters

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
reference | false | System set field.  Provides a unique string to use in identifying a *Policy Application* within Get Covered's system. | 
external_reference | false | System set field.  Provides a unique string used by carrier to identify a *Policy Application* in their external system. | 
effective_date | true | Date coverage will start, must be a date in the future. | 
expiration_date | true | Date coverage will end, for residential policies this will be 1 year from the effective_date.
status | true | Current state of the *Policy Application*. | started, in_progress, complete, abandoned, quote_in_progress, quote_failed, quoted, more_required, accepted, rejected 
status_updated_on | false | Blank | 
fields | true | An array of hashes that represent questions required to quote a *Policy Application*.  The __value__ field of each hash will accept any of the options defined in the __options__ array | 

# Policy Quote

# Policy
