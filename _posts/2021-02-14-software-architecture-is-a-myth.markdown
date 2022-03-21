---
layout: post
title:  "Software architecture is a myth!"
date:   2021-02-14 18:00:00 +0100
categories: architecture
---
> Architecture is a term that lots of people try to define, with little agreement. There are two common elements: One is the highest-level breakdown of a system into its parts; the other, decisions that are hard to change. -- Patterns of Enterprise Application Architecture, Martin Fowler

Have you ever looked up the definition of software architecture? Chances are you’ve found more than one. It’s something about core elements and their relationships, or everything that’s important, or anything that’s hard to change later. I fully agree with the structural part of it; software architecture is about elements and their relationships. It’s about creating and naming these elements, defining their responsibilities and borders, and arranging them.

I struggle to agree with the everything important or anything hard to change parts. These aren’t definitions but rather generalising and subjective statements. What’s important? What’s hard to change? Does everything important and anything hard to change belong to the architecture? I can see where it’s going, but it’s not quite enough for a definition.

> The fundamental organization of a system embodied in its components, their relationships to each other and to the environment, and the principles guiding its design and evolution
-- IEEE 1471

According to IEEE’s definition, software architecture isn’t only about structural aspects, but also about principles guiding software development. This is an often overlooked part of software architecture, because we are focused on all the technical aspects. We forget that the architecture is a framework within which developers collaborate on a daily basis. A framework helping us to communicate, make decisions and closely work together. So basically, software architecture is a myth. 

# It’s a myth!

A myth? This may sound strange. Let me explain… As presented in Sapiens: A Brief History of Humankind, telling myths enables cooperation in large groups, because humans are not equipped to cooperate in that way – we lack the instincts for it.

> […] humans evolved for millions of years in small bands of a few dozen individuals. The handful of millennia separating the Agricultural Revolution from the appearance of cities, kingdoms and empires was not enough time to allow an instinct for mass cooperation to evolve. -- Sapiens: A Brief History of Humankind, Yuval Noah Harari

> We believe in a particular order not because it is objectively true, but because believing in it enables us to cooperate effectively and forge a better society. --
Sapiens: A Brief History of Humankind, Yuval Noah Harari

What makes us work together is a shared belief in the same myths. These myths help us to find a common understanding of our environment – they shape our world view. This was as true when we were hunters and gatherers as it is today. In a similar way, software architecture shapes our way of thinking about a software system, just as myths shape how we think about the world.

# Architectural myths

In software projects we tell myths about development methodologies, organisational structure, software architectures and so on. More often than not, the myths we decide to tell are subjective. We are influenced and limited by our knowledge, experience, environment and habits. Therefore, the order we create by telling these myths is both subjective and imagined. There is nothing bad about this, but it is important to realize, that it is the shared belief which enables us to cooperate.

Just imagine the opposite; a software project is approached without a shared architectural belief. Some developers are used to a monolithic style, others strongly believe in microservices, and a few of them want a fat client. In this scenario there is no common theme or no pattern that could guide the developers. I’ll leave the endless discussions, regular misunderstanding and frustrating project navigation to your imagination.

So software architecture is not only about the technically best way of doing something. Does the team understand the architecture? Does the team believe the architecture to suit the project? Is the team capable to implement the architecture? These are some of many non-technical concerns that should be respected by architects. Ignoring these concerns will limit the acceptance and feasibility of an architecture and consequentially pose a risk to project success.

# Evolving myths

Change is the only constant in software projects. Naturally, software architectures cannot escape from this – it creeps into the architecture just as into anything else. Change requires us to reshape the way we think about a software system – evolving the myths we tell each other.

Software architecture isn’t just a collection of diagrams with explanatory texts. Sure, deployment and component diagrams are all fine, but they are just the big picture – or big boxes if you like. Architectural details can’t or won’t be documented in big boxes with arrows between them. Therefore, software architecture is in our heads – it’s the myths we believe and tell each other.

Understanding, or more precisely a continuously, growing understanding, may result in a desire or need to change. Initial architectural decisions are based on the initial understanding of the problem domain. It’s impossible for a customer to formulate and for an architect to understand the problem domain in its entirety at project start. Only over time, when we experience and work with the software product, our understanding of the problem will improve, giving us the chance to feed back new insights into the architecture.

On the other hand, loss of understanding may trigger change, too. Developers leaving the project will take their understanding with them, they will no longer participate in telling and reinforcing the architectural myths. This is especially hard for key developers – so to say the strongest believers. They were able to tell the myths back and forth, by that providing stability to the architecture. The void left behind may – for better or worse – be filled with new ideas, which in turn evolve the architectural myth.

Change can both be good or bad. However, the challenge in adapting the architecture is to do it collectively. Repeating from above, it is the shared belief that makes us work together. Detecting change that happened unknowingly, is happening, or will happen is best made explicit, giving room for discussion and realigning the team’s understanding of the architecture. Otherwise, implicit change will gradually increase the disparity of the architectural myths a team tells.

# Takeaway

Generally speaking, effective cooperation is built upon a shared belief in myths. In software projects, it’s not so different, just the myths we tell vary. Working on a software project we should remember that we need to:

- be deliberate and explicit about software architecture. There mustn’t be doubt about the myths we tell each other. An implicit architecture or just having a vague idea about it will limit cooperation, because this makes it impossible to believe in the same thing.

- appreciate that myths evolve. Over time, we gather experience and knowledge which we feed back into our previous decisions. Team constellations and architectural drivers change over time. So it’s natural that architectural myths will evolve, too! Make sure everyone stays up-to-date or end up with different beliefs.

- understand that architecture isn’t just structural, but also a framework guiding the choices developers make daily. The technically best architectural style is useless if it is not well understood by the individuals working on the project. Perhaps, some styles don’t work with some teams or require initial training.

So it’s not just boxes and arrows. It’s not just meant to attack technical problems. Software architecture solves an important social problem of developers: effective collaboration. It’s one of the key drivers of working together in a software project.