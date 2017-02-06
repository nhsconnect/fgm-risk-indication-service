---
title: FGM Query v2
keywords: development fgm query v2
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_query_v2.html
summary: "FGM Query/ Response v2 FHIR Messaging API."
---


## Prerequisites ##

To use this Messaging API, the requester:

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have authenticated the user using national smartcard authentication, and obtained a ticket and the UUID from the user's smartcard 
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.



## HTTP Request Headers ##

All FGM Create Flag connections to the FGM Service should include the below HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Host:`        | msg.int.spine2.ncrs.nhs.uk |
| `SOAPAction:`           | "urn:nhs:names:services:clinicals-sync/FGMQuery_2_0" |
| `Content-Length`             | e.g. 3305|
| `Content-Type: `  | `application/xml+fhir; charset=utf-8`|
| `Connection:`      | e.g. close |


## FGM Query ##

Query for an active FGM status flag  for a specified patient using a business identifier (i.e. the NHS Number).

```http
POST /fhir/fgm/query HTTP/1.1
```
- The `[valueString]` field for the Risk Indicator parameter SHALL be populated with the code: `FGM`.
- The `[valueString]` field for the NHS Number parameter SHALL be populated with the patienmt NHS Number identifier e.g.: `9999999999`.
<!-- - The `[base]` is the URL of the Spine endpoint. -->
<!-- - Note: The mime-type can be specified to request either XML or JSON using another URL parameter `?_format=[mime-type]`, or a `Content-Type` HTTP header as per the [FHIR specification](https://www.hl7.org/fhir/http.html#mime-type). -->

### FGM Query Request ###

```xml
TBA

```

<!-- - Additional examples are available here - [XML](Examples/Bundle-Observation.xml)  -->


### Query Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message, containing either:
  - One matching `Flag` resource that conforms to the `spine-ris-flag-1-0` profile; or
  - One `OperationOutcome` resource if the interaction is a success, however no FGM flag record has been found.
<!-- - Where an Observation is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `observation` resource. -->


```xml
TBA

```

Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the search cannot be executed.
- The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|500 | information | not-found | FGM-0001 | No FGM Record Found|
|500 | error | invalid | FGM-0002 | NHS Number invalid |
|500 | error | invalid | FGM-0004 | Invalid value for parameter - {0} |
|500 | error | forbidden | 300 | Access to service denied |


- The error codes are defined in the [spine-response-code-1-0 ValueSet](/ValueSets/spine-response-code-1-0.xml)
- Error FGM-0002 would occur if the NHS number being requested in the search request is invalid.
- Error 300 comes from the old SOAP error for Access Denied with is already used throughout Spine when the endpoint lookup fails.

```xml
TBA

```

<!-- - Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Message-Bundle-1-0)  -->

### Example Code ###

#### Java ####

```java
TBA
```
