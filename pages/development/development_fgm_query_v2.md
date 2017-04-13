---
title: FGM Query v2
keywords: development fgm query v2
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_query_v2.html
summary: "FGM Query/ Response v2 FHIR Messaging API."
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

### FGM Query Request v2 ###


For all Spine 2 FGM connections the FGM Query request payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the `spine-message-bundle-1-0` profile;
- MessageHeader resource that conforms to the `spine-Request-messageheader-2-0` profile;
- Parameters resource that conforms to the `spine-ris-parameters-1-0` profile;
- Practitioner resource that conforms to the `spine-practitioner-2-0` profile;

The FGM Query request payload MAY include the FGM FHIR profile:

- Organization resource that conforms to the `spine-organization-1-0` profile;

<!-- ### Non Spine RBAC enabled NHS system ###

For all SMSP connections The FGM Query request payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the `spine-message-bundle-1-0` profile;
- MessageHeader resource that conforms to the `spine-Request-messageheader-1-0` profile;
- Parameters resource that conforms to the `spine-ris-parameters-1-0` profile;

The SMSP FGM Query request payload MAY include the FGM FHIR profile:

- Practitioner resource that conforms to the `spine-practitioner-1-0` profile;
- Organization resource that conforms to the `spine-organization-1-0` profile; -->

### FGM Query Parameters ###

The FGM Query parameters are conveyed in the spine-ris-parameters-1-0 profile and MUST include these parameters:

 - risk Indicator code fixed to 'FGM' (FGM status flag) 
 - business identifier (Verified NHS number for specified patient)



<!-- ```http
POST /fhir/fgm/query HTTP/1.1
```
- The `[valueString]` field for the Risk Indicator parameter SHALL be populated with the code: `FGM`.
- The `[valueString]` field for the NHS Number parameter SHALL be populated with the patienmt NHS Number identifier e.g.: `9999999999`. -->

### FGM Query Request ###

```xml
POST /fhir/fgm/query HTTP/1.1
Host: msg.int.spine2.ncrs.nhs.uk
SOAPAction: "urn:nhs:names:services:clinicals-sync/FGMQuery_2_0"
Content-Length: 3305
Content-Type: text/xml; charset=utf-8
Connection: close
<?xml version="1.0" encoding="UTF-8"?><Bundle><id value="13daadee-26e1-4d6a-9e6a-7f4af9b58870"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="14daadee-26e1-4d6a-9e6a-7f4af9b58870"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-request-messageheader-2-0"/></meta><timestamp value="2015-07-04T09:10:13+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMQuery_2_0"/></event><source><name value="Standon NHS Trust"/><software value="Standon Patient Manager"/><contact><system value="phone"/><value value="0207 333777"/></contact><endpoint value="urn:nhs:addressing:asid:047192794540"/></source><destination><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></destination><author><reference value="Practitioner/41fe704c-18e5-11e5-b60b-1697f925ec7b"/><display value="Dr Town Wood"/></author><data><reference value="Parameters/7cb73a48-090d-469a-a2b2-04f1e6b11ea1"/></data></MessageHeader></resource></entry><entry><resource><Parameters><id value="7cb73a48-090d-469a-a2b2-04f1e6b11ea1"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-parameters-1-0"/></meta><parameter><name value="RiskIndicator"/><valueString value="FGM"/></parameter><parameter><name value="NHSNumber"/><valueString value="9999999999"/></parameter></Parameters></resource></entry><entry><resource><Practitioner><id value="41fe704c-18e5-11e5-b60b-1697f925ec7b"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-practitioner-2-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/local-identifier"/><value value="12456"/></identifier><practitionerRole><managingOrganization><reference value="Organization/99daadee-26e1-4d6a-9e6a-7f4af9b99999"/></managingOrganization></practitionerRole></Practitioner></resource></entry><entry><resource><Organization xmlns="http://hl7.org/fhir"><id value="99daadee-26e1-4d6a-9e6a-7f4af9b99999"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-organization-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/ods-organization-code"/><value value="SDT"/></identifier><name value="Standon NHS Trust "/></Organization></resource></entry></Bundle>
```

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html)


