

ATG and BuildingOS

Overview

With Lucid as a company of the Atrius Technology Group (ATG) we are looking for ways to integration ATG systems. ATG Systems include

Distech’s Eclypse version 1.0 and the upcoming 2.0, Atrius platform services, DGLogik’s Distributed Services Architecture (DSA) and

DGLogik’s DGLux for DSA visualizations. Each of these systems and their components has specific areas of concern within the ATG

portfolio.

**Integrating with Distech Products**

Distech products are responsible for edge and on-premise communications with Building Management Systems (BMS) and the Acuity nLight

products. Eclypse is the primary product offering which is a typical firmware based controller for the edge.

nLight products are Bluetooth enabled lighting and control systems which integrate with an Eclypse controller. The nLight sensors enable

smart lighting technologies including occupancy and asset tracking.

Distech also has a product for configuring and deploying an Eclypse controller, called Envysion. Envysion is a graphical tool that allows a

System Integrator (SI) to create and deploy the BMS and lighting configuration for an Eclypse based system.

Presently it is possible to export site/building/point data from an Eclypse system to the Atrius cloud using MQTT and routing to the Azure

MQTT instance and leveraging IOTHub services.

Eclypse also supports a REST API which allows for discovery of the BACNet network and the ability to execute scheduling and other control

calls. Within the discovery of the BACNet network, you can extract values and timestamps. There are also API calls to adjust controls (set

points) and schedules. Envysion provides the same functionality from the perspective of a supervisor.

In 2020 Eclypse 2.0 will be released. This version of the Eclypse controller will leverage DSA at its core for network discovery, scheduling,

controls and alerting.

Envysion is a graphical interface for Eclypse that is run as a supervisor for the Eclypse controller. This provides a "fog" level view of an

organizations BMS sensors and controls with a richer set of analytic functions. This comes with the benefit of classic fog computing where

the system is contained within the larger network but not exposed to "the internet". Both Eclypse and Envision are single tenant systems.

Envision is a software solution that is run on a hardware configuration that is defined by the needs of the customers. The general deployment

of an Eclypse controller is specific to a building (the edge). It can run basic analytics without sacrificing performance. Eclypse controllers run

Niagra at the core with additional functionality provided in support of controls, scheduling and management tools provided Distech and

Eclypse. This means you can add traditional "a la carte" Niagra options for your Eclypse controller. Envision when deployed as a fog system

provides longer data storage and more sophisticated analytics. I have not seen any benchmarks for Envision or Eclypse. I am presently

looking for some official documentation which would shed light on capacity constraints for both Eclypse and Envision.

In 2020 Envision 3.0 will be released. At this point, there is a discussion between Distech and Lucid to define where the fog ends and the

cloud begins. As well Distech is looking to develop an integration with BuildingOS. This is still in the discussion phase as Distech would

prefer an MQTT interface to leverage existing code. This presents complications as BuildingOS does not have any intention of supporting

"Pub/Sub" protocols for the internet of things. Atrius (see below) on the other hand does. Messaging between Eclypse and the Atrius Cloud

platform is brokered through the Azure IoTHub.

Eclypse and Envysion both support OpenId Connect as an interface for Single Sign-On (SSO) with for a given organization. This, of course,

runs on the presumption that you have an Identity Provider which supports OpenID Connect, and this needs to be deployed within network

reach of the components, as well as the administration of the users within the system.

Eclypse is a firmware-based deployment and as such comes with the expected release cycles. Envysion is based on DGLux and is

dependent on the support, management, and distribution of DGLux to a given customers site.

**The model below depicts the deployment of Envysion and Eclypse today. These would be inside of the same network and would be**

**isolated from external systems.**

**Integrating with Atrius Cloud**

Atrius expands on the Distech offering to create a cloud-based, Azure deployment sensory network for customers. Using Atrius ready

luminaires (nLight enabled) Atrius creates indoor positioning, asset tracking, and management, contextual spatial analytics, and space

utilization applications.

