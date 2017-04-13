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

## FGM Create ##

### FGM Create Request ###

For all FGM Create request interactions the payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the `spine-message-bundle-1-0` profile;
- MessageHeader resource that conforms to the `spine-Request-messageheader-2-0` profile;
- Flag resource that conforms to the `spine-ris-flag-1-0` profile;
- Practitioner resource that conforms to the `spine-practitioner-1-0` profile;
- Patient resource that conforms to the `spine-ris-patient-2-0` profile;
- GP Organization resource that conforms to the `spine-gp-organization-1-0` profile;

The FGM Create request payload MAY include the FGM FHIR profile:

- Organization resource that conforms to the `spine-organization-1-0` profile;

### FGM Create Request ###

```xml
POST /fhir/fgm/query HTTP/1.1
Host: msg.int.spine2.ncrs.nhs.uk
SOAPAction: "urn:nhs:names:services:clinicals-sync/FGMCreate_1_0"
Content-Length: 3305
Content-Type: text/xml; charset=utf-8
Connection: close
<?xml version="1.0" encoding="UTF-8"?><Bundle><id value="13daadee-26e1-4d6a-9e6a-7f4af9b58877"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="29daadee-26e1-4d6a-9e6a-7f4af9b44444"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-request-messageheader-2-0"/></meta><timestamp value="2015-07-04T09:10:14+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMCreate_1_0"/></event><source><name value="FooBar NHS Trust"/><software value="FooBar Patient Manager"/><contact><system value="phone"/><value value="0207 444777"/></contact><endpoint value="urn:nhs:addressing:asid:047192794544"/></source><destination><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></destination><author><reference value="Practitioner/41fe704c-18e5-11e5-b60b-1697f925ec7b"/></author><data><reference value="Flag/7cb73a48-090d-469a-a2b2-04f1e6b11ea3"/></data></MessageHeader></resource></entry><entry><resource><Flag><id value="7cb73a48-090d-469a-a2b2-04f1e6b11ea3"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-flag-1-0"/></meta><!--contained patient--><contained><Patient><id value="20daadee-26e1-4d6a-9e6a-7f4af9b58877"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-patient-2-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/nhs-number"/><value value="1234567890"/></identifier><!--DOB added--><birthDate value="2005-01-01"/><!--GP Organization referenced.--><careProvider><reference value="#99600119-ebaf-4362-bb89-d473a33b1675"/></careProvider></Patient></contained><!--contained sibling GP Organization--><contained><Organization><id value="99600119-ebaf-4362-bb89-d473a33b1675"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-gp-organization-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/ods-organization-code"/><!--GP Practice Code--><value value="B86022"/></identifier></Organization></contained><status value="active"/><period><start value="2015-02-04"/></period><subject><reference value="#20daadee-26e1-4d6a-9e6a-7f4af9b58877"/></subject><code><coding><system value="http://fhir.nhs.net/ValueSet/risk-indicator-type-1-0"/><code value="FGM"/></coding></code></Flag></resource></entry><entry><resource><Practitioner><id value="41fe704c-18e5-11e5-b60b-1697f925ec7b"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-practitioner-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/sds-user-id"/><value value="200009876204"/></identifier><identifier><system value="http://fhir.nhs.net/Id/sds-role-profile-id"/><value value="100077650987"/></identifier><practitionerRole><managingOrganization><reference value="Organization/13daadee-26e1-4d6a-9e6a-7f4af9b58878"/></managingOrganization></practitionerRole></Practitioner></resource></entry><entry><resource><Organization><id value="13daadee-26e1-4d6a-9e6a-7f4af9b58878"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-organization-1-0"/></meta><identifier><system value="http://fhir.nhs.net/Id/ods-organization-code"/><value value="RKE"/></identifier><name value="THE WHITTINGTON HOSPITAL NHS TRUST"/></Organization></resource></entry></Bundle>
```

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html)


### Create Response ###

Success:

- SHALL return a `201` **Created** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message that conforms to the `spine-message-bundle-1-0` profile;
- SHALL return a MessageHeader resource that conforms to the `spine-Response-messageheader-2-0` profile.


Spine 2 will return:
  


```xml
HTTP/1.1 201 Created
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
<?xml version="1.0" encoding="UTF-8"?><Bundle><id value="13daadee-26e1-4d6a-9e6a-7f4af9b58878"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="14daadee-26e1-4d6a-9e6a-7f4af9b58879"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-2-0"/></meta><timestamp value="2015-07-04T10:10:15+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMCreateResponse_1_0"/></event><response><identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/><code value="ok"/></response><source><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></source><destination><name value="FooBar NHS Trust"/><endpoint value="urn:nhs:addressing:asid:047192794544"/></destination></MessageHeader></resource></entry></Bundle>
```

