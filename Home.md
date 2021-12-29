# Introduction

Welcome to the home page of the Decentralized Skilling and Education Protocol (DSEP) Open API Specifications. This page serves as the primary documentation of the DSEP specs, high level architecture and other details.


# Network Architecture

The purpose of the DSEP APIs is to enable interoperability between Skill, Employment and Education provider (trainers, content providers, training centers, assessors, institutes) and their respective consumer. The idea is to enable various use-cases in the Skill, Employment and Education domain. The driving philosophy is to empower the providers and the consumers by providing them with DSEP compliant applications that can converse with each other regardless of the provider of the application. 

While the focus of this project is to bring forth interoperability by standardising the provider-consumer interfaces, we have envisioned a layered architecture that sets out to tie multiple layers together to bring to life the aforementioned use-cases.

![DSEP Network Architecture](https://github.com/beckn/DSEP-Specification/blob/documentation/docs/images/DSEP-Network-Architecture.png)



The details of the layers is as follows:

## Layer I: Infrastructure Layer

This layer consists of the underlying building blocks that are essential for the skill, employment and education ecosystem to come to life. These consists of functional registries of skill hubs, trainers, assessors, assessment agencies etc. as well as key components like Digital Identity, PKI Infrastructure to bring in Digital Signature capability, Payment and Settlement Infrastructure to enable digital payments, Data Protection and Consent Frameworks to ensure data privacy and consented data sharing, Document Store to store requisite document that needs to be exchanged, Rating and Reputation Infrastructure to provide facility to rate network participants and/or individuals, and online dispute resolution to address any grievances that arises out of transactions happening on DSEP Network.

## Layer II: Interoperability Layer via. DHP Spec

This the primary area of focus for this project and is the transport layer between the skill, employment and education provider and consumers enabling interoperability between the two. This layer consist of API specifications for both the consumers & provider. It leverages the underlying layer by generalising the objects that carries information between the provider & consumer and vice-versa.

## Layer III: Backend Modules/Microservices

This layer is the actual backend microservices that implements the API specifications detailed in Layer II. The microservices can have additional APIs/functionality on top of the Layer II interoperability specs to bring forth innovation in the skill, employment and education ecosystem.

## Layer IV: Experience Layer

This layer is the user facing layer and form the UI/UX for the end-user.

# Design Principles [TBD]

# Acknowledgments

The DSEP specs are extended from the [Beckn protocol](https://becknprotocol.io/). 
