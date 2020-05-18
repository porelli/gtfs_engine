---
title: GTFS Engine
layout: default
toc: yes
---

# Introduction

This "GTFS (General Transit Feed Specification) Engine" is a [Rails
Engine](rails-engine) which will allow your Rails application to process GTFS
feeds and model their associations through *ActiveRecord* objects.

This project is currently a sub-component of the [KnowTime Server](knowtime_git)
project, but hopefully can of use to others projects. This project also depends
on another GitHub project, [GTFS Reader](gtfs_reader_git), to actually process
the *.zip* files in which GTFS feeds are contained.

# Example


A live example is running as part of the KnowTime Server project at
[api.knowtime.ca/v2/gtfs](http://api.knowtime.ca/v2/gtfs)

# Configuration

You can add `gtf-engine` to your Rails application the same way as you would any
Rails Engine. Simply:

Add the following to your `Gemfile`:

``` ruby
gem 'gtfs_engine'
```

And then you only have to mount the engine at some path. Add something like the
following to your `config/routes.rb` file:

``` ruby
mount GtfsEngine::Engine, at: '/gtfs'
```

Finally, you need to import the DB migration files that define the GTFS
resources used by this engine:

``` bash
rake 'gtfs_engine:install:migrations'
```

## Sources

To actually get GTFS data into your database, you need to specify the sources of
your data. The easiest way to do this is to create an [initializer
file](http://guides.rubyonrails.org/configuring.html#using-initializer-files)
that species all of the GTFS data sources you are interested in. Here is an
example:

``` ruby
GtfsEngine.sources do |source|
  source.halifax do
    title 'Halifax Metro'
    url   'http://www.example.com/metrotransit.zip'
  end
  source.edmonton do
    title 'Edmonton Transit System'
    url   'http://example.com/etsdatafeed.zip'
  end
end
```

That being said, the source data is only used when updating the data sources. As
this is most often done via `rake` tasks (see below), you may want to restrict
your configuration to that.

# Resources

 - [DataSet]({{ site.url }}/resources/data_set)
 - [Agency]({{ site.url }}/resources/agency)
 - [Stop]({{ site.url }}/resources/stop)
 - [Route]({{ site.url }}/resources/route)
 - [Trip]({{ site.url }}/resources/trip)
 - [StopTime]({{ site.url }}/resources/stop_time)
 - [Calendar]({{ site.url }}/resources/calendar)
 - [CalendarDate]({{ site.url }}/resources/calendar_date)
 - [FareAttribute]({{ site.url }}/resources/fare_attribute)
 - [FareRule]({{ site.url }}/resources/fare_rule)
 - [Shape]({{ site.url }}/resources/shape)
 - [Frequency]({{ site.url }}/resources/frequency)
 - [Transfer]({{ site.url }}/resources/transfer)
 - [FeedInfo]({{ site.url }}/resources/feed_info)


# JSend

The GTFS Engine currently returns only one type of content: [JSON (JavaScript
Object Notation)](wiki-json); moreover, the returned data is wrapped in a
[JSend-compliant response](jsend)

# Rake Tasks

This Rails Engine provides a few `rake` tasks to help you maintain your GTFS
data sets.

## Install Migration

``` bash
rake 'gtfs_engine:install:migrations'
```

Like most Rails Engines, this engine defines many `ActiveRecord` classes,
representing the various GTFS resources. This rake task will copy these
migration files to your Rails app's `db/migrate` directory.

## List the DataSets in the database

``` bash
rake 'gtfs_engine:sets:list'
```

This lists all of the DataSets in the database. The sets are ordered under a
heading for each source.

### Example Output

```
halifax
    ac43f674eaa0cf1:0: 2014-07-19 00:25:55 -0300 (http://www.halifax.ca/metrotransit/googletransitfeed/google_transit.zip)
    d57-4f669d05eedfd: 2014-07-19 00:52:32 -0300 (http://localhost/sample-feed.zip)

edmonton
    52699964659bcf1:0: 2014-07-19 00:31:46 -0300 (http://webdocs.edmonton.ca/transit/etsdatafeed/google_transit.zip)
```

### Update a DataSource

``` bash
rake 'gtfs_engine:update[name]'
```

Checks the URL for the named Data Source and, if it's new, download it and
insert its resources into the DB.

### Update every DataSource

``` bash
rake 'gtfs_engine:update'
```

Updates every DataSource configured with `GtfsEngine.sources`.

### Delete a DataSource

``` bash
rake 'gtfs_engine:sets:delete[name,etag]'
```

Deletes the DataSet from the **name**d source with the given **etag**.

### Count the Resources in a DataSet

``` bash
rake 'gtfs_engine:sets:count[name,etag]'
```

#### Example Output

```
Model count for halifax data set with etag ac43f674eaa0cf1:0
               agencies: 1
              calendars: 14
         calendar_dates: 26
        fare_attributes: 0
             fare_rules: 0
             feed_infos: 0
            frequencies: 0
                 routes: 156
                 shapes: 124191
                  stops: 2468
             stop_times: 588376
              transfers: 0
                  trips: 14853
```


[rails-engine]:    {{ site.data.urls.rails_engine }}
[gtfs_reader_git]: {{ site.data.urls.gtfs_reader_git }}
[wiki-json]:       {{ site.data.urls.wiki_json }}
[jsend]:           {{ site.data.urls.jsend }}
