# Merchandising API

## Items (change events)
As items as added, amended and deleted these events are appended to a list of change events in the order that they happen.
These change events can be accessed through a special events collection within the items collection.

The API will always return event items in the order they were created and each event item includes a unique event id.
The event id is always in ascending sequence, but can (and often will) have gaps in the sequence.

### Request
`GET https://{DNS}/merchandising/v1/items/@all/events?{parameters}`

##### Applicable parameters:
<table>
    <tr>
        <td>Parameter</td>
        <td>Description</td>
        <td>Example</td>
        <td></td>
    </tr>
    <tr>
        <td>view</td>
        <td>Specifies the type of item data to be returned. This should always be set to warehouse.</td>
        <td>view=warehouse</td>
        <td>Mandatory</td>
    </tr>
    <tr>
        <td>location</td>
        <td>Filters the response to only include data for a specific location or list of locations</td>
        <td>location=1234</td>
        <td>Mandatory</td>
    </tr>
    <tr>
        <td>format</td>
        <td>Specifies the format of the data to be returned in the HTTP body.<br />
            Valid values are XML or JSON. If not specified JSON will be returned.
        </td>
        <td>format=JSON<br />
            format=XML
        </td>
        <td>Optional</td>
    </tr>
    <tr>
        <td>limit</td>
        <td>An integer value specifying the maximum number of items the API should return in response to this request.<br />
            Where no limit is specified, the entire item set will be returned.
            <br />Please note that the data set may me many megabytes in size.
        </td>
        <td>limit=10</td>
        <td>Optional</td>
    </tr>
    <tr>
        <td>eventid[gt]</td>
        <td>Only items with an event ID number higher than specified in this parameter will be returned.<br />
            The value specified here is typically that of the last ‘event id’ value returned in the previous request.
        </td>
        <td>eventid[gt]=12443</td>
        <td>Mandatory</td>
    </tr>
</table>

The item changes collection should be polled periodically for new change events, using eventid[gt] to avoid downloading events that have already been processed.
Therefore the last event id returned in a response should be used as the eventid[gt] parameter value for the next request.

##### Response
The http body will contain only payload data, and will be in the format as specified by the request.
The format is confirmed in the Content-Type response header variable.

To ensure that only new item change events are included when the next request is made,
the last event id should be saved so that it can be used as the value of eventid[gt] in the next request.

##### Response data
Please refer to separate XSD and sample XML documents.

##### Example API call
`GET https://api.morrisons.com/merchandising/v1/items/@all/events?view=warehouse&location=1234&eventid[gt]=12557483&limit=20`