nLight based systems are configured utilizing a Mobile application. The mobile application can report site, building, asset, and point





information. This configuration data and configuration data can be managed in the Atrius cloud.

**Below is a marketing slide of an Atrius deployment**

The Atrius cloud has integrated with Eclypse controllers. This integration utilizes the Message Queuing Telemetry Transport (MQTT) protocol

in conjunction with the Azure Message Queuing Service (the Azure answer for Kafka) to manage messages of many types. One of those

types is the configuration data for sites, buildings, floors, spaces, and points sent from Eclypse controllers. Presently there are no messages

related to consumption, demand, pulse or actuator data being pushed through this pipe. At present this is a unidirectional pipe. Plans are

being made to enable bi-directional communications.

Atrius is very popular within the retail sector. This is significant simply because of the scale of deployment. Presently Atrius utilizes DGLux as

a graphical tool to develop the interface to communicate with the Atrius backend API. The API is segmented into logical components which

can be expanded to include the ability to auto-generate the BuildingOS domain model using the BuildingOS API's.

While Atrius supports multiple tenants, it is presently not designed to support constructs around licensing, user profiles and access control.

Atrius does support Open ID Connect and SAML2 for SSO. It supports oAuth2 as well. The authentication mechanism is on a per "account"

(organization) basis. Keys and realms are set up to allow for key based identification between systems. Each user is granted access to

system resources based on the keyset defined when authenticating.

From a pure systems perspective, the most interesting aspect of the Atrius cloud is the MQTT support for Distech products and the API in

front of the Atrius Spaces database. This creates an agnostic integration into BuildingOS for Distect and Atrius products that would route

through the Atrius cloud and then make calls to BuildingOS.

**Below is the architecture of the Atrius Cloud MVP**

**Integrating with DGLogik Products**

DGLogik has two products that layer on top of each other. The first is DSA. DSA is an opensource software-based solution that speaks

multiple protocols and enables dynamic data routing (called data-flow) capabilities.

DGLux is a graphical interface that can be utilized for configuring site information, permissions and custom real-time trending and graphing.

The ability to export site, building, point configuration and readings from DSA or DGLux does not presently exist. We are presently

experimenting with a DSALink which will replicate the configuration data from the site, building, floor, space and point data. This same

technology enables the replication of reading data from DSA to the BuildingOS API. This approach has been proven out in a

Proof-of-Concept (POC) but has not been productized.

DGLux has DSA embedded within the deployment. DGLux runs as a single Java virtual machine which is designed for a single tenant.

DGLux has sophisticated network capabilities which enable it to be run on-premise, at the edge or in the fog. Each deployment of DGLux is

referred to as a “project” which includes the configuration of the graphical tools. The act of creating a project within DGLux is referred to as

“zero-code” programming with a graphical interface (DGLux). The artifacts of the “project” are easily exported and imported from one DGLux

instance to another. This process is manual and is not presently automated.

DSA can be configured to speak to a "supervisor" node of DSA. This creates a hierarchical configuration of nodes that always ends with "one

node" at the top. From an edge perspective, this is acceptable. From a cloud perspective, this creates a single point of failure. When

configuring DSA connections you have to be conscious that it uses either a raw TCP connection or will initialize a TCP session using a

WebSocket. Either way, this is a persistent connection. Just as with the Distech configuration above a DSA/DGLux does well on the edge.

Cisco has been working with DSA instances and have solutions to resolve these issues.

BuildngOS Integrations

Integrating these ATG systems needs to be done with care. While inviting to ensure tight integration with other systems, it would be

detrimental to Lucid’s original business model as a ubiquitous SaaS provider for the built environment. All integrations must include an

abstraction layer that enables similar integrations from other vendors who wish to collaborate.

BuildingOS will continue to support its current partnerships and will extend the platform in a vendor-neutral way on all aspects. With this in

mind, the public interface of BuildingOS is where we will extend functionality. We will not do this through a tight coupling. This will be done

through open standards and API’s.

**Below is a high-level model of how the public interface to BuildingOS is exposed**





