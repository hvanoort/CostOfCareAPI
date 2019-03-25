# Overview
This is a publicly available API containing pricing data for common procedures at domestic (US) hospitals. If uninsured, patients can view costs of various procedures at nearby hospitals to review options.

### Notes
- This project is a continual work in progress and may not contain all US hospitals or procedure data.
- The costs returned through this API reflect the average or median cost by procedure
- For DRG (Diagnosis Related Groups) costs, if fewer than 11 discharges were recorded by a provider for a DRG, that DRG is excluded from the dataset.
- Many, but not all, of the costs returned by the API exclude outliers, defined as +/- 2 standard deviations from the mean. In other words, if a hospital performed 10 heart transplants in 2019, and one of those transplants incurred costs greater than 2 standard deviations from the mean of the others, it is not included in the average.

<br><br>

# Base URL
```
https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/
```
The only version currently available is the *beta* version of the API. Access is regulated via API keys. See the following section for more information.

<br><br>

# Access
To access this API, you'll need to provide a valid API key in the *X-Api-Key* header of each request. To obtain an API key, just email harrison.vanoort@gmail.com and explain your use for the API. We'll provide you with an API key that will be tied to a usage plan that is appropriate for your use case.

<br><br>

# Endpoints
## Costs
The _costs_ endpoint allows you to query hospital prices across all cost types (DRG, HCPCS, SVC, etc.). The parameters passed to this endpoint are chained with "AND" functionality, meaning each additional parameter provided will further filter the results. The maximum number of results returned by this endpoint is 10. At least one of the following parameters are required:

parameter | required | description
--- | --- | ---
provider_name | false | The name of the provider. The request will return matches containing the provided string.
provider_id | false | The ID of the provider, as can be found in [this](https://data.medicare.gov/widgets/xubh-q36u) database.
service_name | false | The name of the service being requested (i.e. "heart transplant", "cpr", etc.). The request will return all matches containing the provided string. In other words, if the value passed to this parameter is "biopsy", the API will return results of procedures that contain the word biposy, like "Needle Kidney Biopsy" or "Bone Marrow Needle Biopsy".
code_type | false | One of: ["drg", "hcpcs", "ndc", "svc"]
code | false | The Medicare/Medicaid approved code relating to the DRG, HCPCS, NDC, or SVC. NOTE: If this parameter is provided, you must also pass a value for the "cost_type" parameter.


#### cURL
_Example Request_
```
curl --request GET https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs?provider_id=230038 --header "X-API-Key: {your-api-key}"
```
_Example Response_
```python
{
  'count': 1,
  'results': [
      {
        "provider_name": "SPECTRUM HEALTH - BUTTERWORTH CAMPUS",
        "provider_id": 230038,
        "service_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC",
        "code_type": "drg",
        "code": 1,
        "cost": 868178
      }
  ]
}
```

#### Python
_Example Request_
```python
import requests

params = {'provider_id': 230038}
header = {'X-API-Key': {your-api-key}}

resp = requests.get('https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/costs', params=params, headers=header)
```

_Example Response_
```python
{
  'count': 1,
  'results': [
      {
        "provider_name": "SPECTRUM HEALTH - BUTTERWORTH CAMPUS",
        "provider_id": 230038,
        "service_name": "HEART TRANSPLANT OR IMPLANT OF HEART ASSIST SYSTEM W MCC",
        "code_type": "drg",
        "code": 1,
        "cost": 868178
      }
  ]
}
```

<br>

## Providers
The _providers_ endpoint allows you to query by hospital provider name and/or provider ID and returns data about that provider. Only one result will be returned. Both of the following parameters are optional, but at least one must be provided.

parameter | required | description
--- | --- | ---
provider_id | false | The ID of the provider, as can be found in [this](https://data.medicare.gov/widgets/xubh-q36u) database.
provider_name | false | The name of this provider/hospital. The first result we can find that _begins with_ the value provided in this parameter will be returned. Use "+" in place of spaces (i.e. "Baptist+Hospital+of+Miami")

#### cURL
_Example Request_
```
curl --request GET https://of7bidbqle.execute-api.us-west-2.amazonaws.com/beta/providers?provider_id=100008 --header "X-API-Key: {your-api-key}"
```

_Example Response_
```python
{
  "county_name": "MIAMI-DADE",
  "location": "8900 N KENDALL DR MIAMI, FL (25.687781, -80.339086)",
  "hospital_name": "BAPTIST HOSPITAL OF MIAMI",
  "zip_code": "33176",
  "effectiveness_of_care_national_comparison": "Same as the national average",
  "meets_criteria_for_meaningful_use_of_ehrs": "TRUE",
  "efficient_use_of_medical_imaging_national_comparison": "Above the national average",
  "provider_id": "100008",
  "address": "8900 N KENDALL DR",
  "readmission_national_comparison": "Above the national average",
  "state": "FL",
  "city": "MIAMI",
  "hospital_type": "Acute Care Hospitals",
  "hospital_ownership": "Voluntary non-profit - Private",
  "mortality_national_comparison": "Above the national average",
  "safety_of_care_national_comparison": "Below the national average",
  "phone_number": "7865961960",
  "patient_experience_national_comparison": "Same as the national average",
  "timeliness_of_care_national_comparison": "Below the national average",
  "emergency_services": "TRUE",
  "hospital_overall_rating": "2"
}
```
