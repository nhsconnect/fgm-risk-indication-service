---
title: FGM Query v1
keywords: development fgm query v1
tags: [development]
sidebar: overview_sidebar
permalink: development_fgm_query_v1.html
summary: "FGM Query/ Response FHIR Messaging API."
---

## Prerequisites ##

To use this Messaging API, the requester:

- SHALL have gone through accreditation and received an endpoint certificate and associated ASID (Accredited System ID) for the client system.
- SHALL have authenticated the user using national smartcard authentication, and obtained a ticket and the UUID from the user's smartcard 
- SHALL have previously traced the patient's NHS Number using PDS or an equivalent service.


## HTTP Request Headers ##

All Query connections to the FGM Service should include the below HTTP request headers:

| Header               | Value |
|----------------------|-------|
| `Host:`        | msg.int.spine2.ncrs.nhs.uk |
| `SOAPAction:`           | "urn:nhs:names:services:clinicals-sync/FGMQuery_1_0" |
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

### FGM Query Request ###

```xml
<Bundle xmlns="http://hl7.org/fhir">
  <id value="13daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
  <meta>
    <profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/>
  </meta>
  <type value="message"/>
  <entry>
    <resource>
      <MessageHeader>
        <id value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-request-messageheader-1-0"/>
        </meta>
        <timestamp value="2015-07-04T09:10:14+00:00"/>
        <event>
          <system value="http://fhir.nhs.net/ValueSet/message-event-1-0"/>
          <code value="urn:nhs:names:services:clinicals-sync:FGMQuery_1_0"/>
        </event>
        <source>
          <name value="FooBar NHS Trust"/>
          <software value="FooBar Patient Manager"/>
          <contact>
            <system value="phone"/>
            <value value="0207 444777"/>
          </contact>
          <endpoint value="urn:nhs:addressing:asid:047192794544"/>
        </source>
        <destination>
          <name value="SPINE"/>
          <endpoint value="urn:nhs:addressing:asid:990101234567"/>
        </destination>
        <author>
          <reference value="Practitioner/41fe704c-18e5-11e5-b60b-1697f925ec7b"/>
          <display value="Dr Town Wood"/>
        </author>
        <data>
          <reference value="Parameters/7cb73a48-090d-469a-a2b2-04f1e6b11ea2"/>
        </data>
      </MessageHeader>
    </resource>
  </entry>
  <entry>
    <resource>
      <Parameters>
        <id value="7cb73a48-090d-469a-a2b2-04f1e6b11ea2"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-parameters-1-0"/>
        </meta>
        <parameter>
          <name value="RiskIndicator"/>
          <valueString value="FGM"/>
        </parameter>
        <parameter>
          <name value="NHSNumber"/>
          <valueString value="9999999999"/>
        </parameter>
      </Parameters>
    </resource>
  </entry>
  <entry>
    <resource>
      <Practitioner>
        <id value="41fe704c-18e5-11e5-b60b-1697f925ec7b"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-practitioner-1-0"/>
        </meta>
        <identifier>
          <use value="official"/>
          <system value="http://fhir.nhs.net/Id/sds-user-id"/>
          <value value="G12345678"/>
        </identifier>
        <identifier>
          <use value="official"/>
          <system value="http://fhir.nhs.net/Id/sds-role-profile-id"/>
          <value value="PT1234"/>
        </identifier>
        <name>
          <use value="official"/>
          <family value="Wood"/>
          <given value="Town"/>
          <prefix value="Dr."/>
        </name>
        <practitionerRole>
          <managingOrganization>
            <reference value="Organization/13daadee-26e1-4d6a-9e6a-7f4af9b58878"/>
          </managingOrganization>
        </practitionerRole>
      </Practitioner>
    </resource>
  </entry>
  <entry>
    <resource>
      <Organization>
        <id value="13daadee-26e1-4d6a-9e6a-7f4af9b58878"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-organization-1-0"/>
        </meta>
        <identifier>
          <system value="http://fhir.nhs.net/Id/ods-organization-code"/>
          <value value="RKE"/>
        </identifier>
        <name value="THE WHITTINGTON HOSPITAL NHS TRUST"/>
      </Organization>
    </resource>
  </entry>
</Bundle>

```

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Message-Bundle-1-0) 


