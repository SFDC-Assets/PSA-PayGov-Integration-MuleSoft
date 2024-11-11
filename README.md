![Public Sector Accelerators logo](/docs/Logo_GPSAccelerators_v01.png)

# Pay.gov Integration
**This Accelerator aims to provide a quick-start for integrating with Pay.gov for a common set of use cases.**

Accelerator Listing: [insert url to the public listing on the Accelerator site](https://gpsaccelerators.developer.salesforce.com/) (tbd once published)

## Description

This Accelerator provides a quick-start/bootstrap implementation to simplify integrations with two Pay.gov web services: _TCS Plastic Card Web Service_ & _TCS Single Query Web Service_. A Pay.gov user can use these to programatically create "plastic card" (referring to credit/debit card) transactions as well as query transaction details. These are SOAP based services where messages to and from the Pay.gov web service are in the form of XML payloads that conform to the specific schema defined by that web service.

**Please Note:** The Bureau of the Fiscal Service does not publish the specification for these web services unless an agency has formally gone through an approval process. As such, this Accelerator cannot provide exact field-level details about integrating with Pay.gov, please see the FAQ section for more details on this topic.

This Accelerator includes assets that can be used with MuleSoft AnyPoint Studio and customized for your own needs:
- An extensible, APIKit enabled MuleSoft API built for the Pay.gov _TCS Plastic Card_ & _TCS Single Query_ Web Services using RAML specifications
- Configuration of the Web Services Consumer Connector to demonstrate how these Pay.gov web services can be consumed

_This Accelerator does not provide:_
- Preparation of the XML payloads that are required to invoke the Pay.gov _TCS Plastic Card_ & _TCS Single Query_ Web Services
- Transformation of XML outputs from the Pay.gov web services to JSON format

#### About Pay.gov
Pay.gov is the U.S. government's secure web-based collection portal for making payments to federal agencies. Managed by the U.S. Department of the Treasury's Bureau of the Fiscal Service, Pay.gov provides a standardized way to process collections for government agencies through various payment methods including credit cards, debit cards, and ACH (Automated Clearing House) direct debit. The Pay.gov about page describes the system aptly as, "a free and secure service that allows you to pay many, but not all, United States Government agencies." As the description implies, many U.S. Government agencies rely on Pay.gov to collect and make payments for their day-to-day operations. As such Pay.gov becomes a critical part of the overall architecture for many agencies use cases.

**Interacting with Pay.gov**
At a high level there are two _categories_ of interactions with Pay.gov. Here we will refer to them as "user-interface based" vs "web-services based". The first category, "user-interface", encompass the interactions with Pay.gov that take place via a user interface. Pay.gov provides the ability for an agency to either host a form on their own website or for a user during their payment process to be redirected to a form hosted on the Pay.gov website. The second category, "web-services based", encompases interactions that take place via system-system webservices.  Pay.gov publishes a suite of web services that allow a user to accomplish various tasks. As of version 8.5 of Pay.gov some of these web services include:

1. Trusted Collection Services - a suite of web services that allow agencies to non-interactively submit transactions, either one-at-a-time or in batches, depending on the service used; retrieve the status of submitted batches, and submit queries that retrieve transaction information.
2. eBilling Online Web Service - provides a way for agencies to create ebills using a system-to-system interface
3. ACH Credit Web Service - provides a way for agencies to create ACH Credit transactions on behalf of their customers through a system-to-system interface.
4. Query Web Service - enables agency customers to non-interactively make requests for detailed information on one or more transactions. 

This Accelerator mainly touches upon the services provided within the suite of Trusted Collection Services (TCS).

![Non interactive web services diagram](/docs/non_interactive_service.png)


## Included Assets

This Accelerator includes the following assets:
1. Pay.gov API Implementation Template
    - MuleSoft Source code
    - A pre-made jar file a user can import into Anypoint Studio
    - Documentation about the various flows/connectors/configurations required to complete the template
2. Pay.gov Experience API specification 
    - RAML API specification representing a RESTful experience atop the Pay.gov web services
    - Documentation regarding the API specification

## Before You Install

**Access to Pay.gov Web Services** 
* To get access see: https://www.fiscal.treasury.gov/Pay.gov/getting-started.html
* As part of this process the requesting agency will be assigned an Agency Identifier as well as be provided the Pay.gov certificates needed for authentication. Pay.gov leverages mTLS for authentication, meaning clients need to have the Pay.gov TLS certificate installed in addition to having the correct client certificate that the Pay.gov server will validate. 

**License Requirements** 
* This solution was developed and tested on Anypoint Studio v7.15 and Mule Runtime v4.4.0; you should be running these or higher versions.

**Notes/Assumptions** 
* You are using this Accelerator in a sandbox or test environment. It is recommended that you not install any Accelerator directly into production environments.
* If you have not purchased MuleSoft, you may [try it for free](https://www.mulesoft.com/lp/dl/anypoint-mule-studio).
* Some experience working with Anypoint Studio, see a quick overview about studio and install instructions see [here](https://docs.mulesoft.com/studio/latest/)

## Installation

Please refer to the [implementation README](mulesoft/implementation/README.md) for detailed installation instructions.

To import the example application into MuleSoft AnyPoint Studio:
1) Download the paygov-experience-api-template.jar file (see the /mulesoft/implementation/target directory).
2) Open AnyPoint Studio and go to File → Import.
3) Select the Packaged Mule Application option from AnyPoint Studio category.
4) Navigate to the location where the jar file was downloaded.
5) Click Finish.


