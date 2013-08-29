# Inventory API

The Inventory API provides the following web service collections.

| Collection | Scope of use within M-Local |
|----|----|
| Receiving Orders | Event stream of stock orders going into a 3rd party DC from either a Morrisons DC or Supplier. |
| Transfers | Event stream of stock transfers from an M-Local DC to a store |
| Shipments | -- Send notification of shipments -- Event stream of shipments notifications (deferred) |
| Receipts | Send a receipt to Morrisons |
| Adjustments | Send an inventory adjustment to Morrisons |

## Receiving Orders

### Request
`GET https://{DNS}/inventory/v1/receivingorders/@all/events&{parameters}`

##### Applicable parameters
|Parameter|Description|Example|
|---|---|---|---|
| location | Filters the response to only include data for a specific location or list of locations | location=1234 | Mandatory |
| format | Specifies the format of the data to be returned in the HTTP body. Valid values are XML or JSON. If not specified JSON will be returned. | format=JSON format=XML | Optional |
| limit	| An integer value specifying the maximum number of items the API should return in response to this request. Where no limit is specified, the entire item set will be returned. Please note that the data set may be many megabytes in size. | limit=10 | Optional |
| eventid[gt] | Only receiving orders with an event ID number higher than specified in this parameter will be returned. The value specified here is typically that of the last ‘event id’ value returned in the previous request. | eventid[gt]=12443 | Mandatory |

The receiving orders collection should be polled periodically for new change events, using eventid[gt] to avoid downloading events that have already been processed.
Therefore the last event id returned in a response should be used as the eventid[gt] parameter value for the next request.

### Response
The http body will contain only payload data, and will be in the format as specified by the request.
The format is confirmed in the Content-Type response header variable.

To ensure that only new receiving order events are included when the next request is made, the last event id should be saved so that it can be used as the value of `eventid[gt]` in the next request.

### Response Data
Please refer to separate XSD and sample XML documents.

### Example API call
`GET https://api.morrisons.com/inventory/v1/receivingorders/@all/events?location=1234&eventid[gt]=12557483&limit=20`
