## Workday web services

### WSDL: Web Service Description Language

- Documentation is also published and provided.
- XML-based language for describing and accessing web services
- Machine readable description
    - How a web service is called
    - What parameters it expects
    - What data structure it returns 

### Workday Web Services

- Forms the infrastructure for all workday tasks and integrations
- Makes all interactions with workday objects possible

### Workday integration tools

#### Workday studio

- Powerful development tool
- Enables technical users to build customizable integrations to and from workday

#### EIB: Enterprize Interface Builder

- Easy-to-use, graphical, guided interface.
- Use to defined inbound and outbound integrations without requiring programming.
- Limited to only one input, one transformation, and one output.

#### Document transformation

- More powerful than EIB
- Doesn't interact with the workday tenant or use any web service
- Transforms a connector's output XML document into a different final output using XSLT instructions.

#### Workday connectors

- Templates that extend workday functionality to common external endpoints.
- consists of two types of integrations:
    1. End-to-end connectors
        - Out-of-the-box solutions delivered as part of workday product offerings
        - Fully supported by workday
        - Requires less work than custom integrations
    2. Core connectors  
        - Prebuilt integrations that support specific usage scenarios and functional areas.
        - Import and export data in a workday-defined file format.

### Types of workday services

Public Web services API Directory - Latest version of the workday documentation for web services.

Inbound:
- ADD
- PUT
- UPDATE

Outbound:
- GET
- FIND

Custom api - Using workday report writer.
- Control the data source and fields that appear in the output.
- check the "enable as web service" on the report settings.
