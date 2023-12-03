---
title: Entity Component System Eggsblained
description: "ECS is for you n' me! :)"
date: 2023-12-02T10:37:58.771Z
preview: ""
tags: ["game engine", "c++", "architecture"]
categories: ["Programming"]
image: /assets/img/ecs/preview.png
---

## Introduction
If you're here for the juicy deets, skip to the next section(s).  

This post aims to bring forth a complete guide of an ECS implementation, in the context of a game engine. I will not go in-depth about the details of "what is an ECS", because the [first](https://www.guru99.com/entity-component-system.html) [few](https://www.simplilearn.com/entity-component-system-introductory-guide-article) [results](https://medium.com/@niitwork0921/what-is-entity-component-system-9ba3f06e9d82) on google already covers "what is an ECS". While great for someone not looking to engineer an ECS on their own, these clickbaity articles give little to no information about the actual implementation of an ECS. 
> "Uh yeah so a car has a steering wheel. It helps you turn your car. The car has an engine. That helps to move the car. Aite that's all there is to build a car good luck" - Average ECS article  

On the flip side, there are many good articles on ECS out there too. Some have code that you can refer to and use, others have pretty pictures to look at. I will try to condense all the good parts in this article.

This is not meant to be a step-by-step guide with code that can be copy-pasted and "yay it works". This guide is meant to explain the core concepts of ECS using a ELI5 (explain like I'm 5) format.  

Of course, these days, there are ECS libraries like [entt](https://github.com/skypjack/entt) and [flecs](https://github.com/SanderMertens/flecs) that you can just plonk straight into your game engine. There's pros and cons to that, but I will not cover those. Essentially, you *can* use these libraries, but you may be limited by what they can do. Or, it might fulfil all your needs and that would be great! However, from experience, I can say that the ECS is a critical component in a game engine, and if anything goes wrong with it, it helps that you would be able to debug it effectively if you have an ECS that you understand (not overly complicated). Your mileage with custom ECS or library ECS may vary in this regard.

If you're still keen on learning more about ECS, do keep reading!

## References
Why is the references here? Well, because my explanations may not be in-depth enough (ironically). For those that want MORE DEETS, here are the articles before I waste too much of your time. I'm not an expert, so here's the list of references I think are god-tier and explains stuff much better than i ever could.

[Austin Morlan's simple ECS implementation in C++](https://austinmorlan.com/posts/entity_component_system/)  
- Most popular resource for creating an ECS "back in my day".
- Disclaimer: This is a SIMPLE ECS. It is one way to implement an ECS, but it may not be the best.
- Provides lots of code snippets and implementation in C++, making it very clear how it is implemented. ~~Also makes it very easy for people to copy-pasta the code into a project with zero understanding and have no idea how to fix or modify it~~
- Lacks PiCtUrEs and has too many words so my smooth brain struggles to read it to the end. Not the author's fault, I just ate too much glue.

[Hexops ECS implementation in Zig](https://devlog.hexops.com/2022/lets-build-ecs-part-1/#next-up-starting-our-ecs-implementation)  
- Has (some) pictures! And code snippets!
- But it's coded in Zig (never heard of that language tbh I'm just a hermit crab)
- Explains the thought process and reasoning for using an ECS architecture very well
- Has more links to more resources!

[Coding With Thomas's C++ ECS](https://www.codingwiththomas.com/blog/an-entity-component-system-from-scratch)  
- Extremely bare bones ECS. Zero bells and whistles, just arrays of components and an entity.
- Not suitable to be used in an actual project, but is great for understanding how ECS stores stuff.

[Tobs' C++ Entity Component System](https://gamedev.net/articles/programming/general-and-gameplay-programming/the-entity-component-system-c-game-design-pattern-part-1-r4803/)  
- IMO the most content-rich article on ECS I found so far, covering the use of C++ templates, custom memory allocation, logger and event system
- Too much details for "my first ECS", but a great resource for advancing your ECS to the next level
- Many technical pictures to help visualize the architecture

[Unity's ECS architecture](https://docs.unity3d.com/Packages/com.unity.entities@0.2/manual/img/ecs_core.html)
- Advanced way of organizing components in the memory level
- Too advanced for my smooth brain to implement (maybe one day)
- Nice pictures to explain how they organize components in memory to achieve **MAXIMUM SPEED**

## Prerequisite Knowledge
- Basic to Intermediate knowledge of some data structures (Arrays and stuff)
- Somewhat good understanding of lower-level concepts of computer memory (contiguous memory good)
- Basic understanding of what it takes to make code run faster
- Intermediate to advanced C++ knowledge, including pointers and templates.
- Basic knowledge of how game engines work (game objects, components, decent knowledge in game engines)
- A brain and some thinking capabilities

## The Problem
Aite so you are tasked by your professor to "just make a game engine".  
Ok, sounds great.  
Being the genius you are, you proceed to work on the engine, and soon you are faced with multiple issues:
- How are you going to represent Game Objects/Entities in your code?
- How are they going to be stored and accessed?
- What kind of data do the entities need to store? Would you be adding more data as you develop the engine?
- How are entities going to store data?

## Naive solution
Let's start with a class that holds data for one entity. We'll throw in some irrelevant data for now.  
![ent](/assets/img/ecs/entityclass.svg){: .center }  
We'll make an array of entities from this class. Tada, we can represent entities in the game!  
But wait, that's not useful. We have barely any data to work with. Let's throw in position data that represents the x, y and z coordinates.  
![ent2](/assets/img/ecs/entityclass2.svg){: .center }  
Cool. All entities will now have position data. That's fine for now.  
Now, I want to add physics data, let's say, velocity in the x, y and z directions. Oh, and I'd want to give them sprites too. Oh, some of them needs to have a rotation and scale too. Let's slap them all into the class.  
![ent3](/assets/img/ecs/entityclass3.svg){: .center }  
By now, we have a very basic (and naive) architecture that holds data of entities. However, some issues will arise very soon.  
We will assume that Entity1 is our player, and is the only thing that moves in the game.
1. **Waste of space.** Only 1 entity in our entire entity array is using the Velocity data. This means that the Velocity data in every other entity is a waste of space.
2. **Waste of time.** If we want to process Physics on all of these entities, we would have to loop through all the entities and grab their Velocity data to do some calculations. However, how do we tell that *only* Entity1 would require this processing? We'd have to look through all of the entities, and rely on some condition to check if they need to process Physics or not. In any case, we'd have to loop through _every_ entity, which is a huge waste of computing time.
3. **Not Scalable.** Adding and removing data from entities will soon become a monster to manage as more features are added to the engine. You would have to fit Rendering data, Physics data, Animation Data, Script data, and so on, into the poor Entity class. The entity class may swell up to an incredibly huge class. At this point, your performance will suffer, and your team trying to implement features will cry from the ~~absolute chungus~~ disgusting monster of a class your Entity has become.  

What do? Make a modular architecture to handle all that data! ECS is the perfect architecture to solve all of the aforementioned issues!

## Entity Component System: Eggsblained
The core concept of ECS is to be able to split the [Entity](#the-entity-pt-1) and the [Component](#the-component) (a bunch of data together) apart. [The System](#the-system) is also an essential part of the architecture, but we will go through that in detail in a later portion.
![ent-comp](/assets/img/ecs/entity-component.png){: .center }  
We will separate and group common data together to make a **component**, and split off the **entity** portion to be on its own. The final idea we are working towards will be to store all these data into multiple smaller arrays instead of one huge Entity Monster Array. Just by doing so, we can improve on the [spatial locality](https://en.wikipedia.org/wiki/Locality_of_reference) of data, slightly improving cache performance.

### The Entity (Pt 1)
"But Azoor", you may ask, "what data is in the Entity portion?" Great Question! I don't know! (because it depends on your implementation!)  
If you have looked around online, articles say "[the entity is a tangible thing](https://www.simplilearn.com/entity-component-system-introductory-guide-article)" or "[has not actual data or behaviour](https://www.guru99.com/entity-component-system.html#2)". If you went "huh?", don't worry bud, me too!  

The general idea of an Entity is that Entities do not carry data. However, this is counter-intuitive for someone with a background in using Unity. "Where does the entity name go? Where does the IsActive bool go? Where does entity-specific data go??" Right now, let's not worry about the data in Entity. It will be explained in [The Entity (Pt 2)](#the-entity-pt-2).  

Let us assume we have 10 entities in the game. We can label them one by one, using the indices 0 to 9.  
![ent-comp](/assets/img/ecs/entity-array.svg){: .center }  
Great! But they have no data. That's no good. These guys right now are just imaginary numbers and nothing more, they don't exist anywhere. Not in an array, not in the cloud, it exists only in your head... Hold this thought for now, it will (hopefully) make sense in the next section.  
Just know that we have 10 numbers to dish out, 1 for each entity, but they don't live anywhere but in your head.

### The Component
Alright, this is the real meat of the entire system! Each component is a *collection of data* that are grouped together based on function. In other words, we group data with similar purpose together. Data like Position, Rotation and Scale all describe an Object in 2D/3D space, hence they can be grouped together into a component called "Transform". Rinse and repeat for all other data, and you have a bunch of components!  

Now throw them all into an array

### The Entity (Pt 2)

### The Entity and Component Together

### The System

## Applications
## Further optimizations

