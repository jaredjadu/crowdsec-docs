---
id: intro
title: Service API Presentation
---

:::info
This is an [enterprise plan](https://www.crowdsec.net/pricing) feature
:::

## What is the Service API ?

The **Service API**, **SAPI** for short, provides access to selected **CrowdSec SaaS features**.
New SaaS features will usually appear on **SAPI** first before getting their UI counterpart.
The current capabilities of this API are:

-  [**Blocklist** creation & management](/u/service_api/blocklists)
   -  Allowing you to create private blocklists and share them 
   -  As well as subscribing to any of the blocklists available to your organization
-  [**Integrations** endpoints creation & management](/u/service_api/integrations)
   -  An Essential part of the **Blocklist as a Service** feature.
   -  Manage endpoints for your [**Firewalls**](/u/integrations/intro) or [**Remediation Components**](/u/bouncers/intro) to connect directly to.

## Getting your API keys

The **Service API** is different from the **CTI API**.  
You can create and retrieve your API keys in your console account's **Settings** section for your eligible organization(s)

[You can find a dedicated guide here.](/u/console/api/intro)


## API Specification

You can find a detailed description of the API in multiple formats here:

 - [Swagger](https://admin.api.crowdsec.net/v1/docs#/)
 - [Redoc](https://admin.api.dev.crowdsec.net/v1/redoc)
 - [Openapi specs](https://admin.api.crowdsec.net/v1/openapi.json)



