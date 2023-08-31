---
title: "Started writing integration tests at work"
excerpt: ""
date: 2023-09-01 12:00:00 AM UTC
last_modified_at: 
categories:
  - Journal
  - Programming
tags: 
  - Integration Testing
  - Regression Testing
  - Unit Testing
published: true
---

<!-- 2023-08-31 9:00 PM PHT: started -->

Friday, September 1, 2023

I have been writing integration tests for a task at work since Thursday last week up to yesterday morning. This task is for a software module whose codebase is still in its infancy, and is relatively small, and is written using the latest .NET Core, and is adhering to Clean Architecture principles (because it using ABP Framework) --- and so is not that hard to wrap around ones head, which led me to believe that writing integration tests for this would not be that hard.

I spent about 12 to 16 hours per day of work for five workdays, and about 6 hours last Sunday evening.

I usually work for only 8 hours per work day, and do not work on weekends. But I gave lots of time for this endeavor because I made some slips at work in the previous days: In some of my recent tasks, I failed to test some of the other part of our codebase which were affected by my code changes.

And so I created these tests thinking that they can serve as my regression tests when working on new tasks in the future. And even when I cannot create automated tests for my future tasks, the existence of these tests would help remind me to consider testing all the other parts of our codebase which will be affected by my code changes.

I am spending time to write about this because creating a few integration tests for a project at work is, to my estimation, the greatest achievement in my programming career so far.

At last, I have an answer to give if someone asks me "What contributions have you made at work that you are proud of?" (The truth is, I'm not yet sure if the integration tests I made will be merged into our main codebase/branch. I'm still waiting for the review fron our software architect. But I will still be using these privately even when they will not be merged in our main codebase; and I will be learning more things about writing integration tests through this.)

Prior to this, I have not contributed anything significant to projects I have been involved in. I can even remember contrbuting negatively to one project many years ago while working for a different company:

I was assigned to be the only developer of an existing ASP.NET MVC codebase, which is relatively small in size. After a few weeks of working on it, I thought of writing unit tests for the changes I have made, because I have just recently read the book "Working Effectively with Legacy Code", and I was convinced that

> "Code without tests is bad code. It doesn't matter how well written it is; it doesn't matter how pretty or object-oriented or well-encapsulated it is. With tests, we can change the behavior of our code quickly and verifiably. Without them, we really don't know if our code is getting better or worse."
> --- quote from Michael Feathers

And because the codebase was not structured to be unit-testable, I attempted to start restructuring it litle-by-little to adhere to Clean Architecture ideas.

> "Architecture is the art of drawing lines, with the interesting rule that once you have drawn the lines, all the dependencies that cross that line go in the same direction."
> --- quote from Uncle Bob Martin

That restructuring makes the system-under-test depend on abstractions (rather than on concretions). And because abstract dependencies are easier to mock compared to concrete dependencies, it was then possible for me to write unit tests without much hassle. 

<!-- (If I remember correctly I also tried to write 'acceptance tests') -->

But the unintended consequence of that restructuring was that codebase will be in a state of having two coexisting structures for an unknown period of time: first, the old Top-Down-style Layered sturcture, and second, the Clean/Onion-style Layered structure.

In retrospect, that restructuring was a very wrong decision. I was so naive at that time. I did not have a senior developer telling me that

> ... conceptual integrity is the most important consideration in system design. It is better to have a system omit certain anomalous features and improvements, but to reflect one set of design ideas, than to have one that contains many good but independent and uncoordinated ideas.
>
> Conceptual integrity in turn dictates that the design must proceed from one mind, or from a very small number of agreeing resonant minds.
>
> ... I will certainly not contend that only the architects will have good architectural ideas. Often the fresh concept does come from an implementer or from a user. However, all my own experience convinces me, and I have tried to show, that the conceptual integrity of a system determines its ease of use. Good features and ideas that do not integrate with a system's basic concepts are best left out. If there appear many such important but incompatible ideas, one scraps the whole system and starts again on an integrated system with different basic concepts.
> --- quote from Fred Brooks

That task of resturcturing and writing tests was later abandoned because, among many reasons, I was assigned a second project, and so did not have that much time to concentrate on that first project. And I later learned that the first project was being billed on an hourly basis. So I thought that if I continue doing the restructuring, the client might wonder why he is being billed for something he did not approve or asked for. (I was not and still am not good with sales talk)

I have digressed from the main focus of this journal entry. Back to main...

Like I said above, having successfully created integration tests, even if it only covers a very small part of our codebase for now, is a great achievement to me. Because, apart from my failures with regards to automated tests which I mentioned earlier, I have yet another experience of failed attempt at writing integration tests for another project in a different company:

I was writing unit tests, then found out that there are crucial paths in our codebase which cannot be covered by unit tests, and so needs integration tests (or acceptance/functional/end-to-end test, or test from the outside of the app). That app was written using an older ASP.NET Web API (the one which accompanies .NET Framework 4.5 or so) and, during that time there was not that many resources online on how to create integration tests for that older version of ASP.NET Web API. ANd so I struggled to create my desired integration tests. Also, I was involved with that project for 6 months only because of unforseen circumstances. So I did not have a chance of potentially finding a fix.


These days, especially if you are using .NET Core instead of the older .NET Framework, lots of hurdles have been removed for successfully implementing integration tests. Docker is now ubiquitous; and we now have Testcontainers, which can be used to create throwaway instances of Postgres for example; and LocalStack, which can be used to emulate AWS services for example.

I never thought that this time would come when I am seeing that all those time of struggles about doing unit tests or integration tests is now starting to pay off. I first encountered unit testing 10 years ago. I then encountered other kinds of testing, like acceptance testing (from GOOS) or integration testing, and characterization testing (from WELC) or approval testing. And now is a time where I can be able to apply things which I have learned over the years. 

I'm thankful to all those who have written books and blog post and videos about writing automated tests and TDD, and those who have made the tools necessary for writing integration tests.

I'm thankful to my previous bosses and clients and workmates who have been patient with me.

(murag graduation speech ba)

Most of all, I thank my God and Savior for never leaving me alone, even during those times when I wanted to be left alone; and for guiding me in living this life. May His kingdom come; may His will be done on earth as it is in heaven. May he always be with me, and with my family, and future children.
