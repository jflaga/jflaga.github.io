---
title: "ABP Error: The breakpoint will not currently be hit. No symbols have been loaded for this document."
excerpt: ""
date: 2023-11-05 02:55:00 PM UTC
date_last_modified:
categories:
  - Programming
tags: 
  - ABP Framework
  - ASP.NET
  - .NET
  - C#
published: true
---

When we need to use an ABP module in a project, we know that a reference to that module needs to be added in the `.csproj` file of the relevant project. 

Here's an example of adding reference to a [`HttpApi.Client` module](https://docs.abp.io/en/abp/latest/API/Dynamic-CSharp-API-Clients) in a `Web` project:

<!-- Dynamic C# API Client Proxies - https://docs.abp.io/en/abp/latest/API/Dynamic-CSharp-API-Clients -->

``` xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <RootNamespace>...Web</RootNamespace>
    ...
  </PropertyGroup>
  <ItemGroup>
    <ProjectReference Include="..\src\<...>HttpApi.Client\<...>HttpApi.Client.csproj" />
    ...
  </ItemGroup>
</Project>
```

That part we remember.


But we might forget that we **also** need to **add the module in the `[DependsOn]` attribute** of the relevant project.

<!-- 

``` csharp
[DependsOn(
  typeof(...HttpApiClientModule),
  ...
  typeof(AbpSwashbuckleModule)
  )]
public class ...WebModule : AbpModule
{
  public override void PreConfigureServices(ServiceConfigurationContext context)
  {
    ...
  }
}
``` 
-->

![ABP: Add the module in the `[DependsOn]` attribute of the relevant project .](/assets/images/2023/2023-11-05-abp-add-module-in-dependson-attribute.png)


Forgetting that will result to the following error when running the relevant project in debug mode.

![ABP Error: The breakpoint will not currently be hit. No symbols have been loaded for this document.](/assets/images/2023/2023-11-05-abp-error-no-symbols-have-been-loaded-for-this-document.png)
