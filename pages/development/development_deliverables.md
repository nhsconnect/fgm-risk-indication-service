---
title: FGM-IS Service Development Assets
keywords: development deliverables
tags: [development]
sidebar: overview_sidebar
permalink: development_deliverables.html
summary: "FGM Service Development Assets."
---

## FGM-IS Query Request Profiles ##

FGM-IS Query Request:

| Profile| Example | ValueSets | Sample Code |
| :--------- | :-----: |:-----: |:-----: |
| [Spine-Message-Bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) | [Bundle](http://data.developer.nhs.uk/fhir/fgm/examples/Profile.FGMRISQueryRequest/Example-qr-1a.xml) | [valueset-bundle-type](http://hl7.org/fhir/DSTU2/valueset-bundle-type.html) | [Development Example Code - Coming Soon] |
| [Spine-Request-MessageHeader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-request-messageheader-2-0) | [MessageHeader](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Request-MessageHeader-1-0) | [message-event-2-0.](https://fhir.nhs.uk/ValueSet/message-event-2-0) |  |
| [Spine-RIS-Parameters-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-parameters-1-0) |  | |  |
| [Spine-Practitioner-2-0](https://fhir.nhs.uk/StructureDefinition/spine-practitioner-2-0) |  | |  |
| [Spine-Organization-1-0](https://fhir.nhs.uk/StructureDefinition/spine-organization-1-0) |  | |  |



FGM-IS Query Request Response:

| Profile| Example | ValueSets | Sample Code |
| :--------- | :-----: |:-----: |:-----: |
| [Spine-Message-Bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) | [Bundle](http://data.developer.nhs.uk/fhir/fgm/examples/Profile.FGMRISQueryRequest/Example-qr-1a.xml) | [valueset-bundle-type](http://hl7.org/fhir/DSTU2/valueset-bundle-type.html) | [Development Example Code - Coming Soon] |
| [Spine-Response-MessageHeader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-response-messageheader-2-0) | [MessageHeader](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Request-MessageHeader-1-0) | [message-event-2-0.](https://fhir.nhs.uk/ValueSet/message-event-2-0) |  |
| [Spine-RIS-Flag-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-flag-1-0) |  |[risk-indicator-type-1-0](https://fhir.nhs.uk/ValueSet/risk-indicator-type-1-0) |  |
| [Spine-OperationOutcome-1-0](https://fhir.nhs.uk/StructureDefinition/spine-operationoutcome-1-0) | | [spine-response-code-2-0](https://fhir.nhs.uk/ValueSet/spine-response-code-2-0)|  |
| [Spine-RIS-Patient-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-patient-1-0) |  | |  |

## FGM-IS Create Request Profiles ##

FGM-IS Create Request:

| Profile| Example | ValueSets | Sample Code |
| :--------- | :-----: |:-----: |:-----: |
| [Spine-Message-Bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) | [Bundle](http://data.developer.nhs.uk/fhir/fgm/examples/Profile.FGMRISQueryRequest/Example-qr-1a.xml) | [valueset-bundle-type](http://hl7.org/fhir/DSTU2/valueset-bundle-type.html) | [Development Example Code - Coming Soon] |
| [Spine-Request-MessageHeader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-request-messageheader-2-0) | [MessageHeader](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Request-MessageHeader-1-0) | [message-event-2-0.](https://fhir.nhs.uk/ValueSet/message-event-2-0) |  |
| [Spine-RIS-Flag-1-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-flag-1-0) | | |  |
| [Spine-Practitioner-1-0](https://fhir.nhs.uk/StructureDefinition/spine-practitioner-1-0) | | |  |
| [Spine-Organization-1-0](https://fhir.nhs.uk/StructureDefinition/spine-organization-1-0) | | |  |
| [Spine-GP-Organization-1-0](https://fhir.nhs.uk/StructureDefinition/spine-gp-organization-1-0) | | |  |
| [Spine-RIS-Patient-2-0](https://fhir.nhs.uk/StructureDefinition/spine-ris-patient-2-0) | | |  |



FGM-IS Create Request Response:

| Profile| Example | ValueSets | Sample Code |
| :--------- | :-----: |:-----: |:-----: |
| [Spine-Message-Bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) | [Bundle](http://data.developer.nhs.uk/fhir/fgm/examples/Profile.FGMRISQueryRequest/Example-qr-1a.xml) | [valueset-bundle-type](http://hl7.org/fhir/DSTU2/valueset-bundle-type.html) | [Development Example Code - Coming Soon] |
| [Spine-Response-MessageHeader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-response-messageheader-2-0) | [MessageHeader](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Request-MessageHeader-1-0) | [message-event-2-0.](https://fhir.nhs.uk/ValueSet/message-event-2-0) |  |
| [Spine-OperationOutcome-1-0](https://fhir.nhs.uk/StructureDefinition/spine-operationoutcome-1-0) | | [spine-response-code-2-0](https://fhir.nhs.uk/ValueSet/spine-response-code-2-0)|  |



## FGM-IS Delete Request Profiles ##

FGM-IS Delete Request:

| Profile| Example | ValueSets | Sample Code |
| :--------- | :-----: |:-----: |:-----: |
| [Spine-Message-Bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) | [Bundle](http://data.developer.nhs.uk/fhir/fgm/examples/Profile.FGMRISQueryRequest/Example-qr-1a.xml) | [valueset-bundle-type](http://hl7.org/fhir/DSTU2/valueset-bundle-type.html) | [Development Example Code - Coming Soon] |
| [Spine-Request-MessageHeader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-request-messageheader-2-0) | [MessageHeader](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Request-MessageHeader-1-0) | [message-event-2-0.](https://fhir.nhs.uk/ValueSet/message-event-2-0) |  |
| [Spine-FGMDelete-Parameters-1-0](https://fhir.nhs.uk/StructureDefinition/spine-fgmdelete-parameters-1-0) | | [fgm-delete-reason-codes-1-0](https://fhir.nhs.uk/ValueSet/fgm-delete-reason-codes-1-0)|  |
| [Spine-Practitioner-1-0](https://fhir.nhs.uk/StructureDefinition/spine-practitioner-1-0) | | |  |
| [Spine-Organization-1-0](https://fhir.nhs.uk/StructureDefinition/spine-organization-1-0) | | |  |



FGM-IS Delete Request Response:

| Profile| Example | ValueSets | Sample Code |
| :--------- | :-----: |:-----: |:-----: |
| [Spine-Message-Bundle-1-0](https://fhir.nhs.uk/StructureDefinition/spine-message-bundle-1-0) | [Bundle](http://data.developer.nhs.uk/fhir/fgm/examples/Profile.FGMRISQueryRequest/Example-qr-1a.xml) | [valueset-bundle-type](http://hl7.org/fhir/DSTU2/valueset-bundle-type.html) | [Development Example Code - Coming Soon] |
| [Spine-Response-MessageHeader-2-0](https://fhir.nhs.uk/StructureDefinition/spine-response-messageheader-2-0) | [MessageHeader](http://data.developer.nhs.uk/fhir/fgm/Profile.FGMRISQueryRequest/Examples.html#Spine-Request-MessageHeader-1-0) | [message-event-2-0.](https://fhir.nhs.uk/ValueSet/message-event-2-0) |  |
| [Spine-OperationOutcome-1-0](https://fhir.nhs.uk/StructureDefinition/spine-operationoutcome-1-0) | | [spine-response-code-2-0](https://fhir.nhs.uk/ValueSet/spine-response-code-2-0)|  |