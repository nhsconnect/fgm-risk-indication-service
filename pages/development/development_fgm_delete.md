---
title: FGM Delete Flag
keywords: development fgm delete
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_delete.html
summary: "FGM Delete/ Response FHIR Messaging API."
---


## Prerequisites ##

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have authenticated the user using national smartcard authentication, and obtained a ticket and the UUID from the user's smartcard 
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.


<!-- ## HTTP Request Headers ##

All Create connections to the FGM Service should include the below HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Host:`        | msg.int.spine2.ncrs.nhs.uk |
| `SOAPAction:`           | "urn:nhs:names:services:clinicals-sync/FGMCreate_1_0" |
| `Content-Length`             | e.g. 3305|
| `Content-Type: `  | `application/xml+fhir; charset=utf-8`|
| `Connection:`      | e.g. close |
 -->


## FGM Delete ##

### FGM Delete Request ###

For all FGM Delete request interactions the payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the `spine-message-bundle-1-0` profile;
- MessageHeader resource that conforms to the `spine-Request-messageheader-2-0` profile;
- Parameters resource that conforms to the `spine-fgmdelete-parameters-1-0` profile;

The FGM Delete request payload MAY include the FGM FHIR profile:

- Organization resource that conforms to the `spine-organization-1-0` profile;


### FGM Delete Request ###

```xml
POST /fhir/fgm/query HTTP/1.1
Host: msg.int.spine2.ncrs.nhs.uk
SOAPAction: "urn:nhs:names:services:clinicals-sync/FGMDelete_1_0"
Content-Length: 3305
Content-Type: text/xml; charset=utf-8
Connection: close
<?xml version="1.0" encoding="UTF-8"?><Bundle><id value="32daadee-26e1-4d6a-9e6a-7f4af9b50000"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="32daadee-26e1-4d6a-9e6a-7f4af9b50000"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-request-messageheader-2-0"/></meta><timestamp value="2015-07-04T09:10:14+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMDelete_1_0"/></event><source><name value="FooBar NHS Trust"/><software value="FooBar Patient Manager"/><contact><system value="phone"/><value value="0207 444777"/></contact><endpoint value="urn:nhs:addressing:asid:047192794544"/></source><destination><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></destination><author><reference value="Practitioner/41fe704c-18e5-11e5-b60b-1697f925ec7b"/></author><data><reference value="Parameters/7cb73a48-090d-469a-a2b2-04f1e6b11ea2"/></data></MessageHeader></resource></entry><entry><resource><Parameters><id value="7cb73a48-090d-469a-a2b2-04f1e6b11ea2"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-fgmdelete-parameters-1-0"/></meta><parameter><name value="RiskIndicator"/><valueString value="FGM"/></parameter><parameter><name value="NHSNumber"/><valueString value="9999999999"/></parameter><parameter><!--	FGMDeleteReason Fix--><name value="FGMDeleteReason"/><valueCodeableConcept><coding><system value="http://fhir.nhs.net/ValueSet/fgm-delete-reason-codes-1-0"/><code value="1"/></coding></valueCodeableConcept></parameter></Parameters></resource></entry><entry><resource><Practitioner><id value="41fe704c-18e5-11e5-b60b-1697f925ec7b"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-practitioner-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/sds-user-id"/><value value="200009876204"/></identifier><identifier><system value="http://fhir.nhs.net/Id/sds-role-profile-id"/><value value="100077650987"/></identifier><practitionerRole><managingOrganization><reference value="Organization/13daadee-26e1-4d6a-9e6a-7f4af9b58878"/></managingOrganization></practitionerRole></Practitioner></resource></entry><entry><resource><Organization><id value="13daadee-26e1-4d6a-9e6a-7f4af9b58878"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-organization-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/ods-organization-code"/><value value="RKE"/></identifier><name value="THE WHITTINGTON HOSPITAL NHS TRUST"/></Organization></resource></entry></Bundle>
```

<!-- - Additional examples are available here - [XML](Examples/Bundle-Observation.xml)  -->


### Delete Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message that conforms to the `spine-message-bundle-1-0` profile;
- SHALL return a MessageHeader resource that conforms to the `spine-Response-messageheader-2-0` profile.


```xml

TBA

```

Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the create flag cannot be executed.
- The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `OperationOutcome` in the response body.


| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Orignal codes |
|-----------|----------------|------------|--------------|-----------------|-----------------|
|404 | error | not-found | NO_RECORD_FOUND | No Record Found| FGM-0001 |
|400 | error | invalid | INVALID_NHS_NUMBER | Invalid NHS number| FGM-0002 |
|400 | error | invalid | PATIENT_OVER_18 | Patient is over 18 | FGM-0003 |
|400 | error | invalid | INVALID_PARAMETER | Invalid parameter | FGM-0004 |
|422 | warning | informational | FLAG_ALREADY_SET | Flag value was already set | FGM-0005 |
|400 | error | structure | MESSAGE_NOT_WELL_FORMED | Message not well formed | FGM-9999 |
|403 | error | forbidden | ASID_CHECK_FAILED | The sender or receiver's ASID is not authorised for this interaction | 300 |

- The error codes are defined in the [spine-response-code-2-0 ValueSet](/ValueSets/spine-response-code-2-0.xml)
- Error Invalid NHS number would occur if the NHS number being requested in the search request is invalid.
- Error ASID_CHECK_FAILED comes from the old SOAP error for Access Denied with is already used throughout Spine when the endpoint lookup fails.



```xml
TBA

```

<!-- - Additional examples are available here - [XML](Examples/Bundle-OperationOutcome.xml)  -->

### Example Code ###



#### Java ####

```java
FhirContext ctx = FhirContext.forDstu2();
Bundle bndl = DeleteFGMFlagXML.createBundle();
MessageHeader mh = DeleteFGMFlagXML.createMessageHeader();
Parameters pm = DeleteFGMFlagXML.createParameters();
Practitioner prac = DeleteFGMFlagXML.createPractitioner();
Organization orgn = DeleteFGMFlagXML.createOrganization();
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

