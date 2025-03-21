---
{
    "title": "JSON_QUOTE",
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
Enclose json_value in double quotes ("), escape special characters contained.

## Syntax
```sql
JSON_QUOTE (<a>)
```

## Parameters

| Parameter | Description                                       |
|-----------|------------------------------------------|
| `<a>`     | The value of the json_value to be enclosed.   |


## Return Values
Return a json_value. Special cases are as follows:
* If the passed parameter is NULL, return NULL.

### Examples
```sql
SELECT json_quote('null'), json_quote('"null"');
```
```text
+--------------------+----------------------+
| json_quote('null') | json_quote('"null"') |
+--------------------+----------------------+
| "null"             | "\"null\""           |
+--------------------+----------------------+
```
```sql
SELECT json_quote('[1, 2, 3]');
```
```text
+-------------------------+
| json_quote('[1, 2, 3]') |
+-------------------------+
| "[1, 2, 3]"             |
+-------------------------+
```
```sql
SELECT json_quote(null);
```
```text
+------------------+
| json_quote(null) |
+------------------+
| NULL             |
+------------------+
```
```sql
select json_quote("\n\b\r\t");
```
```text
+------------------------+
| json_quote('\n\b\r\t') |
+------------------------+
| "\n\b\r\t"             |
+------------------------+
```
