# OS Hub API Change Notes for November 2022 

## Overview

The OS Hub’s API offers the ability for contributors and users to upload, download and query data from the OS Hub. The key differentiator for the API, compared to the manual upload process, is the ability to efficiently interact with the database on a per facility basis. The OS Hub API is built on top of the OAR API, meaning that all functionality that exists in the OAR API will continue to be supported by the OS Hub API. One of the key differences between the two APIs is variable name changes for facility IDs and the contributor ID. Other changes will focus on expanding functionality of the API. 

### API Changes for November: 
The sections below are a comprehensive list of the changes being made for the November launch of OS Hub.

#### Endpoint Addresses: 
All endpoints can be reached at https://opensupplyhub.org/api/[endpoint]. This will continue to be supported, but will forward all traffic to opensupplyhub.org/api. For best performance, API calls should be updated to use the new URL. 

The existing API endpoints will still be available at https://openapparel.org/api/[endpoint] but will return http resonse code 302. Client code may need adaption to call the address of the new endpoint (https://opensupplyhub.org/api/[endpoint]), which will also be part of the message reply header. We strongly advise to update the scripts to use the new URLs under https://opensupplyhub.org/api/[endpoint].

If you use python, both `urllib` and `requests` should require no change unless you specified a redirect handler (`urllib`), or set `allow_redirect` explicitly to `False`.

#### Support for sectors
When generating facilities via POST to https://opensupplyhub.org/api/facilities, we now accept JSON which contains a "sector_product_type" field. Purpose of this field is to specify sector, and product types, as a single string, or `|` delimited list of fields. 

Examples:
```
{
    "sector_product_type": "Electronics|Media Players",
    "country": "China",
    "name": "Example Corp",
    "address": "One Industry Park, Shanghai."
}
```
With Electronics being on the list of know sectors, this would set the sector to "Electronic", and product type to "Media Players".
```
{
    "sector_product_type": "Media Players",
    "country": "China",
    "name": "Example Corp",
    "address": "One Industry Park, Shanghai."
}
```
In case the "sector" field is not provided, we will set the sector to "Unspecified".

You can also specify the values as a JSON list/array, thus avoiding the `|` delimiter
```
{
    "sector_product_type": ["Electronics","Media Players","Electric Toothbrushes"],
    "country": "China",
    "name": "Example Corp",
    "address": "One Industry Park, Shanghai."
}
```

#### Change of response payload fields from oar_id to osh_id

API calls which return `oar_id` will now return `os_id` fields. This will specifically affect the GET call of https://opensupplyhub.org/api/facilities .

### New Endpoints

#### sectors

Get a list of sectors as stored in the database.

### Notifications

This functionality will not be available during a later release.


#### Lean Response: 

We have added an optional detail query parameter to the GET /api/facilities endpoint. If `detail=false`, the response is restricted to only an array of OS ID, match status, and confidence score, together with geojson information. Using `detail=false` also requires `create=false`. 

```
{
"id": "CN202031463D4MB",
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [
113.9573319,
23.1259237
]
},
"properties": {
"name": "1010 Printing International Ltd",
"address": "Xia Nan Industry, Hui Xing Road, Yuanzhou Town",
"country_code": "CN",
"oar_id": "CN202031463D4MB",
"country_name": "China",
"has_approved_claim": false,
"ppe_product_types": null,
"ppe_contact_phone": null,
"ppe_contact_email": null,
"ppe_website": null,
"is_closed": null
}
},
...
```


