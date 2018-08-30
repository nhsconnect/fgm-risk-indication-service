---
title: FGM Delete Flag
keywords: development fgm delete
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_delete.html
summary: "FGM Delete/ Response FHIR Messaging API."
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


## FGM Delete ##

### FGM Delete Request ###

For all FGM Delete request interactions the payload MUST include these FGM FHIR profiles:

- Bundle resource that conforms to the `spine-message-bundle-1-0` profile;
- MessageHeader resource that conforms to the `spine-Request-messageheader-2-0` profile;
- Parameters resource that conforms to the `spine-fgmdelete-parameters-1-0` profile;

The FGM Delete request payload MAY include the FGM FHIR profile:

- Organization resource that conforms to the `spine-organization-1-0` profile;


### FGM Delete Request ###

<div style="border:solid 1px; border-color: #c3c8cc;">

<pre>
POST /fhir/fgm/query HTTP/1.1
Host: msg.int.spine2.ncrs.nhs.uk
SOAPAction: "urn:nhs:names:services:clinicals-sync/FGMDelete_1_0"
Content-Length: 3305
Content-Type: text/xml; charset=utf-8
Connection: close
</pre>

<script src="https://gist.github.com/sufyanpat/323b9265b995b452f80c7d7e24ddc23a.js"></script>
</div>

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html)

### Delete Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message that conforms to the `spine-message-bundle-1-0` profile;
- SHALL return a MessageHeader resource that conforms to the `spine-Response-messageheader-2-0` profile.

<div style="border:solid 1px; border-color: #c3c8cc;">

<pre>
HTTP/1.1 200 Ok
Server: nginx/1.10.1
Date: Fri, 06 Jan 2017 09:17:00 GMT
Content-Type: application/xml+fhir;charset=utf-8
Content-Length: 948
Connection: close
</pre>

<script src="https://gist.github.com/sufyanpat/a12c9f38da1edd72a58fc14af1a86d6e.js"></script>

</div>

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

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm-v2-draft-d/Chapter.5.Examples/examples.html)

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

