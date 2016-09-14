---
title: SMG Assessment API Reference

language_tabs:
  - curl
  - ruby

toc_footers:
  - <a href='https://www.selfmgmt.com/contact-us/'>Contact us for API Access</a>
  - <a href='https://www.selfmgmt.com'>About Self Mangement Group</a>

includes:
  - languages
  - assessments
  - errors

search: true
---

# Introduction

Welcome to Self Management Group's Assessment API documentation. The SMG Assessment API allows you to gain access to the assessment tools offered by the Self Management Group.  Key functions are made available for consumers to integrate our assessment tools into your custom applications.

Our Assessment API is RESTful. Each request has an associated HTTP verb which must be used. Certain endpoints accept resources/posted data as part of the request.

All requests must be made via SSL (HTTPS), and non-SSL requests will be ignored.  Each request requires authorization through the use of bearer tokens.

The Assessment API base URL is at the following location: `https://www.selfmgmt.com/api/v1`

# Authorization

In order to authorize your API queries, you will need to create a token using your set of public and private keys (currently supplied to you by Self Management Group).

<aside class="notice">
All endpoints except “echo” must be authorized.
</aside>

# Generating a Token

> To authorize, use this code:

```shell
curl  --user "your_public_key:your_private_key" \
  -d "grant_type=client_credentials" \
  https://www.selfmgmt.com/api/v1/oauth/token
```

> If successful, the following typical JSON object will be returned:

```json
{
  "access_token":"f61cLMAODCPrxTZ0uOqwqPNAwNAe8OgIxQzlGRUQzU1OTY4Mjc5RkQxMEI5QkMyNEJERjc3QTczNw",
  "token_type":"Bearer",
  "expires_in":3600
}
```

> Error messages are returned in a JSON array with a code, reason and detail. A typical message is:

```json
{
  "errors":[
    {
     "code":"401",
     "reason":"unauthorized",
     "detail":"invalid_credentials"
     }
    ]
}
```

To create a token you must send an authorization header (basic authorization) using your public key and private keys within a POST request.  The endpoint to reach is:

`https://www.selfmgmt.com/api/v1/oauth/token`

<aside class="notice">
Make sure to replace `your_public_key` and `your_private_key` with your respective keys.
</aside>

To use this token in any endpoint, add this authorization header to your requests:

`Authorization:Bearer <token>`

# Endpoints

## /echo

```shell
curl https://www.selfmgmt.com/api/v1/echo/Hello%20World!
```
> The above command returns JSON structured like this:

```json
{
  "result":"Hello World!"
}
```

Used to test that you are connected

e.g. `https://www.selfmgmt.com/api/v1/echo/Hello%20World!`

This endpoint does not require authorization and only accepts the `GET` method.

## /oauth/token

```shell
curl -H "Authorization:Bearer <token>" https://www.selfmgmt.com/api/v1/oauth/token
```
> The above command returns JSON structured like this:

```json
{
  "access_token":"f61cLMAODCPrxTZ0uOqwqPNAwNAe8OgIxQzlGRUQzU1OTY4Mjc5RkQxMEI5QkMyNEJERjc3QTczNw",
  "token_type":"Bearer",
  "expires_in":3554
}
```

Used to create tokens `POST` as well as retrieve information about tokens `GET`.  See the section “Generating a Token” on how to create a token.  Below is an example using curl to view a token (using the `GET` method).

## /assess

```shell
curl -H "Authorization:Bearer <token>" https://www.selfmgmt.com/api/v1/assess/pops/eng
```
> The above command returns JSON structured like this:

```json
{
  "assessment": {
    "link": https://www.selfmgmt.com/pac/assess/FQQcc4sU5hll7XQO0PlQ72ZIwwcpVX8kzfb”
    "id": "cc4sU5hll7XQO0PlQ72ZIwwcpVX8kzfb"
  }
}
```

Returns a link to complete an assessment. Both the assessment acronym and language are specified in the endpoint. See the Appendix for assessment acronymns and language codes.

<aside class="notice">
As with all endpoints put the actual token value in place of <code>&lt;token&gt;</code>
</aside>

Note: this will return a randomly generated 36 alpha numeric string that we provide for you.  This is will be required to retrieve results (see the the results endpoint).

e.g. `"assessment_id":"D2lFYf45VVbXqdWARl4l4DgpizTumY28"`

### Pre-populating Form Data

We can pre-populate information on the assessments to avoid having the candidate re-enter information you have on file.

Fields | Description
------ | -----------
firstname | The first name of the candidate
lastname | The last name of the candidate
address | The street address of the candidate
city | The city of the candidate
zipcode | The zip/postal code of the candidate
country | The country of the candidate
telephone | The telephone number of the candidate
email | The email address of the candidate

Simply append the name=value pair(s) onto the querystring of the assessment link you receive. For example:

`https://www.selfmgmt.com/pac/assess/?t=pops&l=eng&c=f61cPNA&firstname=jane&lastname=doe`

## /results

```shell
curl -H "Authorization:Bearer <token>"  https://www.selfmgmt.com/api/results/pops/eng/idValue
```
> The above command returns JSON structured like this:

```json
{
  "records": [
    {
      "id": "B0bAoHpUM4UHvbVmPdbrlYzToMSCe5pq",
      "date": "2016/08/16",
      "personal": {
        "firstname": "Sample",
        "lastname": "Candidate",
        "gender": "",
        "address": "123 Main St.",
        "city": "Toronto",
        "state": "Ontario",
        "zipcode": "N2T 2J8",
        "country": "Canada",
        "telephone": "416-555-1212",
        "email": "sample@yahoo.com"
      },
      "scores": {
        "talent": "3.0",
        "effort": "4.0",
        "opport": "5.0",
        "total": "4.0",
        "recommendation": "Proceed",
        "stoplight": "Green"
      },
      "reports": {
        "manager": "https://selfmgmt.com/api/v1/reports/pops/m/B0bAoHpUM4UHvbVmPdbrlYzToMSCe5pq",
        "candidate": "https://selfmgmt.com/ api/v1/reports/pops/c/B0bAoHpUM4UHvbVmPdbrlYzToMSCe5pq"
      }
    }
  ]
}
```

Retrieves results from an assessment taken.  You must pass the test type (e.g, POPS), the language (e.g., en), followed by the ID we generate for you passed to us in the `/assess` endpoint.

If you have a callback url, the test taker will be redirected to the page once they click on a Continue Button after the results are saved.  All form data will be posted as well.

