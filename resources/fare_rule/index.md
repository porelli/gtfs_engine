---
title: FareRule
layout: default
cite_google: true
required_model: true
toc: removeTopLevel
----

# FareRule

> Rules for applying fare information for a transit organization's routes.

The fare_rules table allows you to specify how fares in
[fare_attributes.txt](fare_attribute) apply to an itinerary. Most fare
structures use some combination of the following rules:

 - Fare depends on origin or destination stations.
 - Fare depends on which zones the itinerary passes through.
 - Fare depends on which route the itinerary uses.

For examples that demonstrate how to specify a fare structure with
fare_rules.txt and [fare_attributes.txt](fare_attribute), see
[FareExamples](examples) in the GoogleTransitDataFeed open source project wiki.

## Fields

### fare_id `(required; unique)`

The **fare_id** field contains an ID that uniquely identifies a fare class. This
value is referenced from the [fare_attributes.txt](fare_attribute) file.

### route_id

The **route_id** field associates the fare ID with a route. Route IDs are
referenced from the [routes.txt](route) file. If you have several routes with
the same fare attributes, create a row in fare_rules.txt for each route.

For example, if fare class "b" is valid on route "TSW" and "TSE", the
fare_rules.txt file would contain these rows for the fare class:

```
b,TSW
b,TSE
```

### origin_id

The **origin_id** field associates the fare ID with an origin zone ID.
Zone IDs are referenced from the [stops.txt](stop) file. If you have
several origin IDs with the same fare attributes, create a row in
fare_rules.txt for each origin ID.

For example, if fare class "b" is valid for all travel originating from
either zone "2" or zone "8", the fare_rules.txt file would contain
these rows for the fare class:

```
b, , 2
b, , 8
```

### destination_id

The **destination_id** field associates the fare ID with a destination zone ID.
Zone IDs are referenced from the [stops.txt](stop) file. If you have several
destination IDs with the same fare attributes, create a row in fare_rules.txt
for each destination ID.

For example, you could use the origin_ID and destination_ID fields together to
specify that fare class "b" is valid for travel between zones 3 and 4, and for
travel between zones 3 and 5, the fare_rules.txt file would contain these rows
for the fare class:

```
b, , 3,4
b, , 3,5
```

### contains_id

The **contains_id** field associates the fare ID with a zone ID, referenced from
the [stops.txt](stop) file. The fare ID is then associated with itineraries that
pass through every contains_id zone.

For example, if fare class "c" is associated with all travel on the GRT route
that passes through zones 5, 6, and 7 the fare_rules.txt would contain these
rows:

```
c,GRT,,,5
c,GRT,,,6
c,GRT,,,7
```

Because all contains_id zones must be matched for the fare to apply, an
itinerary that passes through zones 5 and 6 but not zone 7 would not have fare
class "c". For more detail, see [FareExamples](examples) in the
GoogleTransitDataFeed project wiki.

[fare_attribute]: {{ site.url }}/resources/fare_attribute
[route]:          {{ site.url }}/resources/route
[stop]:           {{ site.url }}/resources/stop
[examples]:       http://code.google.com/p/googletransitdatafeed/wiki/FareExamples
