---
title: "A good rule when using AutoMapper"
excerpt: ""
date: 2023-09-30 12:00:00 AM UTC
date_last_modified:
categories:
  - Programming
tags: 
  - AutoMapper
  - ABP Framework
  - ABP.IO Platform
  - ASP.NET
  - .NET
  - C#
published: true
---

I've had bad experiences with AutoMapper in at least two codebases I was involved in.

Because of those experiences, I promised myself never to use AutoMapper again. And if I will be involved in a codebase which uses it, I might suggest to slowly remove AutoMapper from the codebase and use manual mapping instead.

That was the case until I read the free ebook ["Implementing Domain Driven Design"](https://abp.io/books/implementing-domain-driven-design) by Halil İbrahim Kalkan, the creator of [ABP Framework](https://abp.io/).

On page 70 of the book he says

> **Input DTOs** (those are passed to the Application Service
methods) have different nature than **Output DTOs** (those are
returned from the Application Service methods). So, they will be
treated differently.

I've known about using different or separate `ViewModels` as parameter for each action method in ASP.NET MVC Controllers to [prevent overposting attacks](https://www.hanselman.com/blog/aspnet-overpostingmass-assignment-model-binding-security), but I can't remember of having read anything  which classifies ViewModels or DTOs as input vs output. 

I don't know whether Halil got this idea from others, or whether he came up with this by himself from his own experiences, but this differentation betweeen input and output DTOs is very good stuff.

In the book, he gave some best practices for input and output DTOs.

Here's an example:

> **Do not Re-Use Input DTOs**
>
> Define a **specialized input DTO for each use case** (Application
Service method)...
> 
> Sometimes, it seems appealing to reuse the same DTO class for
two use cases, because they are almost same. Even if they are the
same now, they will probably become different over time and
you will come to the same problem. **Code duplication is a better practice than coupling use cases.**
>
> Another way of reusing input DTOs is **inheriting** DTOs from
each other. While this can be useful in some rare cases, most of
the time it brings you to the same point.

And regarding the use of AutoMapper or any automapping tool, he set forward a rule on page 80 of the book. He says

> **Use** auto object mapping only for Entity to output DTO mappings.
>
> **Do not use** auto object mapping for input DTO to Entity mappings.

To me, finding this _use/do-not-use_ automapping rule is like finding a gem. Had I known this earlier, it could have guided me in finding the best solution for the difficulties I encountered with AutoMapper.

The rest of page 80 of the book seems to suggest that the the rule _"Do not use auto object mapping for input DTO to Entity mappings"_ might be applicable only when you have a rich domain model. You _might_ be able to bypass this rule if you do not have rich domain model.
