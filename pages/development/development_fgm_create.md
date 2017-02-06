---
title: FGM Create Flag
keywords: development fgm create
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_create.html
summary: "FGM Create Flag/ Response FHIR Messaging API."
---

## Prerequisites ##

To use this FHIR Messaging API, the requester:

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have authenticated the user using national smartcard authentication, and obtained a ticket and the UUID from the user's smartcard 
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.


## HTTP Request Headers ##

All Create connections to the FGM Service should include the below HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Host:`        | msg.int.spine2.ncrs.nhs.uk |
| `SOAPAction:`           | "urn:nhs:names:services:clinicals-sync/FGMCreate_1_0" |
| `Content-Length`             | e.g. 3305|
| `Content-Type: `  | `application/xml+fhir; charset=utf-8`|
| `Connection:`      | e.g. close |


## FGM Create ##

TBA.

```http
POST /fhir/fgm/create HTTP/1.1
```
TBA.

### FGM Create Request ###

```xml
TBA

```

<!-- - Additional examples are available here - [XML](Examples/Bundle-Observation.xml)  -->


### Create Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message, containing either:
  - One MessageHeader resource that conforms to the ...... profile; 
  


```xml
tba

```

Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the create flag cannot be executed.
- The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display |
|-----------|----------------|------------|--------------|-----------------|
|||||

<!-- 404 | error | not-found | 0001 | No Record Found |
|400 | error | invalid | 0002 | Invalid NHS Number |
|400 | error | code-invalid | 0003 | Invalid identifier system |
|400 | error | code-invalid | 0004 | Invalid code system |
|400 | error | code-invalid | 0005 | Invalid code value |
|400 | error | value | 0006 | Invalid element |
|401 | fatal | forbidden | 0007 | Author credentials error |
|400 | error | invalid | 0008 | Invalid value for parameter |
|400 | error | invalid | 0009 | Request does not match audit record |
|400 | error |  structure | 9999 | Message not well formed | -->

- The error codes are defined in the [spine-response-code-1-0 ValueSet](/ValueSets/spine-response-code-1-0.xml)

```xml
TBA

```

<!-- - Additional examples are available here - [XML](Examples/Bundle-OperationOutcome.xml)  -->

### Example Code ###


#### Java ####

```java
TBA
```

