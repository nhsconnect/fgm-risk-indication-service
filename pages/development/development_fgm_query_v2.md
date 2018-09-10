---
title: FGM Query
keywords: development fgm query
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_query.html
summary: "FGM Query/ Response FHIR Messaging API."
---


## Prerequisites ##

To use this FHIR Messaging API, the requester:

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have authenticated the user using national smartcard authentication, and obtained a ticket and the UUID from the user's smartcard 
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.



<!-- ## HTTP Request Headers ##

All FGM Create Flag connections to the FGM Service should include the below HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Host:`        | msg.int.spine2.ncrs.nhs.uk |
| `SOAPAction:`           | "urn:nhs:names:services:clinicals-sync/FGMQuery_2_0" |
| `Content-Length`             | e.g. 3305|
| `Content-Type: `  | `application/xml+fhir; charset=utf-8`|
| `Connection:`      | e.g. close | -->


## FGM Query ##

### FGM Query Request ###


For all Spine 2 FGM connections the FGM Query request payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the [spine-message-bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) profile;
- MessageHeader resource that conforms to the [spine-Request-messageheader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-request-messageheader-2-0) profile;
- Parameters resource that conforms to the [spine-ris-parameters-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-parameters-1-0) profile;
- Practitioner resource that conforms to the [spine-practitioner-2-0](https://fhir.nhs.uk/StructureDefinition/spine-practitioner-2-0) profile;

The FGM Query request payload MAY include the FGM FHIR profile:

- Organization resource that conforms to the [spine-organization-1-0](https://fhir.nhs.uk/StructureDefinition/spine-organization-1-0) profile;

<!-- ### Non Spine RBAC enabled NHS system ###

For all SMSP connections The FGM Query request payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the `spine-message-bundle-1-0` profile;
- MessageHeader resource that conforms to the `spine-Request-messageheader-1-0` profile;
- Parameters resource that conforms to the `spine-ris-parameters-1-0` profile;

The SMSP FGM Query request payload MAY include the FGM FHIR profile:

- Practitioner resource that conforms to the `spine-practitioner-1-0` profile;
- Organization resource that conforms to the `spine-organization-1-0` profile; -->

### FGM Query Parameters ###

The FGM Query parameters are conveyed in the [spine-ris-parameters-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-parameters-1-0) profile and MUST include these parameters:

 - risk Indicator code fixed to 'FGM' (FGM status flag) 
 - business identifier (Verified NHS number for specified patient)



<!-- ```http
POST /fhir/fgm/query HTTP/1.1
```
- The `[valueString]` field for the Risk Indicator parameter SHALL be populated with the code: `FGM`.
- The `[valueString]` field for the NHS Number parameter SHALL be populated with the patienmt NHS Number identifier e.g.: `9999999999`. -->

### FGM Query Request ###

<div style="border:solid 1px; border-color: #c3c8cc;">

<pre>
POST /fhir/fgm/query HTTP/1.1
Host: msg.int.spine2.ncrs.nhs.uk
SOAPAction: "urn:nhs:names:services:clinicals-sync/FGMQuery_2_0"
Content-Length: 3305
Content-Type: text/xml; charset=utf-8
Connection: close
</pre>

<script src="https://gist.github.com/sufyanpat/bf3a1b03d2ae99b446a3b12fc6e37ab5.js"></script>

</div>
- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html)


### Query Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of type message that conforms to the [spine-message-bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) profile.
- SHALL return a `MessageHeader` resource that conforms to the [spine-Response-messageheader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-response-messageheader-2-0) profile.
- SHALL return a `Flag` resource that conforms to the [spine-ris-flag-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-flag-1-0) profile. This includes the [spine-ris-patient-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-patient-1-0) profile as a contained resource.

Spine 2 will return:
 
<!-- One `OperationOutcome` resource if the interaction is a success, however no FGM flag record has been found.  -->
<!-- - Where an Observation is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `observation` resource. -->


<div style="border:solid 1px; border-color: #c3c8cc;">

<pre>
HTTP/1.1 200 OK
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
</pre>

<script src="https://gist.github.com/sufyanpat/a3deefdd707562ed27889172fb74e80c.js"></script>
</div>

OR the Query Response Message:

- SHALL return a `404` **Not Found** HTTP status code on successful execution of the interaction, however no FGM flag record has been found.
- SHALL return a `Bundle` of `type` message that conforms to the [spine-message-bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) profile;
- SHALL return a MessageHeader resource that conforms to the [spine-Response-messageheader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-response-messageheader-2-0) profile, containing:
  - The `OperationOutcome` resource which MUST conform to the [spine-operationoutcome-1-0](https://fhir.nhs.uk/StructureDefinition/spine-operationoutcome-1-0) profile; 


Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the [spine-operationoutcome-1](https://fhir.nhs.uk/STU3/StructureDefinition/Spine-OperationOutcome-1) profile if the search cannot be executed.
- Errors may also be generated by the SMS middleware e.g. where the middleware cannot parse a client request message or Spine services are not available. 
- The below table summarises the types of error that could occur, the HTTP response codes, along with the values to expect in the `OperationOutcome`response body.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Orignal codes |
|-----------|----------------|------------|--------------|-----------------|-----------------|
|404 | error | not-found | NO_RECORD_FOUND | No Record Found| FGM-0001 |
|400 | error | invalid | INVALID_NHS_NUMBER | Invalid NHS number| FGM-0002 |
|400 | error | invalid | PATIENT_OVER_18 | Patient is over 18 | FGM-0003 |
|400 | error | invalid | INVALID_PARAMETER | Invalid parameter | FGM-0004 |
|422 | warning | informational | FLAG_ALREADY_SET | Flag value was already set | FGM-0005 |
|400 | error | structure | MESSAGE_NOT_WELL_FORMED | Message not well formed | FGM-9999 |
|403 | error | forbidden | ASID_CHECK_FAILED | The sender or receiver's ASID is not authorised for this interaction | 300 |
|400 | error | structure | INPUT_MESSAGE_VALIDATION_ERROR | Input message validation error| SMSP-0001 |
|400 | error | structure | RESPONSE_MESSAGE_VALIDATION_ERROR |Response message validation error | SMSP-0002 |
|203 | warning | Information | DATA_RETURNED_FROM_LOCAL_STORE_SPINE_UNAVAILABLE | Data returned from local store, Spine unavailable | SMSP-0003 |
|500 | fatal | no-store | COULD_NOT_CONNECT_TO_SPINE | Could not connect to Spine | SMSP-0004 |
|401 | fatal| forbidden | AUTHOR_CREDENTIALS_ERROR | Author credentials error | SMSP-0005 |
|500 | fatal | Internal server error | GENERIC_SPINE_MINI_SERVICE_PROVIDER_SOFTWARE_FAILURE | Generic Spine Mini Service Provider software failure | SMSP-9999 |

<!-- |500 | error | invalid | FGM-0004 | Invalid value for parameter - {0} |
|500 | error | forbidden | 300 | Access to service denied | -->



- The error codes are defined in the [spine-response-code-2-0](https://fhir.nhs.uk/ValueSet/spine-response-code-2-0) ValueSet.
- Error INVALID_NHS_NUMBER would occur if the NHS number being requested in the search request is invalid.
- Error ASID_CHECK_FAILED comes from the old SOAP error for Access Denied with is already used throughout Spine when the endpoint lookup fails.

If the NHS number being requested in the search request is invalid the Spine 2 will return:

<div style="border:solid 1px; border-color: #c3c8cc;">

<pre>
HTTP/1.1 400 Bad Request
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
</pre>

<script src="https://gist.github.com/sufyanpat/a7bf61ab6994ad9ef3a8712274e1ade4.js"></script>

</div>

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html) 

### Example Code ###

#### Java ####

```java
FhirContext ctx = FhirContext.forDstu2();	
Bundle bndl = CreateFGMQueryv2.createBundle();
MessageHeader mh = CreateFGMQueryv2.createMessageHeader();
Parameters pm = CreateFGMQueryv2.createParameters();
Practitioner prac = CreateFGMQueryv2.createPractitioner();
Organization orgn = CreateFGMQueryv2.createOrganization();
final Entry entryBundle = bndl.addEntry();		
final Entry entryParam = bndl.addEntry();
final Entry entryPrac = bndl.addEntry();
final Entry entryOrg = bndl.addEntry();
entryBundle.setResource(mh);
entryParam.setResource(pm);
entryPrac.setResource(prac);
entryOrg.setResource(orgn);
String output = ctx.newXmlParser().setPrettyPrint(true).encodeResourceToString(bndl);
System.out.println(output);
String serverBaseUrl = "http://fhirtest.uhn.ca/baseDstu2";
IGenericClient client = ctx.newRestfulGenericClient(serverBaseUrl);
MethodOutcome outcome = client.create().resource(bndl).execute();
System.out.println(outcome.getId());
```
