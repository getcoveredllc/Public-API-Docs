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

# Billing Strategies
## Introduction

A **Billing Strategy** stores data on how the **Premium** for a **Policy** is billed over the term of its coverage.  **Billing Stratgies** are specific to each **Agency** / **Carrier** / **Policy Type** combination.  The **ID** from these **Billing Strategies** is used in many requests to find Insurable Rates or tell a **Policy Application** how to divide the payments for it's premium.

## Get all Billing Strategies

```shell
curl -X GET \
     -H "Content-Type: application/json" \
     https://api.getcoveredinsurance.com/v2/billing-strategies?agency_id=1&policy_type=residential
```

```ruby
include HTTParty

billing_strategies_request = HTTParty.get("https://api.getcoveredinsurance.com/v2/billing-strategies?agency_id=1&policy_type=residential",
                                          headers: {})
```

> The above command returns JSON structured like this:

```json
[
    {
        "id": 9,
        "title": "Annually"
    },
    {
        "id": 10,
        "title": "Bi-Annually"
    },
    {
        "id": 11,
        "title": "Quarterly"
    },
    {
        "id": 12,
        "title": "Monthly"
    }
]
```

Returns all Billing Strategies for an **Agency**, **Carrier**, **Policy Type** combination.

# Insurables
## Introduction 

The *Insurable* model represents anything for which a policy can be issued to cover.  It may be a residential unit, house, car, business or other type of asset.  The documentation at this time will focus on properties that have a physical address.  They will be expanded as our API opens up to include more functionality.

## Get or Create 

```shell
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{ "address" : "270 Lafayette St, New York, NY 10012", "unit": "1207", "insurable_id": null, "create_if_ambiguous": true, "allow_creation" : true, "communities_only" : false, "titleless" : true }'
     https://api.getcoveredinsurance.com/v2/insurables/get-or-create
```

```ruby
include HTTParty

get_or_create_params = {
    :address => "270 Lafayette St, New York, NY 10012", 
    :unit => "1207", 
    :insurable_id => nil, 
    :create_if_ambiguous => true, 
    :allow_creation => true, 
    :communities_only => false, 
    :titleless => true
}

get_or_create_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/insurables/get-or-create",
                                      body: get_or_create_params.to_json,
                                      headers: {})
```

> The above command returns JSON structured like this:

```json
{
    "results_type": "confirmed_match",
    "results": [
        {
            "id": 7221,
            "title": "1207",
            "account_id": null,
            "agency_id": null,
            "insurable_type_id": 4,
            "enabled": true,
            "preferred_ho4": false,
            "category": "property",
            "primary_address": {
                "full": "270 Lafayette St, New York, NY, 10012",
                "street_number": "270",
                "street_name": "Lafayette St",
                "street_two": null,
                "city": "New York",
                "state": "NY",
                "zip_code": "10012",
                "county": null,
                "country": null
            },
            "community": {
                "id": 7220,
                "title": "270 Lafayette St",
                "enabled": true,
                "preferred_ho4": false,
                "insurable_type_id": 1
            }
        }
    ]
}
```

The **Insurable** Get or Create request finds an existing **Insurable** by its address or creates a new one in the event that it does not exist.

### Get or Create Request Parameters
Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
address | true | The full address string for the mailing location of the insurable without the unit number | 
unit | true | | string or null
allow_creation | true | Allow Get Covered to create an insurable if one does not exist | true, false
insurable_id | false | The known **ID** of an **Insurable** that already exists in Get Covered's system | **Insurable** ID or null
create_if_ambiguous | false | | true, false
communities_only | false | | true, false
titleless | false | | true, false

### Get or Create Response Parameters

## Insruable Rates 
    
```shell
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{ "insurable_id": 7221, "agency_id": 1, "billing_strategy_id": 9, "effective_date": "2020-07-01", "additional_insured": 0, "coverage_selections": [], "estimate_premium": true }'
     https://api.getcoveredinsurance.com/v2/policy-applications/get-coverage-options
```

```ruby
include HTTParty

get_insurable_rates_params = {
  :insurable_id => 7221,
  :agency_id => 1,
  :billing_strategy_id => 1,
  :effective_date => "2020-07-01",
  :additional_insured => 0,
  :coverage_selections => [],
  :estimate_premium => true
}

get_insurable_rates_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/policy-applications/get-coverage-options",
                                            body: get_insurable_rates_params.to_json,
                                            headers: {})
```

> The above command returns JSON structured like this:

