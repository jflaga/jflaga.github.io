---
title: "How to add AutoMapper.Collection in ABP Framework"
excerpt: ""
date: 2023-06-24 08:40:00 PM UTC
last_modified_at:
categories:
  - Miscellaneous
tags: 
  - AutoMapper.Collection
  - AutoMapper
  - ABP Framework
  - ASP.NET
published: true

toc: true
toc_label: 
toc_icon:  # corresponding Font Awesome icon name (without fa prefix)
---


### 1. Install the needed NuGet packages

``` terminal
PM> Install-Package AutoMapper.Collection
```

If you are using the older Entity Framework:

``` terminal
PM> Install-Package AutoMapper.Collection.EntityFramework
```

If you are using the older Entity Framework Core:

``` terminal
PM> Install-Package AutoMapper.Collection.EntityFrameworkCore
```


### 2. Call `AddCollectionMappers()` in your AbpModule class

``` csharp
[DependsOn(typeof(AbpAutoMapperModule))]
public class MyModule : AbpModule
{
    public override void ConfigureServices(ServiceConfigurationContext context)
    {
        Configure<AbpAutoMapperOptions>(options =>
        {
            options.Configurators.Add(context =>
            {
                context.MapperConfiguration.AddCollectionMappers();
            });
            options.AddMaps<MyModule>();
        });
    }
}
```


### 3. Use `EqualityComparison()` in your AutoMapper.Profile class

``` csharp
public class AutoMapperProfile : Profile
{
    public AutoMapperProfile()
    {
        ...        
        CreateMap<OrderDTO, Order>();

        CreateMap<OrderItemDTO, OrderItem>()
          .EqualityComparison((odto, o) => odto.ID == o.ID);
        ...
    }
}
```


Please see the [AutoMapper.Collection docs](https://github.com/AutoMapper/AutoMapper.Collection) for the explanation of what `EqualityComparison()` does.
