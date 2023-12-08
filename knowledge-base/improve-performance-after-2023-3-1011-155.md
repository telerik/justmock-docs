---
title: Slow tests when upgrading to JustMock 2023.3.1011.155
description: Improve the performance of tests after upgrading to JustMock 2023.3.1011.155.
type: troubleshooting
page_title: Slow tests when upgrading to JustMock 2023.3.1011.155
slug: improve-performance-after-2023-3-1011-155
tags: justmock, performance, tests, slow, upgrade
res_type: kb
---

# Description
Tests that reference a lot of 3rd party libraries may experience degraded performance after upgrading to JustMock 2023.3.1011.155. 

# Solution
Set an environment variable called `JUSTMOCK_NEWOBJ_INTERCEPTION_ON_OVERWRITE_ENABLED` to `0` before test execution. The easiest way to set this environment variable is to use runsettings. Here is an example:

{{region }}
    <RunSettings>
        <RunConfiguration>
           <EnvironmentVariables>
            <JUSTMOCK_NEWOBJ_INTERCEPTION_ON_OVERWRITE_ENABLED>0</JUSTMOCK_NEWOBJ_INTERCEPTION_ON_OVERWRITE_ENABLED>
           </EnvironmentVariables>
        </RunConfiguration>
    </RunSettings>
{{endregion}}


