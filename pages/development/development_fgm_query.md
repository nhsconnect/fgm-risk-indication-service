---
title: FGM Query
keywords: development fgm query
tags: [engagement,development]
sidebar: overview_sidebar
permalink: development_fgm_query.html
summary: "Developer Cheat Sheet shortcuts for the <br/>technical build of FGM Service API."
---
<!-- ---
title: Visitors and Migrants Chargeable-Status Indicator
keywords: nhsnumber, chargeable-status
tags: [foundations,use_case]
sidebar: foundations_sidebar
permalink: foundations_use_case_chargeable_status_indicator.html
summary: "Use case to search for an overseas Visitors and Migrants chargeable status indicator."
--- -->

## Prerequisites ##

To use this API, the requester:

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have authenticated the user using national smartcard authentication, and obtained a ticket and the UUID from the user's smartcard (this is pass in a JSON web token - see [Cross Organisation Audit and Provenance](integration_cross_organisation_audit_and_provenance.html) for details)
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.

## Request Headers ##

All Visitors and Migrants APIs should include the below additional HTTP request headers to support audit and security requirements on the Spine:

| Header               | Value |
|----------------------|-------|
| `Ssp-TraceID`        | Client System TraceID (i.e. GUID/UUID) |
| `Ssp-From`           | Client System ASID |
| `Ssp-To`             | The Spine ASID |
| `Ssp-InteractionID`  | `urn:nhs:names:services:visitorsandmigrants:fhir:rest:search:observation`|
| `Authorization`      | This will carry the base64 encoded JSON web token required for audit - see [Cross Organisation Audit and Provenance](integration_cross_organisation_audit_and_provenance.html) for details. |


## Search for Chargeable-Status Indicator ##

Search for a Visitors and Migrants Chargeable-Status Indicator `Observation` for a specified patient using a business identifier (i.e. the NHS Number).

```http
GET [base]/Observation?subject:Patient.identifier=[system]|[value]&code=[system]|[code]
```

