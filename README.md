**Aminov API**
# BIM IFC Services
![Topology1](Images/building-1.png)

**BCFv3.0** is based on BCFv2.1.
**Swagger / OpenAPI Specification**  
On SwaggerHub Coming soon  

# Contributing
Please don't hesitate to create a ticket if you would like advice or suggestions on how to improve or create new services.

**Table of Contents**

<!-- toc -->

- [1. Introduction](#1-introduction)
- [2. Services](#2-services)
  * [2.1 IFC files Registration](#21-ifc-files-registration)
  * [2.2 Check ticket exists](#22-check-ticket-exists)
  * [2.3 Check IFC Syntax](#23-check-ifc-syntax)
  * [2.4 Get Syntax Check Result](#24-get-syntax-check-result)
  * [2.5 Run ILOD Check](#25-run-ilod-check)
  * [2.6 Get ILOD Check Result](#26-get-ilod-check-result)


  

<!-- tocstop -->

# 1. Introduction

We are working in the BIM & PLM industry and started this initiative to assist ourselves in our daily work and also benefit others.

Here, you can find the definitions of various services, which we will continuously enrich.

----------

# 2. Services

## 2.1 IFC files Registration

End point to upload files on the cloud.\nThe files will be kept for a duration of 4 hours then deleted.\nIt will return a ticket that can be used for other services.\nThe ticket is also valid for 4 hours

**Resource URL**

    GET https://{{RAPID-API-HOST}}/register

[openapi schemas](Specs/openapi-Rapid-Api.json) [ path/register]


**Example Request**

    POST /register

    Content-Type: multipart/form-data; boundary=boundary
    X-RapidAPI-Key: {{X-RapidAPI-Key}}
    X-RapidAPI-Host: {{X-RapidAPI-Host}}
    --boundary
    Content-Disposition: form-data; name="files"; filename="file01.ifc"
    Content-Type: text/plain
    < /tmp/file01.ifc
    --boundary
    Content-Disposition: form-data; name="files"; filename="file02.ifc"
    Content-Type: text/plain
    < /tmp/file02.ifc
    --boundary-

**Example Response**

    Response Code: 200 - OK
    Body:
    {
        "ticket": "a8a85e722e714bae9ff4cd839a0d87c3",
        "bytes": 15171642,
        "life": "4:00:00",
        "files": 2,
        "msg": "Use the ticket to call other services on the files"
    }

## 2.2 Check ticket exists

**Resource URL**

    GET https://{{RAPID-API-HOST}}/exists/{{TICKET}}

[openapi schemas](Specs/openapi-Rapid-Api.json) [ path/exists]


**Example Request**

    GET /exists/a8a85e722e714bae9ff4cd839a0d87c3

**Example Response**

    Response Code: 200 - OK
    Body:
    {
        "exists": true
    }

## 2.3 Check IFC Syntax

**Resource URL**

    POST https://{{RAPID-API-HOST}}/syntax/{{RAPID-API-TICKET}}

[openapi schemas](Specs/openapi-Rapid-Api.json) [ path/syntax]


**Example Request**

    POST /syntax/a8a85e722e714bae9ff4cd839a0d87c3

**Example Response**

    Response Code: 200 - OK
    Body:
    {
    "ticket": "a8a85e722e714bae9ff4cd839a0d87c3",
    "msg": "Syntax check exeuted, follow the returned url to get the result",
    "url": "/ifc/result/syntax/a8a85e722e714bae9ff4cd839a0d87c3",
    "method": "GET"
    }

## 2.4 Get Syntax Check Result

**Resource URL**

    GET https://{{RAPID-API-HOST}}/syntax/{{RAPID-API-TICKET}}

[openapi schemas](Specs/openapi-Rapid-Api.json) [ path/syntax]


**Example Request**

    GET /syntax/a8a85e722e714bae9ff4cd839a0d87c3

**Example Response**

    Response Code: 200 - OK
    Body:
    {
    "result": [],
    "expiry": "3:59:57"
    }

## 2.5 Run ILOD Check

**Resource URL**

    POST https://{{RAPID-API-HOST}}/ilod/{{RAPID-API-TICKET}}

[openapi schemas](Specs/openapi-Rapid-Api.json) [ path/ilod]


**Example Request**

    POST /ilod/a8a85e722e714bae9ff4cd839a0d87c3
    Body:
    [
    {
        "id":"0000",--> Rule ID
        "ifctypes":["IfcSlab"], --> Aplicable IFC types
        "propertySet": "Nset_Wildcard", --> Property set
        "property": "location",--> Property 
        "operator": "NOTEMPTY", --> should not be empty
        "value": "N/A" --> Not applicable
    },
        {
        "id":"0001",
        "ifctypes":["IfcSlab"],
        "propertySet": "Nset_Wildcard",
        "property": "Width",
        "operator": "EQ", --> Equal
        "value": "10"
    },
        {
        "id":"0003",
        "ifctypes":["IfcSlab"],
        "propertySet": "Nset_Wildcard",
        "property": "Weight",
        "operator": "GT", --? Greater than
        "value": "301"
    }
    ]

**Example Response**

    Response Code: 200 - OK
    Body:
    {
        "ticket": "a8a85e722e714bae9ff4cd839a0d87c3",
        "msg": "ILOD check exeuted, follow the returned url to get the result",
        "url": "/ilod/a8a85e722e714bae9ff4cd839a0d87c3",
        "method": "GET"
    }

## 2.6 Get ILOD Check Result

**Resource URL**

    GET https://{{RAPID-API-HOST}}/ilod/{{RAPID-API-TICKET}}

[openapi schemas](Specs/openapi-Rapid-Api.json) [ path/ilod]

**Example Request**

    GET /ilod/a8a85e722e714bae9ff4cd839a0d87c3

**Example Response**

    Response Code: 200 - OK
    Body:
    {
    "result": {
        "ifcbridge-model002.ifc": [
        {
            "id": "3LOGhkzE9ENe7ktESJ8TbW", --> GUID
            "fail": [
            "0000", --> Rule ID
            "0001",
            "0002"
            ],
            "success": []
        },
        {
            "id": "1ix42aJbzCxvtH4Osb2WfT",
            "fail": [
            "0000",
            "0001",
            "0002"
            ],
            "success": []
        }
        ]
    }
    }