Failure: 

- SHALL return one of the below HTTP status error codes with an `OperationOutcome` resource that conforms to the `spine-operationoutcome-1` profile if the create flag cannot be executed.
- The below table summarises the types of error that could occur, and the HTTP response codes, along with the values to expect in the `OperationOutcome` response body.

| HTTP Code | issue-severity | issue-type | Details.Code | Details.Display | Orignal codes |
|-----------|----------------|------------|--------------|-----------------|-----------------|
|404 | error | not-found | NO_RECORD_FOUND | No Record Found| FGM-0001 |
|400 | error | invalid | INVALID_NHS_NUMBER | Invalid NHS number| FGM-0002 |
|400 | error | invalid | PATIENT_OVER_18 | Patient is over 18 | FGM-0003 |
|400 | error | invalid | INVALID_PARAMETER | Invalid parameter | FGM-0004 |
|422 | warning | informational | FLAG_ALREADY_SET | Flag value was already set | FGM-0005 |
|400 | error | structure | MESSAGE_NOT_WELL_FORMED | Message not well formed | FGM-9999 |
|403 | error | forbidden | ASID_CHECK_FAILED | The sender or receiver's ASID is not authorised for this interaction | 300 |

- The error codes are defined in the [spine-response-code-2-0](/ValueSets/spine-response-code-2-0.xml) ValueSet.
- Error INVALID_NHS_NUMBER would occur if the NHS number being requested in the search request is invalid.
- Error ASID_CHECK_FAILED comes from the old SOAP error for Access Denied with is already used throughout Spine when the endpoint lookup fails.

If the NHS number used in the create request is invalid the Spine 2 will return:

```xml
HTTP/1.1 400 Bad Request
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
<?xml version='1.0' encoding='UTF-8'?><Bundle><id value="22daadee-26e1-4d6a-9e6a-7f4af9b58892"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/></meta><type value="message"/><entry><resource><MessageHeader><id value="14daadee-26e1-4d6a-9e6a-7f4af9b58879"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-2-0"/></meta><timestamp value="2015-07-04T10:10:15+00:00"/><event><system value="http://fhir.nhs.net/ValueSet/message-event-2-0"/><code value="urn:nhs:names:services:clinicals-sync:FGMQueryResponse_2_0"/></event><response><identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/><code value="fatal-error"/></response><source><name value="SPINE"/><endpoint value="urn:nhs:addressing:asid:990101234567"/></source><destination><name value="FooBar NHS Trust"/><endpoint value="urn:nhs:addressing:asid:047192794544"/></destination></MessageHeader></resource></entry><entry><resource><OperationOutcome><id value="22daadee-26e1-4d6a-9e6a-7f4af9b58877"/><meta><profile value="http://fhir.nhs.net/StructureDefinition/spine-operationoutcome-1-0"/></meta><issue><severity value="error"/><code value="invalid"/><details><coding><system value="http://fhir.nhs.net/ValueSet/spine-response-code-2-0"/><code value="INVALID_NHS_NUMBER"/><display value="Invalid NHS Number"/></coding></details></issue></OperationOutcome></resource></entry></Bundle>
```

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html)

### Example Code ###


#### Java ####

```java
FhirContext ctx = FhirContext.forDstu2();
Bundle bndl = CreateFGMFlagXML.createBundle();
MessageHeader mh = CreateFGMFlagXML.createMessageHeader();
Flag flag = CreateFGMFlagXML.createFlag();
Practitioner prac = CreateFGMFlagXML.createPractitioner();
Organization orgn = CreateFGMQueryv2.createOrganization();
final Entry entryBundle = bndl.addEntry();		
final Entry entryMh = bndl.addEntry();
final Entry entryFlag = bndl.addEntry();
final Entry entryPrac = bndl.addEntry();
final Entry entryOrg = bndl.addEntry();
entryBundle.setResource(mh);
entryFlag.setResource(flag);
entryPrac.setResource(prac);
entryOrg.setResource(orgn);
String output = ctx.newXmlParser().setPrettyPrint(true).encodeResourceToString(bndl);
System.out.println(output);
String serverBaseUrl = "http://fhirtest.uhn.ca/baseDstu2";
IGenericClient client = ctx.newRestfulGenericClient(serverBaseUrl);
MethodOutcome outcome = client.create().resource(bndl).execute();
System.out.println(outcome.getId());
```

