---
title: FHIR Messaging Architecture
keywords: development messaging architecture
tags: [development]
sidebar: overview_sidebar
permalink: development_messaging_architecture.html
summary: "FGM Service Messaging API."
---

## FGM RIS Spine 2 interface Overview ##

This section provides FGM implementers with an overview of the FGM Risk Indication System Spine 2 FHIR messaging interface also known as the 'FGM Service'.  

The FGM Service supports the following functionality:

- Query for patient FGM status
- Create patient FGM flag
- Delete patient FGM flag


The FGM Service is based on the [HL7 FHIR DSTU2 1.0.1 Messaging Implementation](http://hl7.org/fhir/messaging.html) (Sept 2015) Messaging Implementation. 



## FGM FHIR Message Patterns and Structures ##

In FHIR messaging, a "message" is sent from a source application to a destination application when an event happens. Events mostly correspond to things that happen in the real world. 

The FGM Service utilises a synchronous request / response pattern. It is synchronous with respect to HTTP connections which means that only a single HTTP connection 
is required to perform a complete request.

This message communication pattern is similar to the Spine 1 'Web Service Mode'. However, as the FHIR message payload
contains the required sender and receiver endpoint information there is no longer any need to support the legacy HL7 v3 transport wrapper, WS-Addressing or for the SOAP header information within the structure.

The FGM request / response FHIR message payloads consist of combined FHIR resources that are bundled together within a FHIR Bundle wrapper to form the FHIR message structure. The Bundle resource is a container for a collection of resources identified by the type "message", with the first 
resource in the bundle being a MessageHeader resource.

The MessageHeader resource has a message event code (MessageHeader.event.code) that identifies the nature of the request/ response message. The message event in FGM is the SOAPAction (service and action) URI once carried as an HTTP Header e.g. urn:nhs:names:services:clinicals-sync:FGMQuery_1_0

The MessageHeader.source.endpoint and  the MessageHeader.destination.endpoint elements identify the sender and receiver addresses and are used for authorisation and audit purposes. An example of a sender address URN is: urn:nhs:addressing:asid:047192794544

Any resources referenced in the MessageHeader or child resources must be included in the bundle.

For implementers of the FGM Query 'read only' interface this DMS should be read in conjunction with the 'FGM Service' Implementation Guide: [FGM_implementation_guide-v1].

<font color="red">**Note**

Implementation guidance for the FGM Create and Delete 'update' Spine 2 interfaces TBA. 
</font>


[FGM_implementation_guide-v1]: images/fgm/FGM_implementation_guide-v1.pdf


## QRY-FGM-QueryRequest-2-0 Interaction ## 

The client system will construct an FGM Query Request message and send it to Spine 2.

- *Sender:* FGM Client
- *Receiver:* Spine 2
- *Message: Wire Format:* [FGM-QueryRequest-2-0]

** Acknowledgements **

HTTP Response.

** Responses **

Spine 2 **must** send the following response:

*FGM Query Response* - [RSP-FGM-QueryRequestResponse-2-0](#RSP-FGM-QueryRequestResponse-2-0)

----------

## QRY-FGM-QueryRequest-2-0 Interaction ## 

The client system will construct an FGM Query Request message and send it to Spine 2.

|Interaction | Sender| Receiver | Message: Wire Format | Acknowledgements | Responses |
| :---------| :--------- | :-----: |:-----: |:-----: | :-----: |
| QRY-FGM-QueryRequest-2-0 | [FGM Client]| [Spine 2] | [FGM-QueryRequest-2-0] | [HTTP Response] | [RSP-FGM-QueryRequestResponse-2-0](#RSP-FGM-QueryRequestResponse-2-0) OR |
| | | | |

----------

**<a name="RSP-FGM-QueryRequestResponse-2-0"></a> RSP-FGM-QueryRequestResponse-2-0 Interaction** 

Assuming successful transport of the FGM-QueryRequest-2-0 message, Spine 2 will construct an FGM Query Response message and send it to the FGM Client.

- *Sender:* Spine 2 
- *Receiver:* FGM Client
- *Message: Wire Format:* [FGM-QueryRequestResponse-2-0]

There are three possible outcomes to the Query Request:

- **Invalid Query:** SPINE rejects the FGM query due to business rules around the query construct.
- **FGM Entry Found:** SPINE executes the FGM query and there is an FGM risk entry for the patient.
- **No FGM Entry Found:** SPINE executes the FGM query and there is no FGM risk entry for the patient.



## NOT-FGM-CreateFlagRequest-1-0 Interaction ## 

The client system will construct an FGM Create Flag message and send it to Spine 2.

- *Sender:* FGM Client
- *Receiver:* Spine 2
- *Message: Wire Format:* [FGM-CreateFlagRequest-1-0]

**Acknowledgements**

HTTP Response.

**Responses**

Spine 2 **must** send the following response:

*FGM CreateFlag Response* - [RSP-FGM-CreateFlagRequestResponse-1-0](#RSP-FGM-CreateFlagRequestResponse-1-0)

----------

**<a name="RSP-FGM-CreateFlagRequestResponse-1-0"></a> RSP-FGM-CreateFlagRequestResponse-1-0 Interaction** 

Assuming successful transport of the FGM-CreateFlagRequest-1-0 message, Spine 2 will construct an FGM Create Flag Response message and send it to the FGM Client.

- *Sender:* Spine 2 
- *Receiver:* FGM Client
- *Message: Wire Format:* [FGM-CreateFlagResponse-1-0]

There are two possible outcomes to the FGM-CreateFlagRequest-1-0:

- **FGM Create:** SPINE executes the FGM create flag request and an FGM risk entry for the patient is created on Spine 2.
- **Invalid Create Request:** SPINE rejects the FGM create flag request due to business rules around the create flag construct.

----------

## NOT-FGM-DeleteRequest-1-0 Interaction ## 

The client system will construct an FGM Delete message and send it to Spine 2.

- *Sender:* FGM Client
- *Receiver:* Spine 2
- *Message: Wire Format:* [FGM-DeleteRequest-1-0]

**Acknowledgements**

HTTP Response.

**Responses**

Spine 2 **must** send the following response:

*FGM Delete Response* - [RSP-FGM-DeleteRequestResponse-1-0](#RSP-FGM-DeleteRequestResponse-1-0)

----------

**<a name="RSP-FGM-DeleteRequestResponse-1-0"></a> RSP-FGM-DeleteRequestResponse-1-0 Interaction** 

Assuming successful transport of the FGM-CreateFlagRequest-1-0 message, Spine 2 will construct an FGM Delete Response message and send it to the FGM Client.

- *Sender:* Spine 2 
- *Receiver:* FGM Client
- *Message: Wire Format:* [FGM-DeleteResponse-1-0]

There are two possible outcomes to the FGM-DeleteRequest-1-0:

- **FGM Delete:** SPINE executes the FGM delete request and the FGM risk entry for the patient is removed from Spine 2.
- **Invalid Delete Request:** SPINE rejects the FGM delete request due to business rules around the delete message construct.

----------



## FGM Risk Indication System Spine 2 Interaction Diagram ##


The diagram shows the FGM Risk Indication System Spine 2 Interactions:

<!-- </br>

<div style="display: block;"><img  src="FGMFHIRInteractions.png" alt="Interactions"></div>  
<br> -->

![stuff](images/fgm/FGMFHIRInteractions.png)



## Further Information ##
  
For more information about FHIR messaging please visit: [HL7 FHIR DSTU2 1.0.1 Messaging Implementation](http://hl7.org/fhir/messaging.html) and for the 'FGM Service' read only interface see: [FGM_implementation_guide-v1](images/fgm/FGM_implementation_guide-v1.pdf).

Quick links to FHIR reference implementations and implementation tool downloads are available from the [FHIR DSTU2 1.0.1](http://hl7.org/fhir/index.html)(Sept 2015) specification.