### Query Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message that conforms to the `spine-message-bundle-1-0` profile;
- SHALL return a MessageHeader resource that conforms to the `spine-Response-messageheader-2-0` profile, containing either:
  - One matching `Flag` resource that conforms to the `spine-ris-flag-1-0` profile; 
  - The `Flag` resource SHALL contain a patient resource that conforms to the `spine-ris-patient-1-0` profile;

Spine 2 will return:
 
<!-- One `OperationOutcome` resource if the interaction is a success, however no FGM flag record has been found.  -->
<!-- - Where an Observation is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `observation` resource. -->


```xml
HTTP/1.1 200 OK
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
<?xml version="1.0" encoding="UTF-8"?><Bundle><id value="13daadee-26e1-4d6a-9e6a-7f4af9b58878"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="14daadee-26e1-4d6a-9e6a-7f4af9b58879"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-2-0"/></meta><timestamp value="2015-07-04T10:10:15+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMQueryResponse_2_0"/></event><response><identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/><code value="ok"/></response><source><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></source><destination><name value="FooBar NHS Trust"/><endpoint value="urn:nhs:addressing:asid:047192794544"/></destination><data><reference value="Flag/8cb73a48-090d-469a-a2b2-04f1e6b11ea2"/></data></MessageHeader></resource></entry><entry><resource><Flag><id value="8cb73a48-090d-469a-a2b2-04f1e6b11ea2"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-flag-1-0"/></meta><contained><Patient><id value="20daadee-26e1-4d6a-9e6a-7f4af9b58877"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-patient-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/nhs-number"/><value value="1234567890"/></identifier></Patient></contained><status value="active"/><period><start value="2015-02-04"/></period><subject><reference value="#20daadee-26e1-4d6a-9e6a-7f4af9b58877"/></subject><code><coding><system value="http://fhir.nhs.net/ValueSet/risk-indicator-type-1-0"/><code value="FGM"/></coding></code></Flag></resource></entry></Bundle>
```
OR the Query Response Message:

- SHALL return a `404` **Not Found** HTTP status code on successful execution of the interaction, however no FGM flag record has been found.
- SHALL return a `Bundle` of `type` message that conforms to the `spine-message-bundle-1-0` profile;
- SHALL return a MessageHeader resource that conforms to the `spine-Response-messageheader-2-0` profile, containing:
  - The `OperationOutcome` resource which MUST conform to the `spine-operationoutcome-1-0` profile; 


Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the search cannot be executed.
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



- The error codes are defined in the [spine-response-code-2-0](/ValueSets/spine-response-code-2-0.xml) ValueSet.
- Error INVALID_NHS_NUMBER would occur if the NHS number being requested in the search request is invalid.
- Error ASID_CHECK_FAILED comes from the old SOAP error for Access Denied with is already used throughout Spine when the endpoint lookup fails.

If the NHS number being requested in the search request is invalid the Spine 2 will return:


```xml
HTTP/1.1 400 Bad Request
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
<?xml version='1.0' encoding='UTF-8'?>
<Bundle><id value="22daadee-26e1-4d6a-9e6a-7f4af9b58892"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="14daadee-26e1-4d6a-9e6a-7f4af9b58879"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-2-0"/></meta><timestamp value="2015-07-04T10:10:15+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMQueryResponse_2_0"/></event><response><identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/><code value="fatal-error"/></response><source><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></source><destination><name value="FooBar NHS Trust"/><endpoint value="urn:nhs:addressing:asid:047192794544"/></destination></MessageHeader></resource></entry><entry><resource><OperationOutcome><id value="22daadee-26e1-4d6a-9e6a-7f4af9b58877"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-operationoutcome-1-0"/></meta><issue><severity value="error"/><code value="invalid"/><details><coding><system value="http://fhir.nhs.net/ValueSet/spine-response-code-2-0"/><code value="INVALID_NHS_NUMBER"/><display value="Invalid NHS Number"/></coding></details></issue></OperationOutcome></resource></entry></Bundle>

```

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