### Query Response ###

Success:

- SHALL return a `200` **OK** HTTP status code on successful execution of the interaction.
- SHALL return a `Bundle` of `type` message, containing either:
  - One matching `Flag` resource that conforms to the `spine-ris-flag-1-0` profile; or
 <!--  - One `OperationOutcome` resource if the interaction is a success, however no FGM flag record has been found. -->
<!-- - Where an Observation is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `observation` resource. -->


```xml
<Bundle xmlns="http://hl7.org/fhir">
  <id value="13daadee-26e1-4d6a-9e6a-7f4af9b58878"/>
  <meta>
    <profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/>
  </meta>
  <type value="message"/>
  <entry>
    <resource>
      <MessageHeader>
        <id value="14daadee-26e1-4d6a-9e6a-7f4af9b58879"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-1-0"/>
        </meta>
        <timestamp value="2015-07-04T10:10:15+00:00"/>
        <event>
          <system value="http://fhir.nhs.net/ValueSet/message-event-1-0"/>
          <code value="urn:nhs:names:services:clinicals-sync:FGMQueryResponse_1_0"/>
        </event>
        <response>
          <identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
          <code value="ok"/>
        </response>
        <source>
          <name value="SPINE"/>
          <endpoint value="urn:nhs:addressing:asid:990101234567"/>
        </source>
        <destination>
          <name value="FooBar NHS Trust"/>
          <endpoint value="urn:nhs:addressing:asid:047192794544"/>
        </destination>
        <data>
          <reference value="Flag/8cb73a48-090d-469a-a2b2-04f1e6b11ea2"/>
        </data>
      </MessageHeader>
    </resource>
  </entry>
  <entry>
    <resource>
      <Flag>
        <id value="8cb73a48-090d-469a-a2b2-04f1e6b11ea2"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-flag-1-0"/>
        </meta>
        <contained>
          <Patient>
            <id value="20daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
            <meta>
              <profile value="http://fhir.nhs.net/StructureDefinition/spine-ris-patient-1-0"/>
            </meta>
            <identifier>
              <system value="http://fhir.nhs.net/Id/nhs-number"/>
              <value value="1234567890"/>
            </identifier>
          </Patient>
        </contained>
        <status value="active"/>
        <period>
          <start value="2015-02-04"/>
        </period>
        <subject>
          <reference value="#20daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
        </subject>
        <code>
          <coding>
            <system value="http://fhir.nhs.net/ValueSet/risk-indicator-type-1-0"/>
            <code value="FGM"/>
          </coding>
        </code>
      </Flag>
    </resource>
  </entry>
</Bundle>

```
 - One `OperationOutcome` resource if the interaction is a success, however no FGM flag record has been found.
<!-- - Where an Observation is returned, it SHALL include the `versionId` and `fullUrl` of the current version of the `observation` resource. -->

