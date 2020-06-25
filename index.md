---
title: Introduction to the FGM Information Sharing Service
keywords: homepage
tags: [getting_started]
sidebar: home_sidebar
permalink: index.html
toc: false
summary: A brief introduction to getting started with the FGM Information Sharing Service FHIR&reg; Messaging API.
---

{% include important.html content="This site is under active development by NHS Digital and is intended to provide all the technical resources you need to successfully develop the FGM Service FHIR Messaging API. Some areas are being formulated and iterative updates to content will be added on a regular basis." %}

# Background to FGM Information Sharing System #

Female Genital Mutilation (FGM) is an illegal, extremely harmful practice and a form of child abuse and violence. The Government is committed to preventing and ending this harmful practice which violates the rights of children and adults. 

The Department of Health (DH) in partnership with NHS England has launched a FGM Prevention Programme which aims to improve the NHS’ response to FGM, increase levels of safeguarding for children at risk of FGM and provide better subsequent support and care for those that have undergone FGM. 

As part of the FGM Prevention Programme, a national FGM Information Sharing System (FGM-IS) has been developed. This is a national IT service and forms part of the national Spine architecture. It allows FGM information for children under 18 to be stored and shared with relevant NHS healthcare professionals across departmental, organisational and geographical boundaries within England, supporting effective early intervention and ongoing safeguarding of children potentially at risk of FGM. 

The system was delivered in two stages:

**Stage 1: Delivery of the ‘Core Service’ -** the FGM-IS forms part of the national Spine architecture. From August 2015, the Summary Care Record application (SCRa) has included new functionality to allow authorised healthcare professionals to record, remove and view FGM information for children under the age of 18.   

**Stage 2: Delivery of ‘Integration Capabilities’ -** to make information held within the Core Service more widely available to healthcare staff by allowing integration of:

 - A ‘view only’ capability within local NHS systems and Spine Mini Service Providers (SMSP). This will allow fully integrated query and response access to national FGM information sharing system from local NHS systems and SMSP's. 
 - An ‘update’ capability within local NHS systems. Enabling additional create and delete capability for local NHS systems integration to the FGM-IS. This functionality is only applicable to local NHS IT systems that are Spine compliant and use National Role Based Access Control (RBAC). The FGM-IS can only be updated by authorised healthcare professionals using Smartcards with the correct RBAC activity codes. 


{% include important.html content="This Implementation Guide is only concerned with FGM-IS **Stage 2: Delivery of ‘Integration Capabilities’**. Please note that the **'update' capability** is not currently available for those suppliers wishing to integrate FGM IS within their product offering. This will be kept under review by the NHS. Currently, the only functionality to be integrated into supplier systems is the **'view only' capability**." %}


# The FGM Vision #

The FGM System provides the ability to display an alert message when a patient with female genitalia under 18 years old has a family history of FGM, as recorded in the FGM-IS service.

The FGM-IS Service supports the wider Female Genital Mutilation Prevention (FGMP) Programme which is in place to protect, prevent and care for children and vulnerable adults who have a family history of FGM. This service will allow Healthcare professionals and other government organisations (with an appropriate relationship), to flag children and vulnerable adults who are at risk of FGM. This information will be available in the following settings: Spine 2, GP systems, Child Health Systems and other government agencies.

Find out more on the NHS Digital [Female Genital Mutilation - Information Sharing](https://digital.nhs.uk/services/female-genital-mutilation-information-sharing) homepage.



