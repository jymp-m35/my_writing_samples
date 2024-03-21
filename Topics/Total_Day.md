# Total Day

A container may need to be stored in the yard before it can be loaded to a vessel or delivered to its consignee. A stored reefer container, especially when full, will need to be connected to electricity to remain at the required temperature. These services are charged to customers based on the number of days a container spent in storage or days a reefer container is connected to electricity.

In TOS+, the number of days charged for a storage or reefer electricity service is recorded as Total Day. Depending on your system configuration, Total Day is calculated on a daily or 24-hour basis. To know the Total Day configuration in your system, contact your system admin.

## Sample storage duration

For example, consider a container stored in the yard on January 15, 2023 at 08:00 and pulled out from the yard on January 16, 2023 at 07:00. In the system, the storage date and time are recorded as Start Date, whereas the pull-out date and time are recorded as End Date:

> Start Date: 15/01/2023 08:00

> End Date: 16/01/2023 07:00

## Total Day: daily basis calculation

If Total Day is calculated on a daily basis, the value is counted based on the calendar days. For the given example, Total Day is 2 days based on the following calculation:

> 15/01/2023 = 1 day

> 16/01/2023 = 1 day

> Total Day = 2 days

## Total Day: 24-hour basis calculation

If Total Day is calculated on a 24-hour basis, the value is counted based on 24-hour intervals. An interval of less than 24 hours is rounded up to 1 day. For the same given example, Total Day is 1 day based on the following calculation:

> 15/01/2023 08:00 to 16/01/2023 07:00 = 23 hours → 1 day

> Total Day = 1 day

If End Date is 16/01/2023 09:00 instead of 16/01/2023 07:00, Total Day is 2 days based on the following calculation:

> 15/01/2023 08:00 to 16/01/2023 08:00 = 24 hours → 1 day

> 16/01/2023 08:01 to 16/01/2023 09:00 = 1 hour → 1 day

> Total Day = 2 days

## Related concepts

A storage or reefer electricity service charge can be reduced by deducting Free Day or Holiday period from a container’s Total Day. For more information, see _Free Day_ or _Holiday_.
