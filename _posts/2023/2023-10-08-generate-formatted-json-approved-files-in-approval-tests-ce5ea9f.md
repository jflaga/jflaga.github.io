---
title: "Approval Testing in .NET Core: How to generate properly formatted <code>.json</code> approved files using Shouldly and Newtonsoft.Json"
excerpt: ""
date: 2023-10-08 03:40:00 PM UTC
date_last_modified:
categories:
  - Programming
tags: 
  - ASP.NET
  - .NET
  - C#
  - Approval Testing
published: true
---

[Approval tests](https://understandlegacycode.com/blog/characterization-tests-or-approval-tests/), also called characterization tests in [WELC](https://wiki.c2.com/?WorkingEffectivelyWithLegacyCode), is a very useful tool when working on existing code which does not have tests.

Here are some ways to make the approval files (or [golden master](https://www.youtube.com/watch?v=8wPx0O4gFzc&list=PL0C32F89E8BBB5368&index=3)) easier to read when writing approval tests for Web APIs which returns JSON as response data:

1. Configure the assertion tool to produce an approval file with `.json` extension instead of a `.txt.` extension. When using [Shouldly](https://docs.shouldly.org/documentation/equality/matchapproved) for example, use this code:

   ``` csharp
   content.ShouldMatchApproved(c => c.WithFileExtension(".json"));
   ```

2. JSON-format the response data before the approval phase of the test. Newtonsoft.Json can be used to do that:

   ``` csharp
   string jsonStringFromatted = JToken.Parse(jsonString)
     .ToString(Formatting.Indented);
   ```

Doing that produces a properly formatted `.approved.json` file which looks like this..

``` json
{
  "employee": {
    ...
    "name": "Juan dela Cruz",
    "email": "juandelacruz@example.com"
  }
}
```

.. instead of the harder to read `.approved.txt` file..

```
{"employee":{..."name":"Juan dela Cruz","email":"juandelacruz@example.com"}}
```


Here is a more complete approval test sample:

``` csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Shouldly;
using System.Net.Http;
using System.Threading.Tasks;
using Xunit;
...
public class CreateEmployeeTests : ApprovalTestBase
{
  [Fact]
  public async Task Succeeds_if_happy_path()
  {
    ...
    var payload = ...
    var response = await httpClient.PostAsJsonAsync("/api/employee...", payload);

    string content = JToken.Parse(await response.Content.ReadAsStringAsync())
      .ToString(Formatting.Indented);

    content.ShouldMatchApproved(c => c.WithFileExtension(".json"));
  }
  ...
}
```