## Post-Install Setup & Configuration

Please refer to the [implementation README](mulesoft/implementation/README.md) for detailed installation instructions.


## FAQs


**_Q: How do I get support for accessing and configuring Pay.gov APIs and services?_**

A: Access, setup, troubleshooting and all support questions regarding the Pay.gov web services are addressed by the Pay.gov implementation team. To contact the Pay.gov implementation team please email Pay.gov@fiscal.treasury.gov

**_Q: Does this Accelerator provide me plug-and-play connectivity to Pay.gov?_**

A: The Bureau of the Fiscal Service does ***not*** publish the specification for the Pay.gov web services unless an agency has formally gone through an approval process. As such, this Accelerator _cannot_ provide exact field-level details about integrating with Pay.gov or exact details as to the XML payloads required to complete operations against the Pay.gov web services. What this Accelerator _does_ provide is a template that:
** Provides the scaffolding of a project that can integrate with the Pay.gov _TCS Plastic Card Web Service_ & _TCS Single Query Web Service_. 
** Pre-configures the authentication against the Pay.gov Web Services (see [implementation README for more details](mulesoft/implementation/README.md))
** Provides a documented & template-ized project that can be completed easily by filling in appropriate propeties files and defined object transformations

## Additional Resources

* [Configuration Properties](https://docs.mulesoft.com/mule-runtime/latest/mule-app-properties-to-configure)
* [Configure TLS with keystores and truststores](https://docs.mulesoft.com/mule-runtime/4.4/tls-configuration)
* [Pay.gov Web Services Documentation _Note: technical documentation is only accessible after requesting a copy by email_ ](https://qa.pay.gov/agencydocs/html/guides.html)


## Revision History

**v1.0 (31 Oct 2024)** - Initial release of the Accelerator.


## Acknowledgements

* Noor Ahmad, Principal Solutions Engineer
* Gianfranco Leto, Technical Consultant
* Raghu Vanavasam, Director, Technical Consulting

## Terms of Use

Thank you for using Global Public Sector (GPS) Accelerators.  Accelerators are provided by Salesforce.com, Inc., located at 1 Market Street, San Francisco, CA 94105, United States.

By using this site and these accelerators, you are agreeing to these terms. Please read them carefully.

Accelerators are not supported by Salesforce, they are supplied as-is, and are meant to be a starting point for your organization. Salesforce is not liable for the use of accelerators.

For more about the Accelerator program, visit: [https://gpsaccelerators.developer.salesforce.com/](https://gpsaccelerators.developer.salesforce.com/)
