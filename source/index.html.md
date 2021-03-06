---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby

toc_footers:
  - <a href='mailto:brandon@staffingreferrals.com'>Sign Up for a Developer Key</a>

#includes:
#  - errors

search: true
---

# Introduction

Welcome to the Staffing Referrals API! You can use our API to access API endpoints, which allow retrieving and updating information on Referral Leads and Users.  You are also able to manually create Referral Leads.

The API is designed to be [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) and returns data in the JSON format.  Parameters are passed as simple HTTP parameters.  The API is accessible by any system that can make HTTP requests and parse JSON.

We have examples in Shell and Ruby. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This API is currently under development.  If you need a feature we don't currently support or would like some examples in a specific language, please contact your account executive!

# Authentication

> To authorize, use this code:

```ruby
require 'curb' # Curl access in ruby

http = Curl.get('https://example.staffingreferrals.com/api/v1/ENDPOINT_HERE', PARAMS) do |http|
  http.headers['Authorization'] = "YOUR_TOKEN_HERE"
end

json = JSON.parse(http.body)
```

```shell
# With shell, you can just pass the correct header with each request
curl "https://example.staffingreferrals.com/api/v1/ENDPOINT_HERE" \
  -H "Authorization: YOUR_TOKEN_HERE"
```


Staffing Referrals allows certain admin accounts to access the API.  It uses API access tokens to allow access to the API.  **Please contact your account executive to set up an account and obtain your token.**

Staffing Referrals expects for the access token to be included in all API requests to the server in a header that looks like the following:

`Authorization: YOUR_TOKEN_HERE`

<aside class="notice">
You must replace <code>YOUR_TOKEN_HERE</code> with your access token.
</aside>

# General Return Structure
The Staffing Referrals API will return a JSON object with `status` and either `data` or `message` elements.  When the request was successful, the status will be `"success"` and the relevant data will be provided.  For example:

`
{"status":"success", "data":{"id":10,"status":"Lead",...}}
`

```json
/* Example success */
{
  "status":"success",
  "data":[
    {
      "id":10,
      "status":"Lead",
      ...      
    }
  ]
}
```

In the event of an error, the returned status will be `"error"` and the message will indicate what went wrong.  For example:

`
{"status":"error", "message":"Invalid status"}
`

```json
/* Example error */
{
  "status":"error",
  "message":"Invalid status"
}
```

In some situations, you might encounter an HTTP response other than 200 OK.  In the event your endpoint is incorrect or there is a system issue, the HTTP response code will indicate the problem.

# Referrals

## Get All Referrals

```ruby
require 'curb' # Curl access in ruby

http = Curl.get('https://example.staffingreferrals.com/api/v1/referrals', {}) do |http|
  http.headers['Authorization'] = "YOUR_TOKEN_HERE"
end

json = JSON.parse(http.body)
```

```shell
curl "https://example.staffingreferrals.com/api/v1/referrals" \
  -H "Authorization: YOUR_TOKEN_HERE"
```

> The above command returns JSON structured like this:

```json
{
  "status":"success",
  "data":[
    {
      "id":10,
      "status":"Lead",
      "created_at":"2001-02-02 21:05:06 UTC",
      "updated_at":"2001-02-02 21:05:06 UTC",
      "is_manual":false,
      "has_resume":false,
      "previous_referral_ids":"",
      "campaign":{
        "id":48,
        "status":"Live",
        "minimum_term":null,
        "minimum_term_units":null,
        "applicant_bonus":"$500",
        "ambassador_bonus":"$42",
        "job_id":null,
        "job_title":null,
        "job_category":null
      },
      "applicant":{
        "id":37,
        "status":"Active",
        "email":"immanuel@windlervandervort.info",
        "first_name":"Candy",
        "last_name":"Date",
        "phone":null
      },
      "ambassador":{
        "id":36,
        "status":"Active",
        "email":"bailey.wiza@mosciskimuller.co",
        "first_name":"Pro",
        "last_name":"Motor",
        "phone":null
      },
      "recruiter":{
        "id":35,
        "status":"Active",
        "email":"rosalyn@leuschke.org",
        "first_name":"Recruity",
        "last_name":"Recruiter",
        "phone":null
      }
    }
  ]
}
```

This endpoint retrieves all referral leads, optionally filtered to only get recently updated or created ones.

### HTTP Request

