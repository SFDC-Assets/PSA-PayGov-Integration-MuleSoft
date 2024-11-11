# Pay.gov Integration Template Documentation

## Background

For the background & context of this project please refer to [the following documentation](../../README.md)

## Project Overview

This project consists of APIKit enabled API which showcases how to connect to the _TCS Plastic Card Web Service_ & _TCS Single Query Web Service_. These web services can be used to accomplish a variety of use cases including:
* Submit various types of transactions: Sale, Authorization, Force, Refund & Cancel
* Ad hoc retrieval of details on any collections transaction in Pay.gov, regardless of the service used to create the transaction

In this project we aim to show how a user might leverage these services to *Authorize* a Plastic Card transaction, *Force* a Plastic Card transaction and finally *Query* a transaction's details. 

The *Authorize* and *Force* operations are one path to complete plastic card transactions with Pay.gov. This 2 step process first Holds (_Authorize_) and then Finalizes (_Force_) a payment. This use case might be relevant for example when an agency wants to hold funds until some process is complete (_Authorize_). Upon successful process completion the agency may actually charge the card (_Force_). This is opposed to the one step (_Sale_) operation which directly charges the plastic card without first placing an initial hold. 

## Project Structure
```
.
├── ...
├── src/main/mule                         # Home for MuleSoft configuration files
│   ├── global.xml                        # Configuration file where all connector configurations are defined
│   └── paygov-experience-api.xml         # "Main" project file with flows
├── src/main/resources/
│   ├── local.properties                  # Properties file that must get populated with appropriate values
│   └── api/
│       └── paygov-experience-api.raml    # API specification
```

## Pay.gov Authentication

The Pay.gov web services use mTLS authentication. This means a client must have (1) the certificate of the Pay.gov server in its truststore and (2) the private/public key pair in the keystore. In our context, since we are using the Web Services Consumer connector to connect to the Pay.gov web services we must configure an HTTPS transport which will dispatch the SOAP messages. In our project we have setup this HTTP transport by means of creating an HTTP Request Configuration (named PAYGOV_HTTPS_Config) that has the TLS Context pre configured. This HTTP Request Configuration is then selected as the transport mechanism in the Web Services Consumer configuration.

![Web Services Consumer Transport Config](/docs/wsc_transport.png)

*Web Services Consumer Transport Config*

![HTTPS Request Config](/docs/https_request_config.png)

*HTTP Request Configuration*

![TLS Context Config](/docs/tls_context_config.png)

*TLS Context Configuration*


## Property Files

The local.properties file defines variables throughout the project. These variables reference important values such as the location for the WSDL of the Pay.gov web services, the path to the truststore containing the Pay.gov certificate, etc. These variables and what values they should be populated with are defined below.

| Variable | Description |
| --- | --- |
| http.listener.host | The IP address the Mule server receive requests  |
| http.listener.port | The port on which the Mule server should receive requests |
| paygov.tcs.singlequery.wsdl | The location of the WSDL file for the Pay.gov Single Query Web Service |
| paygov.tcs.singlequery.service | The service name that needs to be called for a Single Query |
| paygov.tcs.singlequery.port | The port name that needs to be called for a Single Query |
| paygov.tcs.singlequery.operation | The specific operation name that references a Sinle Query |
| paygov.tcs.plasticcard.wsdl | The location of the WSDL file for the Pay.gov Plastic Card Web Service |
| paygov.tcs.plasticcard.service | The service name that needs to be called for a Plastic Card Web Transaction |
| paygov.tcs.plasticcard.port | The port name that needs to be called for a Plastic Card Web Transaction |
| paygov.tcs.plasticcard.auth.operation | The specific operation name that references a Plastic card Authorization |
| paygov.tcs.plasticcard.force.operation | The specific operation name that references a Plastic card Force |
| paygov.tls.truststorepath | The path to the truststore |
| paygov.tls.truststorepassword | The password for the truststore |
| paygov.tls.keystorepath | The path to the keystore |
| paygov.tls.privatekeypass | The password for the private key contained within the keystore |
| paygov.tls.keystorepass | The password to the keystore |

### Creating the truststore and keystore

In order to create the trust/keystores one must first get the relevant certifcates/keys from the Pay.gov specialist team. Once these are attained follow the steps listed [here](https://docs.mulesoft.com/http-connector/0.3.9/tls-configuration#generating-keystores-and-truststores) to appropriately generate the truststore and keystore.

## Flow Description

After importing this project into Anypoint Studio a user will see an [APIKit](https://docs.mulesoft.com/apikit/latest/) enabled MuleSoft project. This project is scaffolded off the API specification provided within this repository (see: [here](../specification)). The APIKit Router will route the requests to this endpoint to the appropriate [flow](https://docs.mulesoft.com/mule-runtime/latest/about-flows). This API consists of 3 endpoints:

** Initiate Payment (_Authorize_)
** Complete Payment (_Force_)
** Get Payment Details (_Query_)

![Project Flows](/docs/project_flows.png)


These 3 endpoints map to the following flows:

* Initiate Payment (_Authorize_) --> GetPaymentDetails
* Complete Payment (_Force_) --> PlasticCardPaymentAuthorization
* Get Payment Details (_Query_) --> PlasticCardPaymentForce

Let's take a look at one of the flows to understand how these flows operate, taking the GetPaymentDetails flow as an example:

![Get Payment Details Flow](/docs/get_payment_details_flow.png)

Here we see the following components:

* The first component is a Transform Message component. This Transform Message creates a variable called vars.agencyTrackingId. The value for this variable is passed in to the request as a URI parameter. This variable is used in the Query to search for a given transaction.
* The second component we see is another Transform Message component. As you can see below, the transformation required in this component needs to be completed. Once the properties file has been correctly configured and Web Services consumer (the next component) is able to reach the Pay.gov Single Query web service the schema required for the XML payload to Query will automatically be populated. _In the below screen show, the schema will automatically be populated where it says Binary Define metadata_. Once this metadata is loaded you will need to drag and drop from the input payload to the appropriate fields of the Single Query metadata. This process will generate the Dataweave required for the message transformation.
 ![Transform to Create Query XML](/docs/transform_for_query.png)
 * The third component is the Web Services Consumer. This is the component that actually reached out to the Pay.gov web service and, in this case, queries for a transaction. 
 * The final component is yet another Transform message component. Here we can transform, like we did in step 2, the _output_ of the Pay.gov Single Query web service to the data model defined by the Get Payment Details API response.


## Using this Accelerator

In order to use this accelerator:

1. Contact the Pay.gov team and get access to the Pay.gov web services technical documentation and certificates
2. Fill out the properties files with the appropriate values
3. Create the mapping in the flows to create the XML payloads required by the Pay.gov web services to invoke the assocaited operation
4. Test and run the application!

##