# Overview
This repo offers documentation for querying a publicly accessible API to retrieve cost data by hospital provider and diagnosis. The purpose of making this publicly available is to attempt to help push down health care costs nationwide. If uninsured, patients can view costs of certain procedures to review options.

### Notes
- This project is a continual work in progress and may not contain all US hospitals.
- The costs returned through this API reflect the average cost by diagnosis (DRG) reported by each hospital in January 2019.
- Many, but not all, of the costs returned by the API exclude outliers, defined as +/- 2 standard deviations from the mean. In other words, if a hospital performed 10 heart transplants in 2019, and one of those transplants incurred costs greater than 2 standard deviations from the mean of the others, it is not included in the average.

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
