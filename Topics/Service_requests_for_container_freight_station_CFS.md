## Service requests for container freight station (CFS) operations

Service requests for container freight station (CFS) operations are used to manage the following terminal services:

-   Stuffing of cargoes to containers
-   Stripping of cargoes from containers

The following diagram illustrates each request type for CFS operations and their supported terminal operations.

![alt text](/Images/Service_request_flow_CFS.png)

### Cargo receive request for direct stuffing

A cargo receive request is used to manage direct stuffing operations of cargoes from truck to container.

In this operation, a cargo receive request is created and set with a purpose of stuffing. The cargoes for stuffing are declared in the service request, along with the details of the associated container and truck.

When the service request is dispatched, the corresponding cargo receive job is planned to manage the direct stuffing of cargoes from the truck to the associated container.

To know how to create a cargo receive request for direct stuffing operations, see _Create a cargo receive request for direct stuffing_.

### Stuffing request

A stuffing request is used to manage the normal stuffing operations in which cargoes are unloaded from truck to CFS facility, and then to a container. This operation consists of two major processes:

-   Truck unloading: The cargoes for stuffing are received from a truck and then unloaded to the CFS facility. This process is managed using a cargo receive request (purpose is Export) for normal vessel loading. To know more, see Cargo receive request.
-   Stuffing: The received cargoes are stuffed from the CFS facility to a container. This process is managed using a stuffing request.

For the stuffing process, a stuffing request is created. The cargoes for stuffing are nominated in the service request, along with the details of the associated container.

When the service request is dispatched, the corresponding stuffing job is planned to manage the stuffing of cargoes from the CFS facility to the associated container.

To know how to create a stuffing request for normal stuffing operations, see [Create a stuffing request](/Topics/Create_a_stuffing_request.md).

### Stripping request

A stripping request is used to manage the stripping of cargoes from containers. This service request supports the following stripping operations:

-   Normal stripping
    
    In normal stripping operations, cargoes are stripped from a container to the CFS facility and then loaded to a truck. This operation consists of two major processes:
    
    1.  Stripping: The cargoes are stripped from a container and then moved to the CFS facility. This process is managed using a stripping request.
    2.  Delivery: The stripped cargoes are loaded from the CFS facility to a truck for delivery. This process is managed using a cargo delivery request for normal delivery. To know more, see _Cargo delivery request_.
    
    For the stripping process, a stripping request is created and nominated with the container that consists of the cargoes for stripping. The shipment status of the container is either full container load (FCL) or less container load (LCL). The warehouse is set as the target location at which the cargoes will be stripped.
    
    When the service request is dispatched, the corresponding stripping job is planned to manage the stripping of cargoes from the container to the designated location in the CFS facility.
    
    To know how to create a stripping request for normal stripping operations, see _Create a stripping request_.
    
-   Direct stripping
    
    In direct stripping operations, cargoes are stripped directly from the container to truck. For this operation, a stripping request is created and nominated with the container that consists of the cargoes for stripping. The shipment status of the container is either full container load (FCL) or less container load (LCL). The truck is set as the target location at which the cargoes will be stripped.
    
    When the service request is dispatched, the corresponding stripping job is planned to manage the stripping of cargoes from the container to the designated truck.
    
    To know how to create a stripping request for direct stripping operations, see _Create a stripping request_.
    
-   Partial stripping
    
    In partial stripping operations, a container is stripped in portions under separate stripping jobs. Scenarios that require separate stripping jobs for one container includes the following:
    
    -   Container is LCL, wherein the commodities belong to different B/L documents.
    -   Container commodity will be delivered in batches based on availability of truck carriers.
    -   Container commodity need to undergo customs inspection before delivery.
    -   Container commodity is overlanded, wherein the stripped quantity exceeds the declared quantity.
    
    For this type of operation, separate stripping requests are created and dispatched for each stripping job. As each stripping job proceeds, new seal numbers are attached to the container until the last stripping job. When the last stripping job is completed, the container is tagged as empty to end the stripping operation for the container.
    
    Partial stripping can be performed for both normal stripping and direct stripping operations.