- The `[system]` field for the Paient Itentifier SHALL be populated with the identifier system URL: `http://fhir.nhs.net/Id/nhs-number`.
- The `[system]` field for the Code SHALL be populated with the identifier system URL: `http://fhir.nhs.net/fhir-observation-code-1`.
- The `[base]` is the URL of the Spine endpoint.
- Note: The mime-type can be specified to request either XML or JSON using another URL parameter `?_format=[mime-type]`, or a `Content-Type` HTTP header as per the [FHIR specification](https://www.hl7.org/fhir/http.html#mime-type).

### Search Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` searchset, containing either:
	- One matching `Observation` resource that conforms to the `spine-vm-observation-1` profile; or
	- One `OperationOutcome` resource if the interaction is a success, however no Overseas Visitors and Migrants record has been found.
- Where an Observation is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `observation` resource.


```json
{
"resourceType": "Bundle",
  "id": "6f759a10-d1b6-11e6-9598-0800200c9a66",
  "type": "searchset",
  "entry": [
    {
      "resource": {
        "resourceType": "Observation",
        "id": "76e39290-d1aa-11e6-9598-0800200c9a66",
        "meta": {
          "profile": [
            "http://fhir.nhs.net/StructureDefinition/spine-vm-observation-1"
          ]
        },
        "status": "final",
        "code": {
          "coding": [
            {
              "system": "http://fhir.nhs.net/fhir-observation-code-1",
              "code": "0001",
              "display": "Visitors and Migrants status observation"
            }]
        },
        "subject": {
          "reference": "Patient/9900002831",
          "display": "Miss Mary Taylor"
        },
        "effectiveDateTime": "2015-01-01T15:00:00+00:00",
        "component": [
          {
            "code": {
              "coding": [
                {
                  "system": "http://fhir.nhs.net/spine-vm-observation-component-1",
                  "code": "bcs",
                  "display": "Basic Chargeable Status"
                }]
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://fhir.nhs.net/spine-chargeable-status-1",
                  "code": "y",
                  "display": "Chargeable"
                }]
            }
          },
          {
            "code": {
              "coding": [
                {
                  "system": "http://fhir.nhs.net/spine-vm-observation-component-1",
                  "code": "ccs",
                  "display": "Category Chargeable Status"
                }
              ]
            },
            "valueCodeableConcept": {
              "coding": [
                {
                  "system": "http://fhir.nhs.net/spine-category-status-1",
                  "code": "F",
                  "display": "F"
                }]
            }
          }
        ]
      }
    }
  ]
}
```

- Additional examples are available here - [XML](Examples/Bundle-Observation.xml) and [JSON](Examples/Bundle-Observation.json)

Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the search cannot be executed (not that there is no match).
- The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `ObservationOutcome` in the response body.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|404 | error | not-found | 0001 | No Record Found |
|400 | error | invalid | 0002 | Invalid NHS Number |
|400 | error | code-invalid | 0003 | Invalid identifier system |
|400 | error | code-invalid | 0004 | Invalid code system |
|400 | error | code-invalid | 0005 | Invalid code value |
|400 | error | value | 0006 | Invalid element |
|401 | fatal | forbidden | 0007 | Author credentials error |
|400 | error | invalid | 0008 | Invalid value for parameter |
|400 | error | invalid | 0009 | Request does not match audit record |
|400 | error |  structure | 9999 | Message not well formed |

- The error codes are defined in the [Spine Error or Warning Code ValueSet](/ValueSets/spine-error-or-warning-code-1.xml)
- Error 0009 would occur if the NHS number being requested in the search request does not match the requested_record value in the JWT - see [Cross Organisation Audit and Provenance](integration_cross_organisation_audit_and_provenance.html) for details.

```json
{
"resourceType": "Bundle",
  "id": "67883730-d1a8-11e6-9598-0800200c9a66",
  "type": "searchset",
  "entry": [
    {
      "resource": {
        "resourceType": "OperationOutcome",
        "id": "ff00d600-d1a6-11e6-9598-0800200c9a66",
        "meta": {
          "profile": [
            "http://fhir.nhs.net/StructureDefinition/spine-operationoutcome-1"
          ]
        },
        "issue": [
          {
            "severity": "error",
            "code": "invalid",
            "details": {
              "coding": [
                {
                  "system": "http://fhir.nhs.net/spine-error-or-warning-code-1",
                  "code": "0002"
                }]
            },
            "diagnostics": "Invalid NHS Number"
          }]
      }
    }
  ]
}
```

- Additional examples are available here - [XML](Examples/Bundle-OperationOutcome.xml) and [JSON](Examples/Bundle-OperationOutcome.json)

### Example Code ###

#### C# ####

```csharp
var client = new FhirClient("http://spine-base-url/fhir-base/");
client.PreferredFormat = ResourceFormat.Json;
var query = new string[] { "identifier=http://fhir.nhs.net/Id/nhs-number|9900002831" };
var bundle = client.Search("Patient", query);
FhirSerializer.SerializeResourceToXml(bundle).Dump();
```

#### Java ####

```java
FhirContext ctx = new FhirContext();
IGenericClient client = ctx.newRestfulGenericClient("http://spine-base-url/fhir-base/");
Bundle bundle = client.search().forResource(Observation.class)
.where(new TokenClientParam("identifier").exactly().systemAndCode("http://fhir.nhs.net/Id/nhs-number", "9900002831"))
.encodedXml()
.execute();
```

#### cURL ####

{% include embedcurl.html title="Search for Chargeable-Status Indicator" command="curl -X GET -H 'Ssp-From: 0001' -H 'Ssp-To: 0002' -H 'Ssp-InteractionID: urn:nhs:names:services:visitorsandmigrants:fhir:rest:search:observation' -H 'Cache-Control: no-cache' -H 'Ssp-TraceID: e623b4de-f6bb-be0c-956d-c4ded0d58fc0' 'http://spine-base-url/fhir-base/Observation?identifier=http://fhir.nhs.net/Id/nhs-number%7C9900002831'" %}
