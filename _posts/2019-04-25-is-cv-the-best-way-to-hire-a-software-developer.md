---
title: Is a CV the best way to hire a software developer?
date: 2019-04-25 00:00:00
featured_image: "/images/cv-resume/featured.jpg"
excerpt: I've been on both sides of the table as an applicant and as an interviewer. In both cases I cannot help but feel I'm engaging in a bullshit process that supposedly is meant to either exemplify ones experience or vet them. Surely there must be a simpler way.
---

![Photo by Clem Onojeghuo on Unsplash](/images/cv-resume/featured.jpg)

I really hate doing things I know are borderline bullshit just because they are conventional wisdom. CVs are definitely one of them. First of all, biases always find their way around a CV. Age, sex and name are just some of the information we now reduct or obfuscate in order to remove certain biases. I would argue we need to deduct the whole CV in order to be somewhat impartial and effective. But then, you might ask, how do we vet people if not through a CV? How would you know about their background, skills and experience if you don't read their CV?

Let me save you some time. People exaggerate so what you see on a CV is a stretch of their actual experience. The sad thing is everyone knows this. But I'd like to examine a couple of *sections* on the CV bit closer that reflect my own biases when I'm reading someone's CV.

### Education
Cambridge University. I can hear myself thinking, "Cool this is a smart person".

University of x. "Meh, I'm not sure about this university. I have heard it before; maybe it's good, maybe it's not".

"Politecnico di Milano", I can hear my brain shouting "This is not even a British university! How do I even begin to find out if it's good or not?! I will not bother checking it out".

Bootcamp. Well for sure they are not experienced enough. How could they possible know about garbage collection cycles". For the record, I have no idea about garbage collection cycles apart from that blog post I read 2 years ago on Hacker News. So, you can call me an expert I guess.

So, 4 humans but only 1 seems to be getting a 🥇. Why is that? People often think that discounting university degrees is an attack on formal education. That's not the case. Maybe it's an attack on elitism actually. It's not about making candidate number 1 more but rather why are the other 3 candidates less?

In our great and diverse industry, there are plenty of developers that didn't even study Computer Science. Some of them are the best I know. An example is my good friend [Txus](https://twitter.com/txustice).

So how about we drop that section completely? It takes a bit of a real estate on paper as well.

### Experience
Fasten your seat belts for this section. Mind the gap as you enter the platform.

When I was at Deliveroo, my CV will probably say, I worked on Kafka to decouple a monolith into microservices with a throughput of 1 billion messages a nanosecond. True story. Actually, partially true. I did work at Deliveroo. And I have used a thing called Kafka. It was provided as a library of course so I was able to call `Kafka.please_publish` and `Kafka.please_consume_without_dying_because_i_have_no_idea_what_i_am_doing`. I have no idea about the internals of Kafka, what partitions are, how to achieve ordering or how to replay events. *Even this is not necessarily true because I was involved with Kafka at carwow quite a bit 😅.*

You are probably thinking right now, "I will be able to uncover that on the CV". Unless the job is to work on Kafka itself, no you won't. Most likely the applicant would have read a bit more about Kafka the night before and know most of the terminology.

Just because someone worked at a very well known company, doesn't make them experts in the technology they are claiming they know. Software is about abstractions. Sometimes we don't see the internals of the technologies we work on. That's even more true for bigger companies as dedicated teams take care of those pieces. In the case of Deliveroo and Kafka, the Data Engineering team had complete control of it.

This is not to say that working on Kafka even on that capacity does not make it a valuable experience. Abstractions are great, that's how we get things done instead of re-inventing the wheel. But why bother, if this is something that can be taught on the job?

The **Experience** section has also another problematic side to it. It's more or less the same as the education. I guarantee you stop skimming through a CV when you come across Twitter, Facebook, Google and Amazon. But not when the name of the company does not ring a bell. There are a plethora of businesses that are doing excellent work, it's just not that fancy for TechCrunch. Even consultancies: everyone knows ThoughtWorks (for good reason) but what about a non-known consultancy? Does that mean the developer hasn't produced quality work? Again, is not about being an engineer at Twitter is less but why being an engineer at x is not more?

### Buzzwords
This is a simple one. As applicants, we are trying to fill CV with the latest cool technologies and words that will make the recruiter look at the CV. No, I hadn't had the chance to work with Go prior to joining Deliveroo. How long do you think it took me to contribute production code? With the right support system (thanks Antonio et al) I was productive in Go from day 1.

### So what's the answer you might ask?
To be honest, I'm not 100% sure but I tend to find more value in conversational CVs or cover letters as they are called I suppose. There are a lot of benefits, I believe, in a story as opposed to a list of items.

#### Value in technical writing
Often people say, programmers read more code than they write. This is absolutely a true statement. The way in which one writes an email, a technical spec or documentation often mirrors their normal writing. A cover letter can highlight the technical writing skills of somone, how they lay out their thoughts. I find that to be an important attribute of any developer I enjoy working with.

There you go, that's your first gate keeper. If someone puts effort and thought in a well written cover letter, it is likely they do the same for their code. A CV is just an incoherent bullet point list of things with no expressional value.

#### Lack of personal identity
There is nothing more boring than trying to think bullet points for each company so you can showcase you've done something. It's just a cold and impersonal way of trying to highlight your experience. Instead, ask the candidate to answer a few questions?

> What have you worked on that you are the most proud of?
Which aspect of a technology / business have you personally took ownership for and why?
What do you enjoy working on the most? What would you like to work on here?

Just a few simple questions to get the creative juices flowing. People will come up with their own, unique way of expressing themselves. They might send you commits to open source projects that are super proud of, how the debugged a complicated memory leak (which wouldn't go on the CV) or how they helped grow their colleagues. All of which are way more interesting and important than a bunch of bullet points.

### Conclusion
Even though I truly believe this is a better way to hire both the the company and the candidate, I don't see changing it in the near future. One might say, "Well, that's what I do with a CV. I then call the candidate and find out more information". If you find that easy, great. I just don't think there are any actionable information on a CV. On the other hand, having the candidate explaining what drives them, what they have *actually* worked on and are proud of provide you with a wealth of information to drill down more and learn about them.

I will probably run the above as an experiment at [CreditDigital](https://www.creditdigital.co.uk) and see the results.
