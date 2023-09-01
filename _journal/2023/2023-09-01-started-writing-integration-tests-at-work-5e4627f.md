---
title: "Started writing integration tests at work"
excerpt: ""
date: 2023-09-01 06:00:00 PM UTC
last_modified_at: 
categories:
  - Journal
  - Programming
tags: 
  - Integration Testing
  - Unit Testing
  - Clean Architecture
  - ABP Framework
  - Testcontainers
  - LocalStack
published: true
---

<!-- 2023-08-31 9:00 PM PHT: started -->

Friday, September 1, 2023

I have been writing integration tests for a task at work since Thursday last week up to yesterday morning. This task is for a software module whose codebase is still in its infancy, and is relatively small, and is written using the latest .NET Core, and is adhering to Clean Architecture principles (because it uses [ABP Framework](https://abp.io/)) --- and so is not that hard to wrap around ones head, which led me to believe that writing integration tests for this would not be that hard.

I spent about 12 to 16 hours per day of work for five workdays, and about 6 hours last Sunday evening.

I usually work for only 8 hours per work day, and do not work on weekends. But I gave lots of time for this endeavor because I made some slips at work in the previous days: In some of my recent tasks, I failed to test some of the other part of our codebase which were affected by my code changes.

And so I created these tests thinking that they can serve as my regression tests when working on new tasks in the future. And even when I cannot create automated tests for my future tasks, the existence of these tests would help remind me to consider testing all the other parts of our codebase which will be affected by my code changes.

I am spending time to write about this experience because creating integration tests successfully for a project at work is, to my estimation, the greatest achievement in my programming career so far.

At last, I have an answer to give to someone who asks "What contributions have you made at work that you are proud of?" (The truth is, I'm not yet sure if the integration tests I made will be merged into our main codebase/branch. I'm still waiting for the review from our software architect. But I will still be using this privately even when it will not be merged in our main codebase; and I will be learning more things about writing integration tests through this.)

Prior to this experience, I have not contributed anything significant to projects I have been involved in: I would just implement the feature I was told to do, and fix bugs I was assigned to fix. When I attempted to do something significant, I fail. I can even remember contrbuting negatively to one project while working for a different company many years ago :

I was assigned to be the only developer of an existing ASP.NET MVC codebase, which is relatively small in size. After a few weeks of working on it, I thought of writing unit tests for the code changes I have made, because I have just recently read the book "Working Effectively with Legacy Code", and I was convinced that

> "Code without tests is bad code. It doesn't matter how well written it is; it doesn't matter how pretty or object-oriented or well-encapsulated it is. With tests, we can change the behavior of our code quickly and verifiably. Without them, we really don't know if our code is getting better or worse."
> --- quote from Michael Feathers (Working Effectively with Legacy Code, WELC)

And because the codebase was not structured to be unit-testable, I attempted to start restructuring it litle-by-little to adhere to Clean Architecture ideas.

> "Architecture is the art of drawing lines, with the interesting rule that once you have drawn the lines, all the dependencies that cross that line go in the same direction."
> --- quote from Uncle Bob Martin ([Architecture the Lost Years](https://www.youtube.com/watch?v=WpkDN78P884))

The purpose of that restructuring was to make the code I wanted to test depend on abstractions (rather than on concretions). And because abstract dependencies are easier to mock compared to concrete dependencies, it was then possible for me to write unit tests without much hassle. 

<!-- (If I remember correctly I also tried to write 'acceptance tests') -->

But the consequence of that restructuring was that codebase will be in a state of having two coexisting structures for an unknown period of time: first, the old Top-Down-style Layered sturcture, and second, the Clean/Onion-style Layered structure.

In retrospect, I think that that restructuring was a very wrong decision. I was so naive at that time. I did not have a senior developer telling me that

> ... conceptual integrity is the most important consideration in system design. It is better to have a system omit certain anomalous features and improvements, but to reflect one set of design ideas, than to have one that contains many good but independent and uncoordinated ideas.
>
> Conceptual integrity in turn dictates that the design must proceed from one mind, or from a very small number of agreeing resonant minds.
>
> ... I will certainly not contend that only the architects will have good architectural ideas. Often the fresh concept does come from an implementer or from a user. However, all my own experience convinces me, and I have tried to show, that the conceptual integrity of a system determines its ease of use. Good features and ideas that do not integrate with a system's basic concepts are best left out. If there appear many such important but incompatible ideas, one scraps the whole system and starts again on an integrated system with different basic concepts.
> --- quote from Fred Brooks (The Mythical Man-Month)

I think it was a wrong decision because that task of restructuring and writing tests was later abandoned. The reason is, among others, I was assigned a second project, and so did not have that much time to concentrate on that first project. And I later learned that the first project was being billed on an hourly basis. So I thought that if I continue doing the restructuring, the client might wonder why he is being billed for something he did not approve or asked for. (I was not and still am not good with sales talk)

I have digressed from the main focus of this journal entry. Back to main...

Like I said above, having successfully created integration tests, even if it only covers a very small part of our codebase for now, is a great achievement to me. Because, apart from my failures with regards to automated tests which I mentioned earlier, I've had another experience of a failed attempt at writing tests for another project in a different company:

I was writing unit tests for new functionality. But then found out that there are crucial paths in our codebase which cannot be covered by unit tests, and so needs tests from the outside of the app. So I created tests from the outside using HttpClient. I was not sure what to call the kind of tests I was writing. I had learned from GOOS and from the Outside-In TDD people that this kind of tests is called Acceptance Tests (which others say is [the same as integration/functional/end-to-tend tests](https://www.obeythetestinggoat.com/book/chapter_02_unittest.html)). But I was not sure, so I just called them API Endpoint Tests. I had some success in doing that, but those tests were later abandoned because it was hard to test the existing API endpoints, because the DTOs being used are so big, and they are being reused in many different endpoints, so that it takes lots of time to trace which DTO fields are being used in the endpoint I was trying to write tests for. <small>(In case you are interested, Halil İbrahim Kalkan, the creator of ABP Framework, has shared some best practices about DTOs in his free ebook ["Implementing DDD"](https://abp.io/books/implementing-domain-driven-design))</small>

<!-- That app was written using an older ASP.NET Web API (the one which accompanies .NET Framework 4.5 or so) and, during that time there was not that many resources online on how to create integration tests for that older version of ASP.NET Web API. So I had difficulty in creating my desired integration tests. Also, I was involved with that project for 6 months only because of unforseen circumstances. So I did not have a chance of potentially finding a fix. -->


The lesson from that experience which stuck in my mind is that it is very important to be able to write tests from the outside and not just be able to write unit tests. <small>(Test from the outside is even more important when attempting to write tests for legacy codebases because you have to write what they call characterization tests for the code you need to change. Tests from the outside are also important in codebases which are not structured well for unit testing.)</small>

I later learned that some programmers consider these tests from the outside (integration tests, they call it) as more important than unit tests:

> "In my experience, writing "integration" tests in ASP.NET Core are for controllers is far more valuable than trying to unit test them, and is easier than ever in ASP.NET Core"
> --- [Andrew Lock](https://andrewlock.net/should-you-unit-test-controllers-in-aspnetcore/)

> "Writing integration (or functional) tests on a C# API gives more confidence in the code that is written, in addition, it increases the productivity during all stages of development."
> --- [Tim Deschryver](https://timdeschryver.dev/blog/why-writing-integration-tests-on-a-csharp-api-is-a-productivity-booster) 

> "In my opinion, one good integration test is worth 1,000 unit tests."
> ---  [Khalid Abuhakmeh](https://khalidabuhakmeh.com/secrets-of-a-dotnet-professional#integration-tests--unit-tests)


These days, especially if you are using [.NET Core](https://blog.ploeh.dk/2021/01/25/self-hosted-integration-tests-in-aspnet/) instead of the older .NET Framework, lots of hurdles have been removed for successfully implementing integration tests. [Docker](https://www.youtube.com/watch?v=8IRNC7qZBmk) is now ubiquitous; and we now have [Testcontainers](https://www.azureblue.io/asp-net-core-integration-tests-with-test-containers-and-postgres/), which can be used to create throwaway instances of Postgres for example; and [LocalStack](https://blog.genezini.com/p/integration-tests-with-aws-s3-buckets-using-localstack-and-testcontainers/), which can be used to emulate AWS services for example.

I think that all those days of struggles with learning to write automated tests is now starting to pay off: From my first encounters with unit testing ten years ago: To my encounters with other kinds of testing, like acceptance testing (from GOOS), and characterization testing (from WELC). And now I am able to apply things which I have learned over the years. 

I'm thankful to all those who have written books and blog post and videos on writing automated tests and on TDD, and to those who have made the tools necessary for writing integration tests.

I'm thankful to my previous bosses and clients and workmates who have been patient with me.

(murag graduation speech ba)

Most of all, I thank my God and Savior Jesus the Messiah for never leaving me alone (even during those times when I wanted to be left alone), and for guiding me in living this life. May His kingdom come; may His will be done on earth as it is in heaven.
