# Merchandising API

## Items (change events)
As items as added, amended and deleted these events are appended to a list of change events in the order that they happen. These change events can be accessed through a special events collection within the items collection.

The API will always return event items in the order they were created and each event item includes a unique event id. The event id is always in ascending sequence, but can (and often will) have gaps in the sequence.
