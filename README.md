# Overview
This repo offers documentation for querying a publicly accessible API to retrieve cost data by hospital provider and diagnosis. The purpose of making this publicly available is to attempt to help push down health care costs nationwide. If uninsured, patients can view costs of certain procedures to review options.

# How to use it

### Base URL
```
https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs
```
Two parameters can be prodivided when querying the API - provider_id and/or drg_code.
* **provider_id** - The ID of the provider, as can be found in [this](https://data.medicare.gov/widgets/xubh-q36u) database
* **drg_code** - The DRG code of the diagnosis, which can be found [here](https://www.icd10data.com/ICD10CM/DRG)

<br>

### cURL
##### Example Request
```
curl --request GET https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs?provider_id=230038
```
##### Example Response
```python
[
  {
    "drg_code": 1, 
    "provider_id": 230038,
    "cost": 868178, 
    "provider_name": "SPECTRUM HEALTH - BUTTERWORTH CAMPUS",
    "drg_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC",
  },
  {...}
]
```

### Python
##### Example Request
```python
import requests

params = {
    'provider_id': 230038,
    'drg_code': 1
}

resp = requests.get('https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs', params=params)
```

##### Example Response
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
