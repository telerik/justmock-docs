---
title: Profiler Weaving Optimizations
page_title: Profiler Weaving Optimizations | JustMock Documentation
description: Profiler Weaving Optimizations
previous_url: /advanced-usage-profiler-weaving-optimizations.html
slug: justmock/advanced-usage/profiler-weaving-optimizations
tags: weaving,profiler,performance
published: True
position: 23
---

# Profiler Weaving Optimizations

JustMock profiler has a built-in capability to filter CLR weaving (instrumentation) based on a given assembly, class and/or method. This will help you in minimizing time for instrumentation and improve overall performance of the test runner.

## Enabling

There are two possible ways to enable waving filter:

* preferred one, set environment variable __JUSTMOCK_CONFIG_PATH__ with full path to configuration file
* fallback one, place a file named __config.json__ in the same directory where the profiler resides

## File format

Supported format is JSON, parse failures are logged (see [Profiler Log And Trace](profiler-log-and-trace.html) article), and the configuration is rejected.

### Structure

The weaving configuration is represented by the key of the root object named __weavingFilter__. This key itself is an object, containing the following key-value pairs:

* __defaultAction__ string (mandatory), see Action constants below
* __entries__ array (mandatory), contains objects, see Filter entry below

### Filter entry

Each entry is an object with the following common key-value pairs:

* __scope__ string (mandatory), see Scope constants below
* __pattern__ string (mandatory), regular expression to match the scope of the filter entry
* __action__ string (mandatory), see Action constants below
* __supplements__ array (optional), contains supplement entry objects, see Remarks
* __options__ array (optional), contains objects used as parameters in the evaluation, see Remarks

### Action constants

Used for defining action key values in the entry objects, possible values are:

* __"include"__
* __"exclude"__

### Scope constants

Used for defining the target scope of the filtering, further used as type possible values are:

* __"module"__ applies to assembly
* __"type"__ applies to type
* __"member"__ applies to member (method or property)

### Remarks

Supplements of a given entry can have supplements of the other types, the following rules applied:

* module entry can have only type supplements
* type entry can have only member supplements

Filter entries can have additional evaluation parameters, which are objects with the following keys:

* __name__ string (mandatory), contains the unique name
* __value__ string, number or Boolean, depending of the option

Available options are:

* {__"name"__: "accessibility", __"value"__: "public" | "private"} - can be applied to the member entrtries only, where "public" refers to the public members, and "private" - to all others

## Example

The configuration sample below allows instrumentation of all methods matching to the following conditions:

* are belong to type from __mscorlib__ assembly
* are belong to __System.String__ type and are public properties (names are starting with get_)
* are not belong to __System.DateTime__ type

Configuration file looks as following:

```
{
  "weavingFilter": {
    "defaultAction": "exclude",
    "entries": [
      {
        "scope": "module",
        "pattern": "mscorlib",
        "action": "include",
        "supplements": [
          {
            "scope": "type",
            "pattern": "System.String",
            "action": "include",
            "supplements": [
              {
                "scope": "member",
                "pattern": "get_.*",
                "action": "include",
                "options": [
                  {
                    "name": "accessibility",
                    "value": "public"
                  }
                ]
              }
            ]
          },
          {
            "scope": "type",
            "pattern": "System.DateTime",
            "action": "exclude"
          }
        ]
      }
    ]
  }
}
```

## Setting up environment

One of the possible solutions is to use .runsettings (can be used command line or inside Visual Studio), here is the sample:

```
<RunSettings>
    <RunConfiguration>
      <EnvironmentVariables>
        <JUSTMOCK_CONFIG_PATH>%config.json.path%</JUSTMOCK_CONFIG_PATH>
      </EnvironmentVariables>
    </RunConfiguration>
</RunSettings>
```
__%config.json.path%__ should be replaced with full path to the configuration file.