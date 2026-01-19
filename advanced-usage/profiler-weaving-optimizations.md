---
title: Performance Optimizations
page_title: Performance Optimizations | JustMock Documentation
description: Performance Optimizations
previous_url: /advanced-usage-profiler-weaving-optimizations.html
slug: justmock/advanced-usage/profiler-weaving-optimizations
tags: weaving,profiler,performance, optimize
published: True
position: 22
---

# Performance Optimizations

To allow advanced mocking, **JustMock** needs to insert an additional code to the different members prior the CLR compiles them to machine code. This action is called instrumentation (code weaving) and inevitably affects the performance of executing unit tests. To improve the execution time, **JustMock** allows you to configure what should be instrumented. This article describes how you can enable such optimizations.

## Enabling

There are two possible ways to enable waving filter:

* The suggested approach is to set environment variable **JUSTMOCK_CONFIG_PATH** with full path to the configuration file.
- In case this is not possible in a specific setup, place a file that contains the settings in the same directory where the profiler resides. This file must be named **config.json**.


## File Format

Supported format is JSON. When the file cannot be parsed, failures are logged (see [Profiler Log And Trace](advanced-usage/profiler-log-and-trace.md) article), and the configuration is rejected.

### Structure

The weaving configuration is represented by the key of the root object named `weavingFilter`. This key itself is an object, containing the following key-value pairs:

* `defaultAction` string (mandatory)—Its value should be one of the  [Action Constants](#action-constants).
* `entries` array (mandatory)—Contains objects, representing the entries the filter applies to. See [Filter Entry](#filter-entry) section below.

### Filter Entry

Each entry inside the `entries` array is an object with the following key-value pairs:

* `scope` string (mandatory)—Its value should be one of the [Scope constants](#scope-constants).
* `pattern` string (mandatory)—Regular expression to match the scope of the filter entry.
* `action` string (mandatory)—Its value should be one of the [Action Constants](#action-constants).
* `supplements` array (optional)—Contains supplementary entry objects. The supplements are used to describe child members of an entry, see [Supplements](#supplements).
* `options` array (optional)—Contains objects used as parameters in the evaluation, see [Options](#options).

### Action Constants

Used for defining values for the `action` key in the filter entry objects. The possible values are:

* `"include"`
* `"exclude"`

### Scope Constants

Used for defining the target scope of the filtering. The possible values are:

* `"module"`—Applies to assembly.
* `"type"`—Applies to type.
* `"member"`—Applies to member (method or property).

### Options
Filter entries can have additional evaluation parameters, which are objects with the following keys:

* `name` string (mandatory)—Contains the unique entry name.
* `value` string, number or Boolean—Its value depends on the value of the `name` key.

**Available options**

At this point, a single option is available that allows you specify the accessibility of a member entry.
```json
  {
    "name": "accessibility",
    "value": "public" | "private"
  }
```
This option can be applied to the **member entries only**, where `"public"` refers to the public members, and `"private"` - to all others.

### Supplements

The `supplements` key describes child members of another entry. The supplements can be used when you need to specify a type from an assembly, or a member of a type.

Supplements of a given entry can have supplements of the other types. The following rules apply:

* Module entry can have only type supplements.
* Type entry can have only member supplements.

## Example

The configuration sample below allows instrumentation of all methods matching to the following conditions:

* Belong to type from __mscorlib__ assembly.
* Belong to __System.String__ type and are public properties (names are starting with *get_*).
* Do not belong to __System.DateTime__ type.

The settings file for a similar setup would look like the following example:
```json
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

To setup your environment to use the performance optimizations discussed in this topic, you need to pass the path to the configuration file through the **JUSTMOCK_CONFIG_PATH** environment variable.

The options that you can use to achieve that vary depending on the specific setup you might have.
In command line or inside Visual Studio, you can use the *.runsettings* file to specify the path. The next examples shows how you can set the variable in *.runsettings*:


```xml
<RunSettings>
    <RunConfiguration>
      <EnvironmentVariables>
        <JUSTMOCK_CONFIG_PATH>%config.json.path%</JUSTMOCK_CONFIG_PATH>
      </EnvironmentVariables>
    </RunConfiguration>
</RunSettings>
```
__%config.json.path%__ should be replaced with full path to the configuration file.