---
{
"title": "CANCEL RESTORE",
"language": "en"
}
---

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->



## Description

This statement is used to cancel an ongoing RESTORE task.

## Syntax

```sql
CANCEL RESTORE FROM <db_name>;
```

## Parameters

**1.`<db_name>`**

The name of the database to which the recovery task belongs.

## Usage Notes

- When cancellation is around a COMMIT or later stage of recovery, the table being recovered may be rendered inaccessible. At this time, data recovery can only be performed by executing the recovery job again.

## Example

1. Cancel the RESTORE task under example_db.

```sql
CANCEL RESTORE FROM example_db;
```
