---
title: Get Covered Rent Guarantee Policy API

language_tabs: # must be one of https://git.io/vQNgJ
- shell
- ruby
- javascript

toc_footers:
  - <a href='/residential.html'>Residential</a>
  - <a href='/rent-guarantee.html'>Rent Guarantee</a>

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
