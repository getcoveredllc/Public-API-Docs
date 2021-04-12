---
title: Get Covered Residential Policy API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

search: true

code_clipboard: true
---

# Introduction

Welcome to the **Get Covered** Residential Policy API Documentation page.  The following sections will detail the process and options required to bind a policy for users on an external service.  In order to proceed you will need an **Access Token** provided by **Get Covered**.

All requests accept JSON and respond in kind.  Most endpoints are RESTful, although a few outliers do exist and are detailed in the sections below.

# Authentication

```shell
curl -X GET \
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB" \
     https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?policy_type=residential
```

```ruby
include HTTParty

billing_strategies_request = HTTParty.get("https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?policy_type=residential",
                                          headers: {
                                            :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                          })
```

```javascript
let response = fetch("https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?policy_type=residential", {
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    }   
});
```

Get Covered's API makes use of header based authorization.  The authorization value is a string comprised of two parts seperated by a colon.  A properly formated authorization header looks like `key:secret`.

# Policy Types
## Introduction 

Get Covered supports multiple products through a single API.  To differentiate between offerings, the *Policy Type* model helps our system designate which rules and integrations need to be called to bind the correct product.

## Usage 

A *Policy Type* is specified on a *Policy Application*, *Policy Quote* and a *Policy* but its **ID**.  Use the following reference to select which product the request is relevant to.

ID | Title | Slug | Description
--------- | ------- | ------- | ----------- 
1 | Renters | residential | An HO4 product covering renters at a residential address
2 | Master Policy | master-policy | TLL product
3 | Master Policy Coverage | master-policy-coverage | Coverage issued on behalf of a Master Policy
4 | Commercial | commercial | Business Owner Policy
4 | Rent Guarantee | rent-guarantee | Coverage for a renter or landlord in the event of a job loss.
5 | Renters Deposit Bond | deposit | A surety bond or security bond is a guarantee from a third party company that any unpaid rent and or damages to a rental property will be covered, up to a certain amount.

# Billing Strategies
## Introduction

A **Billing Strategy** stores data on how the **Premium** for a **Policy** is billed over the term of its coverage.  **Billing Stratgies** are specific to each **Agency** / **Carrier** / **Policy Type** combination.  The **ID** from these **Billing Strategies** is used in many requests to find Insurable Rates or tell a **Policy Application** how to divide the payments for it's premium.

## Get all Billing Strategies

```shell
curl -X GET \
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?policy_type=residential
```

```ruby
include HTTParty

billing_strategies_request = HTTParty.get("https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?policy_type=residential",
                                          headers: {
                                            :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                          })
```

```javascript
let response = fetch("https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?policy_type=residential", {
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    }   
});
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

### HTTP Request
`GET https://api.getcoveredinsurance.com/v2/sdk/billing-strategies?query_params`

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
policy_type | true | SLUG of **Policy Type**, defaults to residential | residential, commercial, rent-guarantee, deposit

# Insurables
## Introduction 

The *Insurable* model represents anything for which a policy can be issued to cover.  It may be a residential unit, house, car, business or other type of asset.  The documentation at this time will focus on properties that have a physical address.  They will be expanded as our API opens up to include more functionality.

## Get or Create 

```shell
curl -X POST \
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     -d '{ "address" : "270 Lafayette St, New York, NY 10012", "unit": "1207", "insurable_id": null, "create_if_ambiguous": true, "allow_creation" : true, "communities_only" : false, "titleless" : true }'
     https://api.getcoveredinsurance.com/v2/sdk/insurables/get-or-create
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

get_or_create_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/sdk/insurables/get-or-create",
                                      body: get_or_create_params.to_json,
                                      headers: {
                                        :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                      })
```

```javascript
let requestBody = {
    address: "270 Lafayette St, New York, NY 10012", 
    unit: "1207", 
    insurable_id: null, 
    create_if_ambiguous: true, 
    allow_creation: true, 
    communities_only: false, 
    titleless: true
};

let request = fetch("https://api.getcoveredinsurance.com/v2/sdk/insurables/get-or-create", {
    method: 'POST',
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    },
    body: JSON.stringify(requestBody)   
});
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

### HTTP Request
`POST https://api.getcoveredinsurance.com/v2/sdk/insurables/get-or-create`

