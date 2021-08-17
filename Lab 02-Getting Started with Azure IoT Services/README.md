# [Getting Started with Azure IoT Services](https://microsoftlearning.github.io/AZ-220-Microsoft-Azure-IoT-Developer/Instructions/Labs/LAB_AK_02-getting-started-with-azure-iot-services.html)
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
        


- The value that you apply to IoT hub name must be unique across all of Azure. This is true because the value assigned to the name will be used in the IoT Hub’s connection string. Since Azure enables you to connect devices from anywhere in the world to your hub, it makes sense that all Azure IoT hubs must be accessible from the Internet using the connection string and that connection strings must therefore be unique.
- IoT Hub is a managed service, hosted in the cloud, that acts as a central message hub for bi-directional communication between your Azure IoT services and your connected devices.
- endpoint is anything connected to or communicating with your IoT Hub.
- The Azure IoT Hub Device Provisioning Service is a helper service for IoT Hub that enables zero-touch, just-in-time provisioning to the right IoT hub without requiring human intervention.
    - *i need to know more about this service and how to use it*
    - The IoT Hub Device Provisioning Service is a helper service for IoT Hub that enables zero-touch, just-in-time provisioning to the right IoT hub without requiring human intervention, *enabling customers to provision millions of devices in a secure and scalable manner*.
    - The Device Provisioning Service can only provision devices to IoT hubs that have been linked to it. Linking an IoT hub to an instance of the Device Provisioning service gives the service read/write permissions to the IoT hub’s device registry; with the link, a Device Provisioning service can register a device ID and set the initial configuration in the device twin.
    - Enrollment groups can be used for a large number of devices that share a desired initial configuration, or for devices all going to the same tenant.
- 

