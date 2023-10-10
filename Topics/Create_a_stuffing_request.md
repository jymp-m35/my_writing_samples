# Create a stuffing request

Create a stuffing request to pre-advise and manage the stuffing of cargo to a container. For more information, see [Service requests for container freight station (CFS) operations](/Service_requests_for_container_freight_station_CFS.md).

When you create a stuffing request, you need to enter the following details:

* Request details, such as the customer name, line operator, and invoice option.
* Container details, such as the container number and quantity. For multiple containers, you can enter the liner booking details.
* Cargo details, such as the cargo description, commodity information, package type, and quantity.
* Additional services required during stuffing, such as palletizing of cargo.

> Currently, stuffing request only supports breakbulk cargo. Additional cargo types will be supported in future software updates.

## Prerequisites

Before you create a stuffing request, make sure of the following:
* The assigned cargo is received at the CFS facility using a cargo receive request. For more information, see _Cargo receive management_.
* The assigned container is nominated to a liner booking. For more information, see _Liner booking management_.

## Procedure

To create a stuffing request:

1. Go to **GC/CFS > Request**.
1. Click **Create Request**.
1. Select **Stuffing** as the request type.
1. Select **Export** as the request purpose.
1. Enter the key request details in the following fields. Then, click **Next**.

   > Before you click **Next**, make sure to enter the correct key details. Once you get to the next page, you can no longer return to make corrections.

   | Field | Description |
   | :--- | :--- |
   | **Customer** | Account code of the customer who requested the stuffing operation. |
   | **Receive By / Nbr** | Container or liner booking assigned to the stuffing request. If you want to assign only one container, enter a container number. If you want to assign multiple containers, enter a booking number. |
   | **Booking Operator** | Operator code of the line operator declared in the liner booking. |
   | **Vessel Visit** | Visit code of the vessel visit assigned to the liner booking. This field only appears when you assign a liner booking to the stuffing request.|

1. Enter additional request details in the following fields:

   | Field | Description |
   | :--- | :--- |
   | **Invoice Option** | Payment method used to settle the billing invoice for the stuffing request. |
   | **CHE Option** | Type of cargo handling equipment (CHE) used for the stuffing operation. |
   | **Request Date** | Date when the customer requested the stuffing request. By default, the request date is set to the current date. |
   | **Deadline** | Date when the customer wanted to complete the stuffing request. By default, the deadline is set to the current date.|
   | **Contact Person** | Name of the person to contact on behalf of the customer. |
   | **Contact Number** | Contact number of the contact person. |
   | **Email** | Email address of the contact person. |
   | **Remarks** | Any additional or relevant details about the stuffing request.|

1. In the **Container Information** section, enter the quantity of the assigned container in the **Request Count** column.
1. Fetch the details of the assigned cargo:

   a. In the **Cargo Pickup Details** section, click **Action > Get Cargo Detail**.

   b. Click **Search By**, and then select **Generic Description**.

   c. Enter the cargo description, and then click **Confirm**.

   d. Enter the assigned cargo quantity in the **Stuffing Quantity** column.

1. (Optional) In the **Additional Services** section, enter one or more additional services required to complete the stuffing operation.
   > You can enter an additional service after you save the stuffing request. For more information, see _Enter an additional service to a service request_.
1. Click **Save**.

The **Service Request** view page appears, showing the created stuffing request and the details you save. The stuffing request is assigned a unique service request (SSR) number and set with the default request status Created.

## Related tasks

After you create a stuffing request, you can perform the following operations:

* Dispatch the stuffing request and initiate the stuffing operation. For more information, see _Dispatch a service request_. 
* Cancel the stuffing request. A canceled stuffing request can no longer be used in any stuffing transactions. For more information, see _Cancel a service request_.









































