`GET https://example.staffingreferrals.com/api/v1/referrals`

### Query Parameters

Parameter | Description
--------- | -----------
since | An **optional** ISO-8601 formatted date to provide a lower bound for updates that you are interested in.  For example, if you want all referral leads created or updated since noon on the Fourth of July in 2017 Denver Time, use `2017-07-04T12:00:00-07:00`.


<aside class="success">
Remember — you must provide the Authorization header for all requests.
</aside>

## Update Referral

```ruby
require 'curb' # Curl access in ruby

http = Curl.post('https://example.staffingreferrals.com/api/v1/referrals/10',
                 {status: "discarded"}) do |http|
  http.headers['Authorization'] = "YOUR_TOKEN_HERE"
end

json = JSON.parse(http.body)
```

```shell
curl "https://example.staffingreferrals.com/api/v1/referrals/10" \
  -H "Authorization: YOUR_TOKEN_HERE" \
  -d "status=discarded" \
  -X POST 
```

> The above command returns JSON structured like this:

```json
{
  "status":"success",
  "data":{
    "id":10,
    "status":"Discarded",
    "created_at":"2001-02-02 21:05:06 UTC",
    "updated_at":"2001-02-02 21:05:06 UTC",
    "is_manual":false,
    "has_resume":false,
    "previous_referral_ids":"",
    "campaign":{
      "id":48,
      "status":"Live",
      "minimum_term":null,
      "minimum_term_units":null,
      "applicant_bonus":"$500",
      "ambassador_bonus":"$42",
      "job_id":null,
      "job_title":null,
      "job_category":null
    },
    "applicant":{
      "id":37,
      "status":"Active",
      "email":"immanuel@windlervandervort.info",
      "first_name":"Candy",
      "last_name":"Date",
      "phone":null
    },
    "ambassador":{
      "id":36,
      "status":"Active",
      "email":"bailey.wiza@mosciskimuller.co",
      "first_name":"Pro",
      "last_name":"Motor",
      "phone":null
    },
    "recruiter":{
      "id":35,
      "status":"Active",
      "email":"rosalyn@leuschke.org",
      "first_name":"Recruity",
      "last_name":"Recruiter",
      "phone":null
    }
  }
}
```

This endpoint updates a specific referral lead.  

### HTTP Request

`POST https://example.staffingreferrals.com/api/v1/referrals/REFERRAL_ID`

### URL Parameters

Parameter | Description
--------- | -----------
REFERRAL_ID | The ID of the referral
status | One of `"lead"`, `"placed"`, `"paid"`, or `"discarded"`.

### Response
If successful, the API will respond with the details of the referral lead.  See example to the right.
  
## Create a Referral

```ruby
require 'curb' # Curl access in ruby

http = Curl.post('https://example.staffingreferrals.com/api/v1/referrals',
                 { recruiter: {
                     email: 'recruiter@email.com',
                   },
                   ambassador: {
                     email: 'ambassador@email.com',
                     first_name: 'Anne',
                     last_name: 'Ambassador',
                     phone: '505-555-5555',
                   },
                   applicant: {
                     email: 'applicant@email.com',
                     first_name: 'Andrew',
                     last_name: 'Applicant',
                     phone: '505-555-1234',
                   }
                 }) do |http|
  http.headers['Authorization'] = "YOUR_TOKEN_HERE"
end

json = JSON.parse(http.body)
```

```shell
curl "https://example.staffingreferrals.com/api/v1/referrals/10" \
  -H "Authorization: YOUR_TOKEN_HERE" \
  -d "recruiter[email]=recruiter@email.com&ambassador[email]=ambassador@email.com&ambassador[first_name]=Anne&ambassador[last_name]=Ambassador&ambassador[phone]=505-555-5555&applicant[email]=applicant@email.com&applicant[first_name]=Andrew&applicant[last_name]=Applicant&applicant[phone]=505-555-1234" \
  -X POST
```

> The above command returns JSON structured like this:

