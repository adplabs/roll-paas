# Using Roll PaaS APIs

## Introduction

Roll by ADP is a first of its kind mobile payroll solution specifically made for small business owners in mind. You can learn more about it [here](https://www.rollbyadp.com/why-roll). Roll platform as a service (Roll PaaS) is available to 3rd party partners who wish to implement innovative new solutions on top of Roll platform. 


Roll PaaS uses GraphQL to expose the features and functionality to our partners. The purpose of this document is to provide developers with a comprehensive understanding of the features and functionalities of the API, and to outline best practices for using it effectively.

In this document, we will provide an overview of Roll PaaS GraphQL, including its key features and benefits. We will also describe the API's functionalities and provide detailed information on how to access and use the API. Additionally, we will outline best practices for effectively using the API, including code samples for common use cases.


## Some background on Roll PaaS GraphQL API

### Why GraphQL

[GraphQL](https://graphql.org/) is at the heart of Roll. It is a modern and flexible data query and manipulation language that allows for efficient and powerful interactions with our APIs. Roll PaaS public GraphQL API is designed to offer partner developers access to a wide range of data and functionalities, making it an ideal choice for many different types of applications.

One of the main advantages of GraphQL over REST is its ability to return only the data that is requested, rather than returning a large and potentially overwhelming amount of data as is common with REST APIs. This results in faster and more efficient data transfers, as well as reduced network usage and better performance on mobile devices. GraphQL also allows for multiple queries to be executed in a single request, further reducing the number of round trips to the server.

Roll PaaS GraphQL provides two main capabilities for interacting with APIs: queries and mutations. Queries are used to retrieve data from the API, while mutations are used to modify data. Both queries and mutations are defined using a simple and expressive syntax, making it easy for developers to interact with the API. 

## Key Concepts

### Client

In Roll, a client is defined as an organization/business, and all of its legal entities and associates. A client may be using Roll to process their payroll and HR needs. The client entity encompasses all the legal entities and associates that are associated with a particular business. The data stored within the client entity is highly secure and confidential, and is accessible only to authorized users who have been granted permission to access it. In Roll PaaS GraphQL `client` graph provides a starting point for accessing client data.

For example, below query returns a list of associates within a specific client, along with their names and email addresses:

```graphql
query {
  client(clientID:"101e599f-7825-40a4-b81b-aa29b2999352"){
    clientName
    associates {
      legalName {
        formattedName        
      }
      personalEmailAddress
    }
  }
}
```

### Partner Client

In Roll, a Partner Client is a special type of client that has the ability to manage other clients, including manipulating their data and running payroll on their behalf. A Partner Client may have a development team assigned to using Roll's Platform as a Service (PaaS) GraphQL, which provides access to the Roll API and enables the development of custom integrations and applications.

The Partner Client is a unique and powerful entity in Roll, offering a high level of control and management over other clients. This enables Partner Clients to streamline operations, improve data accuracy, and enhance overall business efficiency. With the ability to manage multiple clients, including manipulating their data and running payroll, Partner Clients are well-equipped to handle the needs of small businesses and their associated entities.

The Partner Client also has the ability to onboard new clients by submitting a `companySetup` mutation. This added capability further enhances the Partner Client's role as a centralized hub for managing and controlling multiple small businesses and their associated entities.

The onboarding process for new clients involves creating a new client entity within Roll and setting up all necessary information, such as business details, legal entities, and associates. The Partner Client has full control over the onboarding process, and can customize the setup process to meet the specific needs of each new client.

By providing a streamlined and customizable onboarding process, Roll makes it easy for Partner Clients to expand their business and manage a growing number of small businesses and their associated entities.

The following query example lists out clients that are managed by a partner client:

```
query {
  client(clientID:"101e599f-7825-40a4-b81b-aa29b2999352"){
  	clientRelationships {
      relatedClient {
        clientName
      }
    }
  }
}
```

The following `companySetup` mutation will onboard a new client to be managed by a partner:

```
mutation submitCompany($input: CompanySetupInput!) {
  submitEvent(authorizationContext: {
    clientID: "101e599f-7825-40a4-b81b-aa29b2999352"
  }) {
    companySetup(input: $input) {
      headers {
        clientID
        eventID
        statusCode
        success
      }
      validationErrors {
        message
        vocabulary
      }
      clientID
    }
  }
}
```

Where `input` variable is as follows:

```
{
  "input": {
        "company": {
          "clientName": "My Business",
          "federalEmployerID": "426715616",
          "businessTypeCode": "LLC",
          "industryCode": "Professional Services",
          "locations": [
            {
              "addressStreetName": "1 ADP Blvd",
              "zipCode": "07068",
              "primaryLocationIndicator": true
            }
          ],
          "hasBalancesIndicator": false,
          "firstPartyContractorsPaidOnDemandIndicator": false,
          "billingDetails": {
            "billingRoutingNumber": "00000000",
            "billingAccountNumber": "00000000",
            "accountTypeCode": "Checking",
            "bankName": "TD Bank"
          }
        },
        "employees": [
          {
            "agreementCode": "Employee",
            "associateTypeCode": "Individual",
            "legalName": {
              "firstName": "Emp",
              "middleName": "L",
              "lastName": "Oyee"
            },
            "personalPhoneFormattedNumber": "+00000000000",
            "personalEmailAddress": "employee@etest.com",
            "governmentRegistrationCode": "SSN",
            "governmentRegistrationDocumentNumber": "000-00-0000",
            "homeAddress": {
              "addressStreetName": "1 ADP Blvd",
              "zipCode": "07068"
            },
            "genderCode": "Male",
            "jobDetails": {
              "jobName": "Software Engineer",
              "payRate": 100000,
              "payFrequencyCode": "Yearly"
            },
            "ownershipDetails": {
              "ownershipPercentage": 100,
              "primaryOwnerIndicator": true
            }
          }
        ],
        "payrollSchedules": [
          {
            "payrollScheduleTypeCode": "Weekly",
            "firstPaymentDate": "2021-01-01",
            "firstWorkPeriodStartDate": "2021-01-01",
            "firstWorkPeriodEndDate": "2021-01-01",
            "nonBusinessDayPaymentCode": "EarlierAvailableDate"
          }
        ]
      }
}
```


## Queries

### Client graph

TODO

### Data Protection

TODO

## Mutations

TODO

### Authorization context

TODO


## Accessing Roll PaaS GraphQL


### API Explorer Portal for developer access and documentation

TODO

### B2B Service Access

Roll PaaS GraphQL is accessible via ADP Marketplace. Developer documentation is available [here](https://developers.adp.com/). Specifically, the following are important topics:

* [Generating a Certificate Signing Request](https://developers.adp.com/articles/general/generate-a-certificate-signing-request),
* [Making Your First API Call Using Postman](https://developers.adp.com/articles/general/make-your-first-api-call-using-postman-1)

