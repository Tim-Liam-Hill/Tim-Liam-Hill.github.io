---
layout: post
author: Timothy Hill 
title: Recreating Data Structures in C++ - Part 2
---

Oh the commitments we make when we are full of energy and motivation. 

As it turns out, coding data structures can be fairly tedious (especially in a language like C++). That's not to say I am not enjoying myself, but I am finding that I am not 
particularly in the mood to write unit tests for finished classes and have opted to begin coding a new data structure (Vector) instead of 
just finishing the tests for my current data structure (LinkedList). It seems as though the process of creating of something new is more
enjoyable than the process of proving the created thing is 100% correct (which shouldn't really be surprising).

### A Philosophy on Testing 

I imagine many people have tackled this topic before from the angle of 'Would you want to fly in a plane that only passed 99% of tests???'
so it seems pointless to beat that particular dead horse. Any programmer worth their salt will agree that testing is a necessary part of 
any worthwhile workflow and that no code should be pushed into production before it passes a complete set of rigorous  tests.
What I think is worthwhile to focus on however, is the development of good coding habits.

The temptation in our personal projects would be to bypass the workflows set in our jobs that we see to be tedious and boring. While 
not all of these things may be necessary for pet projects, it is always beneficial to examine the habits we are building in our projects 
and whether or not those habits are conducive to the end goal we have in mind. In short, nobody wants a bug-ridden project that requires 
extensive refactoring before it can be finished but that can definitely become a reality if shortcuts are taken (eg: no unit tests, no diagrams,
systems that are too tightly coupled etc). 

The tradeoff here is likely that, sometimes we just want things to be fun but taking all the necessary steps isn't always fun. What is the point of starting a pet project if you lose all motivation for it due to the extensive testing, documentation, planning etc that you feel need
be done to make it successful? In such a case I would argue that a finished, imperfect project is better than an unfinished project or no project at all.  

Perhaps a nice way of thinking about it is the notion of consistent forward progress. Pet projects don't have to be so serious. Perhaps we can find a balance between the tasks that are enjoyable and those that are tedious such that step by step we still get closer to our goal. I would liken it to a long distance race (a marathon or ultra-marathon): there are some things that really shouldn't be skipped such as your plan for hydration, stretching, wearing shoes etc. But there are other things that offer minimal marginal benefit for the vast majority of us that might actually make the process less enjoyable (like fighting past throngs of people just to be at the front at the start of the race). 

Is this a good analogy? I am not sure, but since I wouldn't enjoy proof-reading this article nor do I think the marginal benefit gained by proof-reading this article justifies the discomfort I would experience, I think I will just publish it as is and hope that my point has been made. 

Feel free to take a look at this project [here](https://github.com/u17112592/DataStructures). I am open to criticisms and suggestions on how to improve. 

