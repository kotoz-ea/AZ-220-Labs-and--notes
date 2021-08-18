# [Getting Started with Azure IoT Services](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_02-getting-started-with-azure-iot-services.html)

## TODO
- [X] Azure IOT Hub Concepts.
    - [X] Devices and Device Communication
    - [X] Device Configuration and Communication
- [ ] Azure IOT Edge  
    - [ ] Azure IoT Edge Deployment Process

- ## IOTHub Endpoints
    - Azure IoT Hub is a multi-tenant service that provides access to its functionality using a combination of built-in and custom endpoints.
    - **Builtin Endpoints**
        - Resource provider: 
            - This interface enables Azure subscription owners to create and delete IoT hubs, and to update IoT hub properties.
            - IoT Hub properties govern hub-level security policies, as opposed to device-level access control, and functional options for cloud-to-device and device-to-cloud messaging.
            - The IoT Hub resource provider also enables you to export *device identities*
                - What is the [device identity](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry)?
                    -  Device identities are used for device authentication and access control
                    - Every IoT hub has an identity registry that stores information about the devices and modules permitted to connect to the IoT hub.
                    - Before a device or module can connect to an IoT hub, there must be an entry for that device or module in the IoT hub's identity registry. A device or module must also authenticate with the IoT hub based on credentials stored in the identity registry.
                    - The identity registry is a REST-capable collection of device or module identity resources.
                    - When you add an entry in the identity registry, IoT Hub creates a set of per-device resources such as the queue that contains in-flight cloud-to-device messages.
                    - Use the identity registry when you need to:
                        - Provision devices or modules that connect to your IoT hub.
                        - Control per-device/per-module access to your hub's device or module-facing endpoints.
                    - The IoT Hub identity registry exposes the following operations:
                        - Create device or module identity
                        - Update device or module identity
                        - Retrieve device or module identity by ID
                        - Delete device or module identity
                        - List up to 1000 identities
                        - Export device identities to Azure blob storage
                        - Import device identities from Azure blob storage

        - Device identity management:
            - Each IoT hub exposes a set of [HTTPS REST](https://www.youtube.com/watch?v=lsMQRaeKNDk) endpoints to manage device identities `CRUD`(create, retrieve, update, and delete).
            - Device identities are used for device authentication and access control
        - Device twin management:
            - Each IoT hub exposes a set of service-facing HTTPS REST endpoints to query and update device twins (update tags and properties).
        - Jobs management:
            - Each IoT hub exposes a set of service-facing HTTPS REST endpoint to query and manage jobs.
        -  Device endpoints
            - For each device in the identity registry, IoT Hub exposes a set of endpoints:
                - Send device-to-cloud messages. A device uses this endpoint to send device-to-cloud messages.
                - Receive cloud-to-device messages. A device uses this endpoint to receive targeted cloud-to-device messages.
                - Initiate file uploads. A device uses this endpoint to receive an Azure Storage SAS URI from IoT Hub to upload a file.
                - Retrieve and update device twin properties. A device uses this endpoint to access its device twin's properties.
                - Receive direct method requests. A device uses this endpoint to listen for direct method's requests.
                    - These endpoints are exposed using MQTT v3.1.1, HTTPS 1.1, and AMQP 1.0 protocols. AMQP is also available over WebSockets on port 443.
        - Service endpoints. Each IoT hub exposes a set of endpoints for your solution back end to communicate with your devices. With one exception, these endpoints are only exposed using the AMQP protocol. The method invocation endpoint is exposed over the HTTPS protocol.
            - Receive device-to-cloud messages. This endpoint is compatible with Azure Event Hubs. A back-end  service can use it to read the device-to-cloud messages sent by your devices. You can create custom endpoints on your IoT hub in addition to this built-in endpoint.
            - Send cloud-to-device messages and receive delivery acknowledgments. These endpoints enable  your solution back end to send reliable cloud-to-device messages, and to receive the corresponding delivery or expiration acknowledgments.
            - Receive file notifications. This messaging endpoint allows you to receive notifications of when your  devices successfully upload a file.
            - Direct method invocation. This endpoint allows a back-end service to invoke a direct method on a device.
            - Receive operations monitoring events. This endpoint allows you to receive operations monitoring events if your IoT hub has been configured to emit them. For more information, see IoT Hub operations monitoring.
        - Custom Endpoints
            - You can link existing Azure services in your subscription to your IoT hub to act as endpoints for message  routing. These endpoints act as service endpoints and are used as sinks for message routes. Devices cannot write directly to the additional endpoints.
            - IoT Hub currently supports the following Azure services as additional endpoints:
                - Azure Storage containers
                - Event Hubs
                - Service Bus Queues
                - Service Bus Topics

- ## Access control and permissions
    - IoT Hub uses permissions to grant access to each IoT hub endpoint. Permissions limit the access to an IoT hub based on functionality.
    
- ## Device Identity and Registration
    - Every IoT hub has an identity registry that stores information about the devices permitted to connect to the IoT hub.
    - Before a device can connect to an IoT hub, there must be an entry for that device in the IoT hub's identity registry.
    - A device must also authenticate with the IoT hub based on credentials stored in the identity registry.

- ### Module Identity
    - In IoT Hub, **under each device identity, you can create up to 20 module identities**.
    - Each module identity implicitly generates a module twin, module twins are JSON documents that store module state information including metadata, configurations, and conditions.
    - On the device side, the IoT Hub device SDKs enable you to create modules where each one opens an independent connection to IoT Hub.
    - This functionality enables you to use separate namespaces for different components on your device. For example, you have a vending machine that has three different sensors. Each sensor is controlled by different departments in your company. 
    - You can create a module for each sensor. This way, each department is only able to send jobs or direct methods to the sensor that they control, avoiding conflicts and user errors
- ## Intro to Device twins
    - Device twins are JSON documents that store device state information including metadata, configurations, and conditions.  
    - Device twins enable synchronizing desired configuration from the cloud and for reporting current configuration and device properties. 
    - The lifecycle of a device twin is linked to the corresponding device identity. Device twins are implicitly created and deleted when a device identity is created or deleted in IoT Hub.
    - Device twins store device-related information that:
        - Device and back ends can use to synchronize device conditions and configuration.
        - The solution back end can use to query and target long-running operations.
    - A device twin is a JSON document that includes
        - **Tags**. A section of the JSON document that the solution back end can read from and write to. Tags are not visible to device apps.
        - Desired properties. Used along with reported properties to synchronize device configuration or conditions. 
        - The solution back end can set desired properties, and the device app can read them. The device app can also receive notifications of changes in the desired properties.
        -  The device app can set reported properties, and the solution back end can read and query them
        - ### Use device twins to:
            - Store device-specific metadata in the cloud. For example, the deployment location of a vending machine.
            - Report current state information such as available capabilities and conditions from your device app. For example, a device is connected to your IoT hub over cellular or WiFi.
            - Synchronize the state of long-running workflows between device app and back-end app. For example, when the solution back end specifies the new firmware version to install, and the device app reports the various stages of the update process.
            - Query your device metadata, configuration, or state.
- ## Azure IoT SDKs
    - **IoT Hub Device SDKs** enable you to build apps that run on your IoT devices using device client or module client.
    - These apps send telemetry to your IoT hub, and optionally receive messages, job, method, or twin updates from your IoT hub.
    - **IoT Hub Service SDKs** enable you to build backend applications to manage your IoT hub, and optionally send messages, schedule jobs, invoke direct methods, or send desired property updates to your IoT devices or modules.

- ## Device Configuration and Communication
    - IoT Hub allows for bi-directional communication with your devices.
    - Use IoT Hub messaging to communicate with your devices by sending messages from your devices to your solutions back end and sending commands from your IoT solutions back end to your devices.
    - **Device-to-Cloud Communications**
        When sending information from the device app to the solution back end, IoT Hub exposes three options:
        - Each Device-to-cloud messages for time series telemetry and alerts.
        - Each Device twin's reported properties for reporting device state information such as available capabilities, conditions, or the state of long-running workflows. For example, configuration and software updates.
        - Each File uploads for media files and large telemetry batches uploaded by intermittently connected devices or compressed to save bandwidth.
    - **Cloud-to-Device Communications**
        IoT Hub provides three options for communicating to a device from a back-end app:
        - Direct methods for communications that require immediate confirmation of the result. Direct methods are often used for interactive control of devices such as turning on a fan.
        - Twin's desired properties for long-running commands intended to put the device into a certain desired state. For example, set the telemetry send interval to 30 minutes.
        - Cloud-to-device messages for one-way notifications to the device app    


- The value that you apply to IoT hub name must be unique across all of Azure. This is true because the value assigned to the name will be used in the IoT Hub’s connection string. Since Azure enables you to connect devices from anywhere in the world to your hub, it makes sense that all Azure IoT hubs must be accessible from the Internet using the connection string and that connection strings must therefore be unique.
- IoT Hub is a managed service, hosted in the cloud, that acts as a central message hub for bi-directional communication between your Azure IoT services and your connected devices.
- endpoint is anything connected to or communicating with your IoT Hub.
- The Azure IoT Hub Device Provisioning Service is a helper service for IoT Hub that enables zero-touch, just-in-time provisioning to the right IoT hub without requiring human intervention.
    - *i need to know more about this service and how to use it*
    - The IoT Hub Device Provisioning Service is a helper service for IoT Hub that enables zero-touch, just-in-time provisioning to the right IoT hub without requiring human intervention, *enabling customers to provision millions of devices in a secure and scalable manner*.
    - The Device Provisioning Service can only provision devices to IoT hubs that have been linked to it. Linking an IoT hub to an instance of the Device Provisioning service gives the service read/write permissions to the IoT hub’s device registry; with the link, a Device Provisioning service can register a device ID and set the initial configuration in the device twin.
    - Enrollment groups can be used for a large number of devices that share a desired initial configuration, or for devices all going to the same tenant.



## Azure IoT Edge Deployment Process
### What is azure iot edge?
Azure IoT Edge moves cloud analytics and custom business logic to devices so that your organization can focus on business insights instead of data management.
## Azure IoT Edge is made up of three components:
- IoT Edge modules are containers that run Azure services, third-party services, or your own code. Modules are deployed to IoT Edge devices and execute locally on those devices.
    - IoT Edge modules are units of execution, implemented as Docker compatible containers, that run your business logic at the edge.
    - Multiple modules can be configured to communicate with each other, creating a pipeline of data processing.
    - Azure IoT Edge allows you to deploy complex event processing, machine learning, image recognition, and other high value AI without writing it in-house. Azure services like Azure Functions, Azure Stream Analytics, and Azure Machine Learning can all be run on-premises via Azure IoT Edge.
- The IoT Edge runtime runs on each IoT Edge device and manages the modules deployed to each device.
    - The runtime sits on the IoT Edge device, and performs management and communication operations.
- A cloud-based interface enables you to remotely monitor and manage IoT Edge devices.

    - ## IOT Edge Hub
        - The IoT Edge hub acts as a local proxy for IoT Hub by exposing the same protocol endpoints as IoT Hub.
        - This means that clients (whether devices or modules) can connect to the IoT Edge runtime just as they would to IoT Hub.
        - **IoT Edge hub supports clients that connect using MQTT or AMQP. It does not support clients that use HTTP**.
        - **The IoT Edge hub is not a full version of IoT Hub running locally**
        - 

## Lab six:Introduction to Azure IoT Edge
In this lab, you will complete the following activities:

- [X] Verify that the lab prerequisites are met (that you have the required Azure resources)
- [X] Deploy an Azure IoT Edge Enabled Linux VM
- [X] Create an IoT Edge Device Identity in IoT Hub using Azure CLI
- [ ] Connect the IoT Edge Device to IoT Hub
- [ ] Add an Edge Module to the Edge Device
- [ ] Deploy Azure Stream Analytics as an IoT Edge Module

        