```xml
<Bundle xmlns="http://hl7.org/fhir">
  <id value="20daadee-26e1-4d6a-9e6a-7f4af9b58890"/>
  <meta>
    <profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/>
  </meta>
  <type value="message"/>
  <entry>
    <resource>
      <MessageHeader>
        <id value="21daadee-26e1-4d6a-9e6a-7f4af9b58891"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-1-0"/>
        </meta>
        <timestamp value="2015-07-04T10:10:15+00:00"/>
        <event>
          <system value="http://fhir.nhs.net/ValueSet/message-event-1-0"/>
          <code value="urn:nhs:names:services:clinicals-sync:FGMQueryResponse_1_0"/>
        </event>
        <response>
          <identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
          <code value="ok"/>
          <details>
            <reference value="OperationOutcome/33daadee-26e1-4d6a-9e6a-7f4af9b58933"/>
          </details>
        </response>
        <source>
          <name value="SPINE"/>
          <endpoint value="urn:nhs:addressing:asid:990101234567"/>
        </source>
        <destination>
          <name value="FooBar NHS Trust"/>
          <endpoint value="urn:nhs:addressing:asid:047192794544"/>
        </destination>
      </MessageHeader>
    </resource>
  </entry>
  <entry>
    <resource>
      <OperationOutcome>
        <id value="33daadee-26e1-4d6a-9e6a-7f4af9b58933"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-operationoutcome-1-0 "/>
        </meta>
        <issue>
          <severity value="information"/>
          <code value="not-found"/>
          <details>
            <coding>
              <system value="http://fhir.nhs.net/ValueSet/spine-response-code-1-0"/>
              <code value="FGM-0001"/>
            </coding>
          </details>
          <diagnostics value="No FGM Record Found"/>
        </issue>
      </OperationOutcome>
    </resource>
  </entry>
</Bundle>


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
<Bundle xmlns="http://hl7.org/fhir">
  <id value="22daadee-26e1-4d6a-9e6a-7f4af9b58892"/>
  <meta>
    <profile value="http://fhir.nhs.net/StructureDefinition/spine-message-bundle-1-0"/>
  </meta>
  <type value="message"/>
  <entry>
    <resource>
      <MessageHeader>
        <id value="23daadee-26e1-4d6a-9e6a-7f4af9b58893"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-response-messageheader-1-0"/>
        </meta>
        <timestamp value="2015-07-04T10:10:15+00:00"/>
        <event>
          <system value="http://fhir.nhs.net/ValueSet/message-event-1-0"/>
          <code value="urn:nhs:names:services:clinicals-sync:FGMQueryResponse_1_0"/>
        </event>
        <response>
          <identifier value="14daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
          <code value="fatal-error"/>
          <details>
            <reference value="OperationOutcome/22daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
          </details>
        </response>
        <source>
          <name value="SPINE"/>
          <endpoint value="urn:nhs:addressing:asid:990101234567"/>
        </source>
        <destination>
          <name value="FooBar NHS Trust"/>
          <endpoint value="urn:nhs:addressing:asid:047192794544"/>
        </destination>
      </MessageHeader>
    </resource>
  </entry>
  <entry>
    <resource>
      <OperationOutcome>
        <id value="22daadee-26e1-4d6a-9e6a-7f4af9b58877"/>
        <meta>
          <profile value="http://fhir.nhs.net/StructureDefinition/spine-operationoutcome-1-0"/>
        </meta>
        <issue>
          <severity value="error"/>
          <code value="invalid"/>
          <details>
            <coding>
              <system value="http://fhir.nhs.net/ValueSet/spine-response-code-1-0"/>
              <code value="FGM-0002"/>
            </coding>
          </details>
          <diagnostics value="NHS Number Invalid"/>
        </issue>
      </OperationOutcome>
    </resource>
  </entry>
</Bundle>

```

- Additional examples are available here - [XML](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Message-Bundle-1-0) 

### Example Code ###

#### Java ####

```java
FhirContext ctx = FhirContext.forDstu2();
Bundle bndl = CreateXML.createBundle();
Organization orgn = CreateXML.createOrganization();
MessageHeader mh = CreateXML.createMessageHeader();
Parameters pm = CreateXML.createParameters();
Practitioner prac = CreateXML.createPractitioner();
final Entry entryBundle = bndl.addEntry();    
final Entry entryMh = bndl.addEntry();
final Entry entryParam = bndl.addEntry();
final Entry entryPrac = bndl.addEntry();
entryMh.setResource(mh);
entryBundle.setResource(orgn);
entryParam.setResource(pm);
entryPrac.setResource(prac);
String output = ctx.newXmlParser().setPrettyPrint(true).encodeResourceToString(bndl);
System.out.println(output);
String serverBaseUrl = "http://spine-base-url/fhir-base/;
IGenericClient client = ctx.newRestfulGenericClient(serverBaseUrl);
MethodOutcome outcome = client.create().resource(bndl).execute();
System.out.println(outcome.getId());
```