```json
{
  "status":"success",
  "data":{
    "id":11,
    "status":"Lead",
    "created_at":"2018-02-02 21:05:06 UTC",
    "updated_at":"2018-02-02 21:05:06 UTC",
    "is_manual":false,
    "has_resume":false,
    "previous_referral_ids":"",
    "campaign":{
      "id":48,
      "status":"Live",
      "minimum_term":null,
      "minimum_term_units":null,
      "applicant_bonus":"$500",
      "ambassador_bonus":"$42",
      "job_id":null,
      "job_title":null,
      "job_category":null
    },
    "applicant":{
      "id":43,
      "status":"Active",
      "email":"applicant@email.com",
      "first_name":"Andrew",
      "last_name":"Applicant",
      "phone":"505-555-1234"
    },
    "ambassador":{
      "id":42,
      "status":"Active",
      "email":"ambassador@email.com",
      "first_name":"Anne",
      "last_name":"Ambassador",
      "phone":"505-555-5555"
    },
    "recruiter":{
      "id":35,
      "status":"Active",
      "email":"recruiter@email.com",
      "first_name":"Recruity",
      "last_name":"Recruiter",
      "phone":null
    }
  }
}
```


This endpoint creates a manual referral.  If the ambassador is not in the system (looked up via email), then they are also created and an invitation email is sent.

### HTTP Request

`POST https://example.staffingreferrals.com/api/v1/referrals`

### URL Parameters

Parameter | Required | Description
--------- | -------- | -----------
recruiter[email] | Required when the ambassador doesn't exist | The email of the recruiter.  If this is provided for an existing ambassador, it will be ignored.
ambassador[email] | Always Required | The email of the ambassador.  If they exist, no further ambassador information is required.
ambassador[first_name] | Required when the ambassador doesn't exist | The first name of the ambassador.
ambassador[last_name] | | The last name of the ambassador.
ambassador[phone] | | The phone of the ambassador.
applicant[email] | Always Required | The email of the applicant.
applicant[first_name] | Always Required | The first name of the applicant.
applicant[last_name] | Always Required | The last name of the applicant.
applicant[phone] | Always Required | The phone of the applicant.

### Response
If successful, the API will respond with the details of the referral lead.  See example to the right.


# Users

## Get All Users

```ruby
require 'curb' # Curl access in ruby

http = Curl.get('https://example.staffingreferrals.com/api/v1/users', {}) do |http|
  http.headers['Authorization'] = "YOUR_TOKEN_HERE"
end

json = JSON.parse(http.body)
```

```shell
curl "https://example.staffingreferrals.com/api/v1/users" \
  -H "Authorization: YOUR_TOKEN_HERE"
```

> The above command returns JSON structured like this:

```json
{
  "status":"success",
  "data":[
    {
      "id":149,
      "status":"Active",
      "email":"hester_ziemann@metz.name",
      "first_name":"Recruity",
      "last_name":"Recruiter",
      "phone":null,
      "created_at":"2018-03-07 23:34:02 UTC",
      "updated_at":"2001-02-02 21:05:06 UTC",
      "role_name":"recruiter"
    },
    {
      "id":150,
      "status":"Active",
      "email":"erick@rueckerstrosin.name",
      "first_name":"Pro",
      "last_name":"Motor",
      "phone":null,
      "created_at":"2018-03-07 23:34:02 UTC",
      "updated_at":"2018-03-07 23:34:04 UTC",
      "role_name":"ambassador"
    },
    {
      "id":151,
      "status":"Active",
      "email":"dandre@homenick.net",
      "first_name":"Candy",
      "last_name":"Date",
      "phone":null,
      "created_at":"2018-03-07 23:34:02 UTC",
      "updated_at":"2018-03-07 23:34:04 UTC",
      "role_name":"applicant"
    },
    {
      "id":148,
      "status":"Active",
      "email":"brittany@lehnerwest.io",
      "first_name":"Anne",
      "last_name":"Admin",
      "phone":null,
      "created_at":"2001-02-02 21:05:06 UTC",
      "updated_at":"2018-03-07 23:34:04 UTC",
      "role_name":"admin"
    }
  ]
}
```

This endpoint retrieves all users in the company, optionally filtered to only get recently updated or created ones.

### HTTP Request

`GET https://example.staffingreferrals.com/api/v1/users`

### Query Parameters

Parameter | Description
--------- | -----------
since | An **optional** ISO-8601 formatted date to provide a lower bound for updates that you are interested in.  For example, if you want all referral leads created or updated since noon on the Fourth of July in 2017 Denver Time, use `2017-07-04T12:00:00-07:00`.


<aside class="success">
Remember — you must provide the Authorization header for all requests.
</aside>
