---
title: Get Covered Rent Guarantee Policy API

language_tabs: # must be one of https://git.io/vQNgJ
- shell
- ruby
- javascript

toc_footers:
  - <a href='/'>Residential</a>
  - <a href='/rent-guarantee'>Rent Guarantee</a>

search: true

code_clipboard: true
---

# Introduction

Welcome to the **Get Covered** Rent Guarantee Policy API Documentation page.  The following sections will detail the process and options required to bind a policy for users on an external service.  In order to proceed you will need an **Access Token** provided by **Get Covered**.

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

# Policy Applications

## Introduction 

A *Policy Application* is the data structure used to store information required to generate a [*Policy Quote*](#policy-quote).  All *Policy Applications* will need information regarding the effective / expiration date, some information about the users who will be covered and data on the *Insurable* that will be covered.

## Create for Redirect

```shell
curl -X POST \ 
     -H "Content-Type: application/json" \
     -H "Authorization: BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"  \
     -d '{"policy_type_id": 4, "redirect_url":"https://some-url.getcoveredinsurance.com", "fields":{"monthly_rent":500, "guarantee_option":3},"users_attributes": [{"email": "user@gmail.com","profile_attributes": {"first_name": "John","last_name": "Doe","contact_phone": "3105551234"}}]}' \
     "https://api.getcoveredinsurance.com/v2/sdk/policy-applications/redirect"
```

```ruby
include HTTParty

application_for_redirect = {
  :policy_type_id => 4,
  :redirect_url => "https://some-url.getcoveredinsurance.com",
  :fields => {
    :monthly_rent => 500,
    :guarantee_option => 3
  },
  :users_attributes => [
    {
      :email => "user@gmail.com",
      :profile_attributes => {
        :first_name => "John",
        :last_name => "Doe",
        :contact_phone => "3105551234"
      }
    }
  ]
}

policy_application_redirect_request = HTTParty.post("https://api.getcoveredinsurance.com/v2/sdk/policy-applications/redirect",
                                                    body: application_for_redirect.to_json,
                                                    headers: {
                                                      :authorization => "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
                                                    })
```

```javascript
let application_for_redirect = {
  policy_type_id: 4,
  redirect_url: "https://some-url.getcoveredinsurance.com",
  fields: {
    monthly_rent: 500,
    guarantee_option: 3
  },
  users_attributes: [
    {
      email: "user@gmail.com",
      profile_attributes: {
        first_name: "John",
        last_name: "Doe",
        contact_phone: "3105551234"
      }
    }
  ]
}

let request = fetch("https://api.getcoveredinsurance.com/v2/sdk/policy-applications/redirect", {
    method: 'POST',
    headers: {
        Authorization: "BxrcM2+lhQS6Jt41GYsVDQ==:4Aa1tM+VVZfIpzd1iy1osfiDlwhbP7LZaJmP+2hs39SRMDvB"
    },
    body: JSON.stringify(application_for_redirect)   
});
```

> The above command returns JSON structured like this:

```json
{
  "id":1,
  "policy_type_id": 4,
  "fields":{
    "monthly_rent":500,
    "guarantee_option":3
  },
  "users_attributes":[
    {
      "id":1,
      "email":"user@gmail.com",
      "profile_attributes":{
        "id":1,
        "first_name":"John",
        "last_name":"Doe",
        "contact_phone":"3105551234"
      }
    }
  ],
  "redirect_url":"https://some-url.getcoveredinsurance.com/residential/:policy_id"
}
```
Some partners prefer to start a **Policy Application** on their own services but have the end user complete it on **Get Covered's** site.  This process has the advantage that Get Covered will manage and complete billing and policy bind tasks.  Creating a **Policy Aplication** for redirect does not require the use of [**Get New Policy Application**](#get-new-policy-application)

### HTTP Request

`POST https://api.getcoveredinsurance.com/v2/sdk/policy-applications/redirect`

### Policy Application for Redirect Request Parameters

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
policy_type_id | true | ID of [**Policy Type**](#usage) for relevant product | 
redirect_url | true | Base url which will be redirected to after create | 
fields | true | hash object of key value pairs necessary to begin **Policy Application** | 
fields["monthly_rent"] | true | The dollar amount of monthly rent obligation to be covered by **Policy Application** | 
fields["guarantee_option"] | true | The numeric number of months the **Policy** will pay out the monthly rent | 3, 6, 12 
users_attributes | true | Array of hash objects for users that will be attached to **Policy Application**, requires at least 1 | 
users_attributes["email"] | true | Email address where user attached to this **Policy Application** can be contacted | 
users_attributes["profile_attributes"] | true | Hash object of attributes for user. | 
users_attributes["profile_attributes"]["first_name"] | true | Given name of user | 
users_attributes["profile_attributes"]["last_name"] | true | Family name of user | 
users_attributes["profile_attributes"]["contact_phone"] | true | 10 digit numeric phone number where user can be reached | 

### Policy Application for Redirect Response Parameters

Parameter | Required | Description | Options
--------- | ------- | ----------- | -----------
id | true | Get Covered's unique ID for this **Policy Application** | 
policy_type_id | true | ID of [**Policy Type**](#usage) for relevant product | 
redirect_url | true | Base url from request with the required identifier to properly look up the **Policy Application** from the Get Covered front end | 
fields | true | hash object of key value pairs necessary to begin **Policy Application** | 
fields["monthly_rent"] | true | The dollar amount of monthly rent obligation to be covered by **Policy Application** | 
fields["guarantee_option"] | true | The numeric number of months the **Policy** will pay out the monthly rent | 3, 6, 12 
users_attributes | true | Array of hash objects for users that will be attached to **Policy Application**, requires at least 1 | 
users_attributes["email"] | true | Email address where user attached to this **Policy Application** can be contacted | 
users_attributes["profile_attributes"] | true | Hash object of attributes for user. | 
users_attributes["profile_attributes"]["first_name"] | true | Given name of user | 
users_attributes["profile_attributes"]["last_name"] | true | Family name of user | 
users_attributes["profile_attributes"]["contact_phone"] | true | 10 digit numeric phone number where user can be reached | 

