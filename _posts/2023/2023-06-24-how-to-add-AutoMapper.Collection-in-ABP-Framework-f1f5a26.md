---
title: "How to add AutoMapper.Collection in ABP Framework"
excerpt: ""
date: 2023-06-24 05:00:00 PM UTC
date_last_modified: 2023-09-07 03:30:00 PM UTC
categories:
  - Programming
tags: 
  - AutoMapper.Collection
  - AutoMapper
  - ABP Framework
  - ABP.IO Platform
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

-------

I created a demo for this using the Todo App from the abp-samples repository.

Go [here](https://github.com/jflaga/abp-samples/tree/jflaga/play/AutoMapper.Collection/TodoApp/Angular-EfCore/aspnet-core) to view the demo.

Or clone my fork of the abp-samples repository, then checkout branch `play/AutoMapper.Collection`

``` terminal
> git clone https://github.com/jflaga/abp-samples.git
> git checkout play/AutoMapper.Collection
```


(NOTE: AutoMapper.Collection does not work with `IReadOnlyCollection`. A solution can be found [here](https://github.com/AutoMapper/AutoMapper.Collection/issues/132#issuecomment-539411397)) 