```json
{
    "valid":true,
    "estimated_premium":"334.0",
    "estimated_premium_errors":null,
    "coverage_options":[
        {
            "uid":"1003",
            "title":"Coverage C - Contents",
            "options":[
                "10000.0",
                "15000.0",
                "20000.0",
                "25000.0",
                "30000.0",
                "35000.0",
                "40000.0",
                "45000.0",
                "50000.0",
                "55000.0",
                "60000.0",
                "65000.0",
                "70000.0",
                "75000.0",
                "80000.0",
                "85000.0",
                "90000.0",
                "95000.0",
                "100000.0"
            ],
            "category":"coverage",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1004",
            "title":"Coverage D - Loss of Use",
            "options":[
                "2000.0",
                "3000.0",
                "4000.0",
                "5000.0",
                "6000.0",
                "7000.0",
                "8000.0",
                "9000.0",
                "10000.0",
                "11000.0",
                "12000.0",
                "13000.0",
                "14000.0",
                "15000.0",
                "16000.0",
                "17000.0",
                "18000.0",
                "19000.0",
                "20000.0",
                "22000.0",
                "24000.0",
                "26000.0",
                "28000.0",
                "30000.0",
                "32000.0",
                "34000.0",
                "36000.0",
                "38000.0",
                "40000.0"
            ],
            "category":"coverage",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1005",
            "title":"Coverage E - Liability",
            "options":[
                "50000.0",
                "100000.0"
            ],
            "category":"coverage",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1006",
            "title":"Coverage F - Med Payment",
            "options":[
                "500.0",
                "1000.0",
                "2000.0"
            ],
            "category":"coverage",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1007",
            "title":"Pet Damage",
            "options":[
                "500.0"
            ],
            "category":"coverage",
            "requirement":"optional",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1008",
            "title":"Water Backup",
            "options":[
                "5000.0"
            ],
            "category":"coverage",
            "requirement":"optional",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1010",
            "title":"Replacement Cost",
            "options":null,
            "category":"coverage",
            "requirement":"required",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1043",
            "title":"Increased Property Limits - Jewelry, Watches",
            "options":[
                "1500.0",
                "2500.0",
                "5000.0"
            ],
            "category":"coverage",
            "requirement":"optional",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1060",
            "title":"AnimalLiabilityBuyBack",
            "options":[
                "50000.0",
                "100000.0",
                "300000.0"
            ],
            "category":"coverage",
            "requirement":"optional",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1011",
            "title":"Scheduled Personal Property",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1012",
            "title":"SPP Jewelry",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1013",
            "title":"SPP Furs",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1014",
            "title":"SPP Silverware",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1015",
            "title":"SPP Fine Arts",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1016",
            "title":"SPP Cameras",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1017",
            "title":"SPP Musical Equipment",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1018",
            "title":"SPP Golf Equipment",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1019",
            "title":"SPP Stamp Collections",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1065",
            "title":"IdentityFraud",
            "options":[
                "5000.0"
            ],
            "category":"coverage",
            "requirement":"optional",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1075",
            "title":"Bed Bug",
            "options":[
                "500.0",
                "750.0",
                "1000.0"
            ],
            "category":"coverage",
            "requirement":"optional",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"1076",
            "title":"Forced Entry Theft",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1077",
            "title":"Tenants Additional Protection",
            "options":null,
            "category":"coverage",
            "requirement":"optional",
            "options_type":"none",
            "options_format":"none"
        },
        {
            "uid":"1",
            "title":"AllOtherPeril",
            "options":[
                "250.0",
                "500.0",
                "1000.0"
            ],
            "category":"deductible",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"2",
            "title":"Theft",
            "options":[
                "500.0",
                "1000.0"
            ],
            "category":"deductible",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        },
        {
            "uid":"5",
            "title":"WindHail",
            "options":[
                "1000.0"
            ],
            "category":"deductible",
            "requirement":"required",
            "options_type":"multiple_choice",
            "options_format":"currency"
        }
    ]
}
```

# Policy Applications

## Introduction 

A *Policy Application* is the data structure used to store information required to generate a [*Policy Quote*](#policy-quote).  All *Policy Applications* will need information regarding the effective / expiration date, some information about the users who will be covered and data on the *Insurable* that will be covered.

## Get New Policy Application

```shell
curl -X POST \ 
     -H "Content-Type: application/json" \
     -d '{ "agency_id" : 1, "account_id" : 1, "policy_type_id" : 1, "policy_insurables_attributes" : [{ "id" : 1 }] }' \
     "http://api.getcoveredinsurance.com/v2/policy-applications/new"
```

```ruby
include HTTParty

new_policy_application_params = {
    :agency_id => 1,
    :account_id => 1, 
    :policy_type_id => 1, 
    :policy_insurables_attribute => [
      { :id => 1 } 
    ]
}

new_policy_application_request = HTTParty.post(":http => //api.getcoveredinsurance.com/v2/policy-applications/new",
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

Parameter | Required | Description
--------- | ------- | -----------
agency_id | true | ID provided for each **Agency** in Get Covered's system |
account_id | false | ID for property management organization which has ownership of the insurable to be covered 
policy_type_id | true | ID of the **Policy Type** from the [Policy Types](#policy-types) section |
policy_insurables_attributes | true | array of IDs of **Insurables** to be covered by policy.  Provided in the [Get or Create](#get-or-create) request 

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

## Create Policy Application

# Policy Quote

## Introduction

## Accept

# Policy

## Introduction

## Get Policy
