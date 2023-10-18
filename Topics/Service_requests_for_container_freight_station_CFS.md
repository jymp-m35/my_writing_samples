# Service requests for container freight station (CFS) operations

The Service Request feature allows you to manage the stuffing and stripping of cargo, also known as container freight station (CFS) operations, at your port terminal. The available request types are as follows:

- Cargo receive request - Used for direct stuffing
- Stuffing request - Used for normal stuffing
- Stripping request - Used for direct and normal stripping

The following diagram shows the operational use-case flow for each CFS request type.

![alt text](/Images/Service_request_flow_CFS.png)

## Cargo receive request for direct stuffing

You can use a cargo receive request to manage the direct stuffing of cargo from truck to container.

In this operation, you create a cargo receive request and set the purpose to Stuffing. Then, you declare the cargo, the associated container (with Empty shipment status), and the truck carrier in the request. After you specify these details, you can dispatch the request to start planning the cargo receive job.

To know how to create a cargo receive request for direct stuffing operations, see _Create a cargo receive request for direct stuffing_.

> A cargo receive request for direct stuffing applies only to cargo with an Import (IM) or Inbound (IB) shipping status. For more information, see _Cargo lot shipping status_.

## Stuffing request

Use a stuffing request to manage the normal stuffing of cargo from a CFS facility to a container. 

In this operation, you create a stuffing request and then nominate the cargo and associated container (with Empty shipment status) to the request. After you specify these details, you can dispatch the request to start planning the stuffing job.

To know how to create a stuffing request for normal stuffing operations, see [Create a stuffing request](/Topics/Create_a_stuffing_request.md).

> Before you manage the stuffing of cargo, make sure the receiving of the cargo from a truck carrier to the CFS facility has been managed using a cargo receive request for vessel loading. To know more, see _Cargo receive request for vessel loading_.

> A stuffing request applies only to cargo with an Import (IM) or Inbound (IB) shipping status. For more information, see _Cargo lot shipping status_.

## Stripping request

Use a stripping request to manage the stripping of cargo from a container to a CFS facility (normal stripping) or truck carrier (direct stripping).

> A stripping request applies only to cargo with an Export (EX) or Outbound (OB) shipping status. For more information, see _Cargo lot shipping status_.

### Normal stripping

Use a stripping request to manage the normal stripping of cargo from a container to a CFS facility.

In this operation, you create a stripping request and nominate the container containing the cargo. Then, you set the target location to Warehouse, indicating that the cargo will be stripped from the container and unloaded to a CFS facility. Lastly, you set the shipment status to either full container load (FCL) or less container load (LCL), depending on the bills of lading associated to the container. After you specify these details, you can dispatch the request to start planning the stripping job.
    
To know how to create a stripping request for normal stripping operations, see _Create a stripping request_.

> After you manage the stripping of a container, you can manage the delivery of the stripped cargo by a truck carrier using a cargo delivery request. To know more, see _Cargo delivery request_.
    
### Direct stripping

You can also use a stripping request to manage the direct stripping of cargo from a container to a truck carrier.
    
In this operation, you create a stripping request and nominate the container containing the cargo. Then, you set the target location to _Truck_, indicating that the cargo will be stripped from the container and loaded to a truck carrier. Lastly, you set the shipment status to either full container load (FCL) or less container load (LCL), depending on the bills of lading associated to the container. After you specify these details, you can dispatch the request to start planning the stripping job.
    
To know how to create a stripping request for direct stripping operations, see _Create a stripping request_.
