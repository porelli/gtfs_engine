---
title: DataSet
layout: default
toc: removeTopLevel
---

# DataSet

The **DataSet** model is not a GTFS model, but describes a single set of such
models, all of which were obtained from a single zip file.

## Routes

### Get All DataSets

```
GET /
```

Returns every `DataSet` in the database. The returned data is a JSON
object where each key is the `name` of a data source, and the value is
the list of DataSets from that source.

#### Example Response

``` json
{
  "status": "success",
  "data": {
    "halifax": [
      {
        "id": 1,
        "name": "halifax",
        "title": "Halifax Metro",
        "url": "http://www.example.com/metrotransit/google_transit.zip",
        "etag": "3a5cb3-4f31f92da3617",
        "created_at": "2014-03-09T00:20:11.998-03:00"
      }
      {
        "id": 2,
        "name": "halifax",
        "title": "Halifax Metro",
        "url": "http://www.example.com/metrotransit/google_transit.zip",
        "etag": "ac43f674eaa0cf1:0",
        "created_at": "2014-07-19T00:25:55.512-03:00"
      }
    ],
    "edmonton": [
      {
        "id": 3,
        "name": "edmonton",
        "title": "Edmonton Transit System",
        "url": "http://www.example.com/etsdatafeed.zip",
        "etag": "52699964659bcf1:0",
        "created_at": "2014-07-19T00:31:46.242-03:00"
      }
    ],
  }
}
```

### Get A Single DataSet {#single}

```
GET /:id
```

Returns the `DataSet` with the given `:id`.

The returned resource also supplies an additional field: **details**. This field
lists a count of how many resources of each type are available from the DataSet,
and the list of filters that can be used to refine a request.

#### Example Response

``` json
{
  "status": "success",
  "data": {
    "id": 2,
    "name": "halifax",
    "title": "Halifax Metro",
    "url": "http://www.example.com/metrotransit/google_transit.zip",
    "etag": "ac43f674eaa0cf1:0",
    "created_at": "2014-07-19T00:25:55.512-03:00",
    "details": {
      "agencies": {
        "count": 1,
        "filters": [
          "agency_name",
          "agency_url",
          "agency_timezone",
          "agency_lang"
        ]
      },
      "calendars": {
        "count": 14,
        "filters": [
          "monday",
          "tuesday",
          "wednesday",
          "thursday",
          "friday",
          "saturday",
          "sunday",
          "start_date",
          "end_date"
        ]
      },
      "calendar_dates": {
        "count": 26,
        "filters": [
          "date",
          "exception_type"
        ]
      },
      "fare_attributes": {
        "count": 0,
        "filters": [
          "price",
          "currency_type",
          "payment_method",
          "transfers",
          "transfer_duration"
        ]
      },
      "fare_rules": {
        "count": 0,
        "filters": [
          "route_id",
          "origin_id",
          "destination_id",
          "contains_id"
        ]
      },
      "feed_infos": {
        "count": 0,
        "filters": [
          "feed_publisher_name",
          "feed_publisher_url",
          "feed_lang",
          "feed_start_date",
          "feed_end_date",
          "feed_version"
        ]
      },
      "frequencies": {
        "count": 0,
        "filters": [
          "start_time",
          "end_time",
          "headway_secs",
          "exact_times"
        ]
      },
      "routes": {
        "count": 156,
        "filters": [
          "agency_id",
          "route_short_name",
          "route_long_name",
          "route_type",
          "agency"
        ]
      },
      "shapes": {
        "count": 124191,
        "filters": [
          "shape_pt_lat",
          "shape_pt_lon",
          "shape_pt_sequence"
        ]
      },
      "stops": {
        "count": 2468,
        "filters": [
          "stop_code",
          "stop_name",
          "stop_desc",
          "stop_lat",
          "stop_lon",
          "zone_id",
          "location_type",
          "parent_station",
          "stop_timezone",
          "wheelchair_boarding"
        ]
      },
      "stop_times": {
        "count": 588376,
        "filters": [
          "arrival_time",
          "departure_time",
          "stop_sequence",
          "stop_headsign",
          "pickup_type",
          "drop_off_type"
        ]
      },
      "transfers": {
        "count": 0,
        "filters": [
          "transfer_type",
          "min_transfer_time"
        ]
      },
      "trips": {
        "count": 14853,
        "filters": [
          "service_id",
          "trip_headsign",
          "trip_short_name",
          "direction_id",
          "block_id",
          "route_id",
          "shape_id",
          "wheelchair_accessible",
          "bikes_allowed"
        ]
      }
    }
  }
}
```

### Get the Newest DataSet for a Source

```
GET /:name
```

Typically, an application will only be interested in the most current data. This
action will return the newest DataSet with the given `:name`.

See [A Single DataSet](#single)

## Fields

### id []{.unique .required;}

The **id** field contains an ID that uniquely identifies a DataSet. The **id**
is dataset unique.

### name `(required)`

The **name** of the source which this DataSet came from. This name is a very
short identifier for the data set. It will server as the key for list of data
sets returned for this source.

### title `(required)`

A more verbose version of **name** which describes the data set. This text is
more free-form, but still be a proper noun phrase. For instance, if the **name**
is `"halifax"`, the **title** could be `"Halifax Metro Transit"`.

### url `(required)`

The **url** where the data came from.

### etag `(required)`

The HTTP header, **etag**, is used to determine when the DataSet at the **url**
has been updated. During an update, if the **etag** at the remote location has
not changed, the update will be skipped.

### created_at `(required)`

The time this DataSet was downloaded from the **url**.
