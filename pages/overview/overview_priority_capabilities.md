---
title: Priority Capabilities for FGM Service
keywords: usecases
tags: [development]
sidebar: overview_sidebar
toc: false
permalink: overview_priority_capabilities.html
summary: A brief introduction to the priority FGM-IS Service capabilities.
---

FGM Service is initially focussing on delivering two main interoperability capabilities:

# FGM View Only Capability #

A ‘view only’ capability within local NHS systems and Spine Mini Service Providers (SMSP). This will allow fully integrated query and response access to national FGM Information Sharing (FGM-IS) from local NHS systems and SMSP’s.

## Query for patient FGM status ##

The FGM client will construct a FHIR FGM query message and send it to the SPINE:

Assuming successful transport, there are three possible outcomes:

- SPINE rejects the query due to business rules around the query construct
- SPINE executes the query and there is an FGM-IS entry for the patient
- SPINE executes the query and there is no FGM-IS entry for the patient

## FGM RIS Query and Response functionality ##

This activity diagram below illustrates interactive FGM-IS Query and Response functionality.

![stuff](images/fgm/FGM Activity Diagram_CapabilitiesPage.png)

<a href="images/fgm/FGM Activity Diagram_CapabilitiesPage.png" target="_blank" style="width: 100%;max-width: 100%;"><b>Click to open in a new window</b></a>

# FGM Update Capability #

An ‘update’ capability within local NHS systems. Enabling additional create and delete capability for local NHS systems integration to the FGM-IS.

This functionality is only applicable to local NHS IT systems that are Spine compliant and use National Role Based Access Control (RBAC). The FGM-IS can only be updated by authorised healthcare professionals using Smartcards with the correct RBAC activity codes. 



## Create patient FGM flag

The FGM-IS client will construct a FHIR create FGM-IS flag message and send it to the SPINE

Assuming successful transport, there are two possible outcomes:

- SPINE rejects the create flag request due to business rules around the message construct
- SPINE executes the create flag request and there is an FGM-IS risk entry created for the patient on Spine


## Delete patient FGM flag

The FGM-IS client will construct a FHIR delete FGM-IS flag request message and send it to the SPINE

Assuming successful transport, there are two possible outcomes:

- SPINE rejects the delete flag request due to business rules around the message construct
- SPINE executes the delete flag request and the FGM-IS entry is deleted for the patient on Spine



