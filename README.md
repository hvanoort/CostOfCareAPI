# Overview
This repo offers documentation for querying a publicly accessible API to retrieve cost data by hospital provider and diagnosis. The purpose of making this publicly available is to attempt to help push down health care costs nationwide. If uninsured, patients can view costs of certain procedures to review options.

### Notes
- This project is a continual work in progress and may not contain all US hospitals.
- The costs returned through this API reflect the average cost by diagnosis (DRG) reported by each hospital in January 2019.
- Many, but not all, of the costs returned by the API exclude outliers, defined as +/- 2 standard deviations from the mean. In other words, if a hospital performed 10 heart transplants in 2019, and one of those transplants incurred costs greater than 2 standard deviations from the mean of the others, it is not included in the average.

# Base URL
```
https://of7bidbqle.execute-api.us-west-2.amazonaws.com
```
The only version currently available is the *beta* version of the API.

# Endpoints
## Costs
The _costs_ endpoint allows you to query hospital prices across all cost types (DRG, HCPCS, SVC, etc.). The parameters passed to this endpoint are chained with "AND" functionality, meaning each additional parameter provided will further filter the results. At least one of the following are required:

* **provider_name** - (optional) The name of the provider. The request will return matches containing the provided string.
* **provider_id** - (optional) The ID of the provider, as can be found in [this](https://data.medicare.gov/widgets/xubh-q36u) database
* **service_name** - (optional) The name of the service being requested (i.e. "heart transplant", "cpr", etc.). The request will return all matches containing the provided string. In other words, if the value passed to this parameter is "biopsy", the API will return results of procedures that contain the word biposy, like "Needle Kidney Biopsy" or "Bone Marrow Needle Biopsy".
* **code_type** - (optional) One of: ["drg", "hcpcs", "ndc", "svc"]
* **code** - (optional) The Medicare/Medicaid approved code relating to the DRG, HCPCS, NDC, or SVC. NOTE: If this parameter is provided, you must also pass a value for the "cost_type" parameter.

#### cURL
_Example Request_
```
curl --request GET https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs?provider_id=230038
```
_Example Response_
```python
[
  {
    "provider_name": "SPECTRUM HEALTH - BUTTERWORTH CAMPUS",
    "provider_id": 230038,
    "service_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC",
    "code_type": "drg",
    "code": 1, 
    "cost": 868178
  },
  {...}
]
```

#### Python
_Example Request_
```python
import requests

params = {
    'provider_id': 230038,
    'code': 1
}

resp = requests.get('https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs', params=params)
```

_Example Response_
```python
[
  {
    "drg_code": 1, 
    "provider_id": 230038,
    "cost": 868178, 
    "provider_name": "SPECTRUM HEALTH - BUTTERWORTH CAMPUS",
    "drg_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC",
  }
]
```

## Provider
The _provider_ endpoint allows you to query by hospital provider name and/or provider ID and returns data about that provider, along with all available pricing information spanning DRG, HCPCS, NDC, and SVC prices.

### cURL
_Example Request_
```
curl --request GET https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/provider?provider_id=230038
```

_Example Response_
```python
{
  "provider_id": 230038,
  "provider_name": "SPECTRUM HEALTH - BUTTERWORTH CAMPUS",
  "service_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC",
  "drg_prices": [
    {
      "drg_code": 1, 
      "price": 868178, 
      "drg_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC"
    },
    {...}
  ],
  "hcpcs_prices": [
    {...},
    {...}
  ]
}
```