### Get or Create Request Parameters
Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
address | true | The full address string for the mailing location of the insurable without the unit number | 
unit | true | Unique iditifier for unit at address (e.g. 101) | string, null, false
allow_creation | true | Allow Get Covered to create an insurable if one does not exist | true, false
insurable_id | false | The known **ID** of an **Insurable** that already exists in Get Covered's system | **Insurable** ID or null
communities_only | false | Restricts to Communities if **unit** is false | true, false
titleless | false | Pass as true when **unit** exists but has no number, e.g. for a house with its own street address | true, false

### Get or Create Response Parameters
Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
results_type | true | denotes how well the entered address is able to match records | no_match, confirmed_match or possible_match
results | true | Array of matches for address provided | 

### Get or Create Response Results Parameters
Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
id | true | The ID of the **Insurable** to be covered by a [**Policy Application**](#policy-applications), used in future requests | 
title | true | display title for **Insurable** | 
account_id | false | BIGING foreign key id of the **Property Management Account** that administers this **Insurable** | 
agency_id | true | BIGINT foreign key id of the **Agency** this **Insurable** belongs to | 
insurable_type_id | true | BIGINT foreign key id of the **InsurableType** this **Insurable** belongs to | 
enabled | true | boolean to denote whether this insurable is able to be interacted with through the API | true, false 
preferred_ho4 | true | Boolean to denote whether this insurable qualifies for preffered HO4 [**Insurable Rates**](#insurable-rates) | true, false 
category | false | general type of insurable | property or entity 
primary_address | true | Key pair object containing the data for the address in the database | 
primary_address[full] | true | full street address of the **Insurable** | 
primary_address[street_number] | true | number of address on a given street | 
primary_address[street_name] | true | name of street on which street number exists | 
primary_address[street_two] | false | apartment, unit or suiet number | 
primary_address[city] | true | locality of the street address | 
primary_address[state] | true | region in which the locality exists | 
primary_address[zip_code] | true | the postal code for the locality and region | 
primary_address[county] | false | the subdivision of the state, may overlap with multiple cities | 
primary_address[country] | false | the nation of which the state belongs | 
community | true | A community is a grouping of residential **Insurables** that share a common or multiple addresses | 
community[id] | true | The id of the parent **Insurable** to which the **Insurable** that will be covered by the policy belongs | 
community[title] | true | display title for **Insurable** | 
community[enabled] | true | boolean to denote whether this insurable is able to be interacted with through the API | true, false
community[preferred_ho4] | true | Boolean to denote whether this insurable qualifies for preffered HO4 [**Insurable Rates**](#insurable-rates) | true, false 
community[insurable_type_id] | true | BIGINT foreign key id of the **InsurableType** this **Insurable** belongs to | 

## Insruable Rates 
    
```shell
curl -X POST \
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     -d '{ "insurable_id": 7221, "billing_strategy_id": 9, "effective_date": "2020-07-01", "additional_insured": 0, "coverage_selections": [], "estimate_premium": true }'
     https://api.getcoveredinsurance.com/v2/sdk/policy-applications/get-coverage-options
```

```ruby
include HTTParty

get_insurable_rates_params = {
  :insurable_id => 7221,
  :billing_strategy_id => 1,
  :effective_date => "2020-07-01",
  :additional_insured => 0,
  :coverage_selections => [],
  :estimate_premium => true
}

get_insurable_rates_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/sdk/policy-applications/get-coverage-options",
                                            body: get_insurable_rates_params.to_json,
                                            headers: {
                                              :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                            })
```

```javascript
let requestBody = {
  insurable_id: 7221,
  billing_strategy_id: 1,
  effective_date: "2020-07-01",
  additional_insured: 0,
  coverage_selections: [],
  estimate_premium: true
};

let request = fetch("https://api.getcoveredinsurance.com/v2/sdk/policy-applications/get-coverage-options", {
    method: 'POST',
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    },
    body: JSON.stringify(requestBody)   
});
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

The **Insurable** Insurable Rates request finds available coverages for an Insurable identified during the [Get Or Create](#get-or-create) request.  This section is only for Renters Policies ([Policy Type ID: 1](#policy-types)).
   
### HTTP Request
`POST https://api.getcoveredinsurance.com/v2/sdk/policy-applications/get-coverage-options`

### Get Coverage Options Request Parameters
Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
insurable_id | true | The **ID** of the [**Insurable**](#insurables) identified int the [Get or Create](#get-or-create) request |
billing_strategy_id | true | The **ID** of the [**Billing Strategy**](#get-all-billing-strategies) being used on the [**Policy Application**](#policy-applications) |
effective_date | true | Date coverage will begin | 
additional_insured | true | Number of additional individuals that will be covered under policy, defaults to 0 | 1, 2, 3, 4, 5, 6, 7 
coverage_selections | false | Array of hashes, see example below |
estimate_premium | true | Boolean to return an estimated premium | true, false

### Coverage Selections
To add a coverage to the coverage selections array, use one of the following hash formats.  Resending the Get Insurable Rates request with **coverage_selections** may change the available rates.

Adding a Coverage that has multiple options:

`{ "category" : "coverage", "uid": "1003", "selection": "30000.0" }`

Adding a Coverage that has no options:

`{ "category" : "coverage", "uid" : "1076", "selection" : true }`

### Get Coverage Options Response Parameters
Parameter  | Description
--------- | -----------
valid | Informs requester whether or not a valid request has been made | 
estimated_premium | Included if _estimate_premium_ was included in request.  Value in dollars and cents |
estimated_premium_errors | Contains errors related to coverage selections for _estimated_premium_

### Insurable Rate Parameters
Parameter | Description | Options
--------- | ----------- | -----------
uid | unique identifier for insurable rate |
title | display title for insurable rate |
options | choices of coverage for a specific insurable rate |
category | dictates whether listing is a coverage or deductible option | coverage, deductible
requirement | indicates whether or not coverage is a required choice | required, optional
options_type | what type of data options for coverage selection are | currency, none
options_format | format of options returned for coverage | multiple_choice, none

# Policy Applications

## Introduction 

A *Policy Application* is the data structure used to store information required to generate a [*Policy Quote*](#policy-quote).  All *Policy Applications* will need information regarding the effective / expiration date, some information about the users who will be covered and data on the *Insurable* that will be covered.

## Get New Policy Application

```shell
curl -X POST \ 
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     -d '{ "account_id" : 1, "policy_type_id" : 1, "policy_insurables_attributes" : [{ "id" : 1 }] }' \
     "https://api.getcoveredinsurance.com/v2/sdk/policy-applications/new"
```

```ruby
include HTTParty

new_policy_application_params = {
    :account_id => 1, 
    :policy_type_id => 1, 
    :policy_insurables_attributes => [
      { :id => 1 } 
    ]
}

new_policy_application_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/sdk/policy-applications/new",
                                               body: new_policy_application_params.to_json,
                                               headers: {
                                                 :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                               })
```

```javascript
let requestBody = {
    account_id: 1, 
    policy_type_id: 1, 
    policy_insurables_attributes: [
      { id: 1 } 
    ]
};

let request = fetch("https://api.getcoveredinsurance.com/v2/sdk/policy-applications/new", {
    method: 'POST',
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    },
    body: JSON.stringify(requestBody)   
});
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
    "policy_users_attributes":[
        {
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
    ]
}
```

This endpoint retrieves a new Policy Application.  The response object is used to [create](#create-policy-application) the PolicyApplication which will return a [**Policy Quote**](#policy-quote).

### HTTP Request

`POST https://api.getcoveredinsurance.com/v2/sdk/policy-applications/new`

### Policy Request Application Parameters

Parameter | Required | Description
--------- | ------- | -----------
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
questions | true | An array of hashes that represent elegibility questions for a given carrier / [**Policy Type**](#policy-type) combination |
carrier_id | true | ID of the **Carrier** issueing coverage for this Policy Application | 
policy_type_id | true | ID of the [**Policy Type**](#policy-type) being applied for |  
agency_id | true | ID of your agency | 
account_id | false | ID of the **Account** that controlls the Insurable |
billing_strategy_id | true | Selected [**Billing Strategy**](#billing-strategies) ID for desired payment terms | [Dynamic](#get-all-billing-strategies)
policy_insurables_attributes | true | An array of hashes that contain the ID of any **Insurables** to be covered from the [Get or Create](#get-or-create) request | 
policy_users_attributes | true | Array of hashes for users covered by this Policy Application | 

### Policy Users Attributes

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
primary | true | 1 per Policy Application.  Denotes primary contact | true, false
spouse | true | Indicates user is married to someone else listed on policy application, this can affect premium in some states | true, false
user_attributes["email"] | true | valid email address for user | 
user_attributes["profile_attributes"]["first_name"] | true | Given name of user | 
user_attributes["profile_attributes"]["last_name"] | true | Family name of user |
user_attributes["profile_attributes"]["contact_phone"] | true | 10 digit phone number |
user_attributes["profile_attributes"]["birth_date"] | true | Date string of users birthday, primary insured must be older than 18 |
user_attributes["profile_attributes"]["job_title"] | false | Occupational title of user |


## Create Policy Application
```shell
curl -X POST \ 
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     -d '{"reference":null,"external_reference":null,"effective_date":"6/1/2021","expiration_date":"6/1/2022","status":"complete","status_updated_on":null,"fields":[{"title":"Number of Insured","value":1,"options":[1,2,3,4,5,6,7,8],"answer_type":"INTEGER","default_answer":1}],"questions":[],"coverage_selections":[{"categy":"coverage","uid":"1003","selection":"30000.0"},{"categy":"coverage","uid":"1004","selection":"30000.0"},{"categy":"coverage","uid":"1006","selection":"2000.0"},{"category":"coverage","uid":"1076","selection":true},{"categy":"deductible","uid":"1","selection":"1000.0"},{"categy":"deductible","uid":"2","selection":"1000.0"},{"categy":"deductible","uid":"5","selection":"1000.0"}],"carrier_id":5,"policy_type_id":1,"account_id":1,"billing_strategy_id":1,"policy_insurables_attributes":[{"id":1}],"policy_users_attributes":[{"primary":true,"spouse":false,"user_attributes":{"email":"johndoe@gmail.com","profile_attributes":{"first_name":"John","last_name":"Doe","contract_phone":"3105551234","birth_date":"1/1/1990","job_title":null}}}]}'
     https://api.getcoveredinsurance.com/v2/sdk/policy-applications
```
```ruby
include HTTParty

completed_application = {
    :reference => nil,
    :external_reference => nil,
    :effective_date => "6/1/2021",
    :expiration_date => "6/1/2022",
    :status => "complete",
    :status_updated_on => nil,
    :fields => [
        {
            :title => "Number of Insured",
            :value => 1,
            :options => [1, 2, 3, 4, 5, 6, 7, 8],
            :answer_type => "INTEGER",
            :default_answer => 1
        }
    ],
    :questions=>[],
    :coverage_selections => [
        { :category => "coverage", :uid => "1003", :selection => "30000.0" },
        { :category => "coverage", :uid => "1004", :selection => "30000.0"},
        { :category => "coverage", :uid => "1006", :selection => "2000.0"},
        { :category => "coverage", :uid => "1076", :selection => true},
        { :category => "deductible", :uid => "1", :selection => "1000.0"},
        { :category => "deductible", :uid => "2", :selection => "1000.0"},
        { :category => "deductible", :uid => "5", :selection => "1000.0"}
    ],
    :carrier_id => 5,
    :policy_type_id => 1,
    :account_id => 1,
    :billing_strategy_id => 1,
    :policy_insurables_attributes => [
        { :id => 1 }
    ],
    :policy_users_attributes => [
        {
            :primary => true,
            :spouse => false,
            :user_attributes => {
                :email => "johndoe@gmail.com",
                :profile_attributes => {
                    :first_name => "John",
                    :last_name => "Doe",
                    :contract_phone => "3105551234",
                    :birth_date => "1/1/1990",
                    :job_title => nil
                }
            }
        }
    ]
}

policy_application_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/sdk/policy-applications",
                                           body: completed_application.to_json,
                                           headers: {
                                             :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                           })
```

```javascript
let requestBody = {
    reference: null,
    external_reference: null,
    effective_date: "6/1/2021",
    expiration_date: "6/1/2022",
    status: "complete",
    status_updated_on: null,
    fields: [
        {
            title: "Number of Insured",
            value: 1,
            options: [1, 2, 3, 4, 5, 6, 7, 8],
            answer_type: "INTEGER",
            default_answer: 1
        }
    ],
    questions: [],
    coverage_selections: [
        { category: "coverage", uid: "1003", selection: "30000.0" },
        { category: "coverage", uid: "1004", selection: "30000.0"},
        { category: "coverage", uid: "1006", selection: "2000.0"},
        { category: "coverage", uid: "1076", selection: true},
        { category: "deductible", uid: "1", selection: "1000.0"},
        { category: "deductible", uid: "2", selection: "1000.0"},
        { category: "deductible", uid: "5", selection: "1000.0"}
    ],
    carrier_id: 5,
    policy_type_id: 1,
    account_id: 1,
    billing_strategy_id: 1,
    policy_insurables_attributes: [
        { id: 1 }
    ],
    policy_users_attributes: [
        {
            primary: true,
            spouse: false,
            user_attributes: {
                email: "johndoe@gmail.com",
                profile_attributes: {
                    first_name: "John",
                    last_name: "Doe",
                    contract_phone: "3105551234",
                    birth_date: "1/1/1990",
                    job_title: null
                }
            }
        }
    ]
};

let request = fetch("https://api.getcoveredinsurance.com/v2/sdk/policy-applications", {
    method: 'POST',
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    },
    body: JSON.stringify(requestBody)   
});
```

Using the data returned in [Get New Policy Application](#get-new-policy-application), the ID of the desired [**Billing Strategy**](#billing-strategies) and the hashes of the relevant [**Insurable Rates**](#insruable-rates) the Policy Application object is complete and considered ready to quote.  A successful create call will return a [**Policy Quote**](#policy-quote) with a final premium and list of invoices with their required due dates to present to the user.

### HTTP Request

`POST https://api.getcoveredinsurance.com/v2/sdk/policy-applications`

# Policy Quote

## Introduction

> Policy Quote JSON Response to [Create Policy Application](#create-policy-application)

```json
{
   "quote":{
      "id":115,
      "status":"quoted",
      "premium":{
         "id":86,
         "base":17200,
         "taxes":0,
         "total_fees":14500,
         "total":31700,
         "enabled":false,
         "enabled_changed":null,
         "policy_quote_id":115,
         "policy_id":null,
         "billing_strategy_id":4,
         "commission_strategy_id":null,
         "created_at":"2020-01-30T10:45:06.232Z",
         "updated_at":"2020-01-30T10:45:06.232Z",
         "estimate":null,
         "calculation_base":29200,
         "deposit_fees":2500,
         "amortized_fees":12000,
         "carrier_base":17200
      }
   },
   "invoices":[
      {
         "id":945,
         "number":"LOFQQKR2UX0X",
         "status":"quoted",
         "status_changed":null,
         "description":null,
         "due_date":"2020-01-30",
         "available_date":"2020-02-06",
         "term_first_date":null,
         "term_last_date":null,
         "renewal_cycle":0,
         "total":-2070,
         "subtotal":6426,
         "tax":0,
         "tax_percent":"0.0",
         "system_data":{

         },
         "amount_refunded":0,
         "amount_to_refund_on_completion":0,
         "has_pending_refund":false,
         "pending_refund_data":{

         },
         "user_id":409,
         "created_at":"2020-01-30T10:45:06.301Z",
         "updated_at":"2020-01-30T10:45:06.301Z",
         "policy_id":null,
         "policy_quote_id":115
      },
      {
         "id":946,
         "number":"LTN77CRBN2MI",
         "status":"quoted",
         "status_changed":null,
         "description":null,
         "due_date":"2020-02-29",
         "available_date":"2020-03-07",
         "term_first_date":null,
         "term_last_date":null,
         "renewal_cycle":0,
         "total":3070,
         "subtotal":2070,
         "tax":0,
         "tax_percent":"0.0",
         "system_data":{

         },
         "amount_refunded":0,
         "amount_to_refund_on_completion":0,
         "has_pending_refund":false,
         "pending_refund_data":{

         },
         "user_id":409,
         "created_at":"2020-01-30T10:45:06.379Z",
         "updated_at":"2020-01-30T10:45:06.379Z",
         "policy_id":null,
         "policy_quote_id":115
      }
   ],
   "user":{
      "id":409
   }
}
```

A Policy Quote object is returned from the successful creation of a complete [**Policy Application**](#policy-applications).  It contains all information needed to bill the User for the correct amount along with a schedule of upcoming payments, tax information and associated fees.  Policy Quotes are not updated, to get a new Premium the original [**Policy Application**](#policy-applications) must be resubmitted with updated values.

### Policy Quote Attributes

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
id | true | Get Covered's ID for this **Policy Quote** | 
status | true | current state of the **Policy Quote** being reviewed | awaiting_estimate, estimated, quoted, quote_failed, accepted, abandoned, expired, error 
premium | true | Hash object of [**Policy Premium**](#policy-premium-attributes) |
invoices | true | Array of hashes of [**Invoices**](#invoices-attributes) |
user | true | ID of primary use from [**Policy Application**](#policy-applications) |

### Policy Premium Attributes

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
id | true | **Policy Premium** ID | 
base | true | base premium amount in cents | 
taxes | true | taxes required for this policy in cents | 
total_fees | true | fees associated with this policy in cents | 
total | true | amount of all invoices the customer is due to pay in cents | 
enabled | true | indicates which premium to use for commercial policies that may recieve multiple premiums during quoting | true, false
enabled_changed | true | date of last change to enabled | 
policy_quote_id | true | ID of parent **Policy Quote** | 
policy_id | true | ID of policy, assigned by system after bind | 
billing_strategy_id | true | ID of [**Billing Strategy**](#billing-strategies) associated with Policy | 
commission_strategy_id | true | ID of **Commission Strategy** used to determine profit splits | 
created_at | true | DateTime **Policy Premium** was created | 
updated_at | true | DateTime **PolicyPremium** was last updated | 
estimate | true | Temporary amount to display to user before quoting is complete | 
calculation_base | true | Dollar amount in cents used in calculation of invoice splits | 
deposit_fees | true | Dollar amount in cents of fees attached to first invoice | 
amortized_fees | true | Dollar amount in cents of fees split between all invoices | 
carrier_base | true | Dollar amount in cents **Carrier** expects to collect over life of the **Policy** | 

### Invoices Attributes

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
id | true | unique identifier of invoice |
number | true | Get Covered's internal unique identifier for invoices to be shown to users |
status | true | current invoice status | quoted, upcoming, available, processing, complete, missed, canceled, managed_externally
status_changed | true | date of last status change |
description | false | |
due_date | true | date by which payment is due |
available_date | true | date on which invoice is available to be paid |
term_first_date | true | first day of term for billing cycle |
term_last_date | true | last day of term for billing cycle |
renewal_cycle | true | number of renewals this billing cycle is on (e.g. 0 for first year, 1 for second) |
total | true | dollar amount in cents due |
subtotal | true | dollar amount of invoice before taxes |
tax | true | dollar amount in cents of taxes to be charged |
tax_percent | true | tax percentage relevant to this transaction |
system_data | false | jsonb column of data relevant to this invoice for system tasks |
amount_refunded | false | dollar amount in cents of invoice refunded |
amount_to_refund_on_completion | false | dollar amount in cents of invoice to be refunded after collection |
has_pending_refund | false | boolean on whether a pending refund can be issued for this invoice | true, false
pending_refund_data | false | jsonb column of relevant system data for refunding invoice |
user_id | true | ID of **User** who will pay this invoice | 
created_at | true | DateTime invoice was created | 
updated_at | true | DateTime invoice was last updated | 
policy_id | true | ID of [**Policy**](#policy) once bound | 
policy_quote_id | true | ID of [**Policy Quote**](#policy-quotes) invoice belongs to | 

## Accept

# Policy

## Introduction

The **Policy** model represents quotes that have been accepted and had payment successfully collected for.  A **Policy** can cover any number of insurables with Insurable Rates selected furing the [**Policy Application**](#policy-applications) process.  A **Policy** is considered *BOUND* until it passes its expiration date without renewal, it has been cancelled or lapsed due to lack of payment longer than the carrier's terms allow.

## Get All Policies

```shell
curl -X GET \ 
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     "https://api.getcoveredinsurance.com/v2/sdk/policies"
```
```ruby
include HTTParty

get_policy_request = HTTParty.get("https://api.getcoveredinsurance.com/v2/sdk/policies",
                                  headers: {
                                    :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                  })
```
```javascript
let response = fetch("https://api.getcoveredinsurance.com/v2/sdk/policies", {
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    }   
});
```

> The above command returns JSON structured like this:

```json
[
  {
    "message": "example coming soon..."
  } 
]
```


## Get Policy

```shell
curl -X GET \ 
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     "https://api.getcoveredinsurance.com/v2/sdk/policies/:policy-number"
```
```ruby
include HTTParty

get_policy_request = HTTParty.get("https://api.getcoveredinsurance.com/v2/sdk/policies/:policy-number",
                                  headers: {
                                    :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                  })
```
```javascript
let response = fetch("https://api.getcoveredinsurance.com/v2/sdk/policies/:policy-number", {
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    }   
});
```
> The above command returns JSON structured like this:

```json
{
  "id":1,
  "number":"PRH9116960",
  "effective_date":"2020-01-10",
  "expiration_date":"2021-01-10",
  "status":"BOUND",
  "billing_behind_since":null,
  "billing_dispute_count":null,
  "billing_dispute_status":null,
  "billing_enabled":true,
  "billing_status":"CURRENT",
  "last_payment_date":"2020-01-04",
  "next_payment_date":"2020-07-10",
  "cancellation_code":null,
  "cancellation_date_date":null,
  "last_renewed_on":null,
  "policy_in_system":true,
  "auto_pay":true,
  "auto_renew":true,
  "renew_count":null,
  "account_id":1,
  "agency_id":1,
  "carrier_id":1,
  "policy_type_id":1,
  "policy_type_title":"Residential Policy",
  "created_at":"2020-01-04T00:56:49.690Z",
  "policy_users_attributes": [
    {
      "primary":true,
      "spouse":false,
      "user_attributes": {
        "email":"johndoe@gmail.com",
        "enabled":false,
        "id":2,
        "user_in_system":null,
        "marital_status":"single",
        "invitation_accepted_at":null,
        "profile_attributes":{
          "birth_date":"1955-09-07",
          "contact_email":null,
          "contact_phone":null,
          "first_name":"John",
          "full_name":"John Doe",
          "id":60,
          "job_title":null,
          "last_name":"Doe",
          "middle_name":null,
          "suffix":null,
          "title":null
        }      
      }
    }
  ],
  "policy_coverages":[
    {
      "id":2,
      "title":null,
      "designation":"Coverage D",
      "limit":0,
      "deductible":0,
      "policy_id":1,
      "policy_application_id":1,
      "created_at":"2020-01-20T12:53:25.514Z",
      "updated_at":"2020-01-20T12:53:25.514Z",
      "enabled":true,
      "special_deductible":null
    },
    {
      "id":1,
      "title":null,
      "designation":"Coverage C",
      "limit":200,
      "deductible":100,
      "policy_id":1,
      "policy_application_id":1,
      "created_at":"2020-01-20T12:35:48.229Z",
      "updated_at":"2020-02-14T06:52:04.733Z",
      "enabled":true,
      "special_deductible":null
    }
  ],
  "primary_insurable":{
    "category":"property",
    "covered":false,
    "enabled":true,
    "id":2,
    "insurable_id":1,
    "insurable_type_id":4,
    "title":"101",
    "parent_community":{
      "category":"property",
      "covered":false,
      "enabled":true,
      "id":1,
      "insurable_id":null,
      "insurable_type_id":1,
      "title":"Narchost Gardens"
    }
  },
  "premium":{
    "id":1,
    "base":25900,
    "taxes":0,
    "total_fees":4500,
    "total":30400,
    "enabled":false,
    "enabled_changed":null,
    "policy_quote_id":1,
    "policy_id":1,
    "billing_strategy_id":2,
    "commission_strategy_id":null,
    "created_at":"2020-01-04T00:56:34.262Z",
    "updated_at":"2020-01-04T00:56:50.137Z",
    "estimate":null,
    "calculation_base":27900,
    "deposit_fees":2500,
    "amortized_fees":2000,
    "carrier_base":25900,
    "special_premium":0,
    "include_special_premium":false
  },
  "documents":[
    {
      "id":1,
      "filename":"1256790-evidence-of-insurance.pdf",
      "url":"http://localhost:3000/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBCZz09IiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--f017f1dc3782b6333cb757cfd9fe7108c80f6dcb/1256790-evidence-of-insurance.pdf",
      "created_at":"2020-01-04T00:38:20.026Z"
    },
    {
      "id":2,
      "filename":"1256790-evidence-of-insurance.pdf",
      "url":"http://localhost:3000/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBCdz09IiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--0b2684c09eb5a9b719ec3b223ac17d0fed40f9ff/1256790-evidence-of-insurance.pdf",
      "created_at":"2020-01-04T00:38:20.026Z"
    },
    {
      "id":3,
      "filename":"1256790-evidence-of-insurance.pdf",
      "url":"http://localhost:3000/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBDQT09IiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--023fd4857a3026ae5337bc74ee87dbd7508b9f39/1256790-evidence-of-insurance.pdf",
      "created_at":"2020-01-04T00:38:20.026Z"
    },
    {
      "id":4,
      "filename":"1256790-evidence-of-insurance.pdf",
      "url":"http://localhost:3000/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBDUT09IiwiZXhwIjpudWxsLCJwdXIiOiJibG9iX2lkIn19--efa9e609c32dfdc7a7567df6361a0f56d59ac015/1256790-evidence-of-insurance.pdf",
      "created_at":"2020-01-04T00:38:20.026Z"
    }
  ]
}
```

Finds a **Policy** by its number.  Response includes data on associated **Users**, [**Insurables**](#insurables), any **Documents** and it's **Premium**


### HTTP Request

`GET https://api.getcoveredinsurance.com/v2/sdk/policies/:policy-number`

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
id | true | | 
number | true | | 
effective_date | true | | 
expiration_date | true | | 
status | true | | AWAITING_PAYMENT, AWAITING_ACH, PAID, BOUND, BOUND_WITH_WARNING, BIND_ERROR, BIND_REJECTED, RENEWING, RENEWED, EXPIRED, CANCELLED, REINSTATED, EXTERNAL_UNVERIFIED, EXTERNAL_VERIFIED
billing_behind_since | false | | 
billing_dispute_count | false | | 
billing_dispute_status | true | | UNDISPUTED, DISPUTED, AWATING_POSTDISPUTE_PROCESSING, NOT_REQUIRED
billing_enabled | true | | true, false 
billing_status | true | | CURRENT, BEHIND, REJECTED, RESCINDED, ERROR, EXTERNAL
last_payment_date | true | | 
next_payment_date | true | | 
cancellation_code | false | | 
cancellation_date | false | | 
last_renewed_on | false | | 
policy_in_system | true | | true, false 
auto_pay | true | | true, false
auto_renew | true | | true, false 
renew_count | false | | 
account_id | false | | 
agency_id | true | | 
carrier_id | true | | 
policy_type_id | true | | 
policy_type_title | false | | 
created_at | true | | 

## Cancel Policy

Coming Soon

# Errors

Coming Soon
