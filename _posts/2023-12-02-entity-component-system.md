---
title: Entity Component System
description: "ECS is for you n' me! :)"
date: 2023-12-02T10:37:58.771Z
preview: ""
tags: ["game engine", "c++", "architecture"]
categories: ["Programming"]
image: /assets/img/ecs/preview.png
---

## **Introduction**
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
- Disclaimer: This is a SIMPLE ECS. It is one way to implement an ECS, but it may not be the best.
- Most popular resource for creating an ECS "back in my day". The "basic b***h" of ECSes.
- Provides lots of code snippets and implementation in C++, making it very clear how it is implemented. ~~Also makes it very easy for people to copy-pasta the code into a project with zero understanding and have no idea how to fix or modify it~~
- Lacks PiCtUrEs and has too many words so my smooth brain struggles to read it to the end. Not the author's fault, I just ate too much glue as a kid.

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

## **Prerequisite Knowledge**
- Basic to Intermediate knowledge of some data structures (Arrays and stuff)
- Somewhat good understanding of lower-level concepts of computer memory (contiguous memory good)
- Basic understanding of what it takes to make code run faster
- Intermediate to advanced C++ knowledge, including pointers and templates.
- Basic knowledge of how game engines work (game objects, components, decent knowledge in game engines)
- A brain and some thinking capabilities

## **The Problem**
Aite so you are tasked by your professor to "just make a game engine".  
Ok, sounds great.  
Being the genius you are, you proceed to work on the engine, and soon you are faced with multiple issues:
- How are you going to represent Game Objects/Entities in your code?
- How are they going to be stored and accessed?
- What kind of data do the entities need to store? Would you be adding more data as you develop the engine?
- How are entities going to store data?

## **Naive solution**
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

## **Entity Component System: Eggsblained**

ECS is an amazing architecture that provides multiple solutions:
1. A nice storage solution for all data to be used by all entities
2. A modular way to add and remove data on a per-entity basis
3. Cache-friendly iteration over data when trying to process data

The core concept of ECS is to be able to split the [Entity](#the-entity) and the [Component](#the-component) (a bunch of data together) apart. [The System](#the-system) is also an essential part of the architecture, but we will go through that in detail in a later portion.
![ent-comp](/assets/img/ecs/entity-component.png){: .center }  
We will separate and group common data together to make a **component**, and split off the **entity** portion to be on its own. The final idea we are working towards will be to store all these data into multiple smaller arrays instead of one huge Entity Monster Array. Just by doing so, we can improve on the [spatial locality](https://en.wikipedia.org/wiki/Locality_of_reference) of data, slightly improving cache performance.

### **The Entity**
You may notice that I split "Entity" away from the data.  
What does this mean? What is Entity?  
Entity is nothing. There is no data in the Entity block. Yet, the Entity is there, in spirit. Like a ghost. Observing the creature covered in flesh beyond the screen, head in arms, trying to understand what it means to be present but non-existent. 

Moving on.

The general idea of an Entity is that Entities are akin to an "Identification Number".  
Let's say we are a factory, building Swiss Army Knives.  
![ent-comp](/assets/img/ecs/swissknife-full.jpg){: .center }  
We are to build 10 of them, but each one is a custom order with custom parts. The customers are peculiar, and they want many combinations of these parts.  

![ent-comp](/assets/img/ecs/swissknife-full.jpg)
_Some may want the full set.._  

![ent-comp](/assets/img/ecs/swissknife-two.jpg)
_...and some may only want a few parts_  

![ent-comp](/assets/img/ecs/swissknife-nohandle.jpg)
_... and some may not even want the handle!_  

We start out our production with 10 slots for these... products. Let's label them with indices, from 0 to 9.
![ent-comp](/assets/img/ecs/entity-array.svg){: .center }  
The idea is that, each "Entity" is an Index to identify the group of "components", in this case, collection of parts.  
We can refer to each specific product using this Index, and that represents an "Entity".  

This raises some more questions:
> What if there are _no_ parts in that specific factory slot? In terms of ECS, what if there are _no_ components in an entity?  

Some may argue that the entity still exists, but has no data. Others may argue that this is impractical in an actual implementation, and that the entity should not exist. It depends.

In my implementation of ECS, I want every entity to have _some_ data, like the Entity Name and stuffs. It makes more sense for me, coming from a background in Unity. This means that each Entity _must_ come with some data stored somewhere! So where should that data be, if the "Entity" should not contain any data?? I'll explain this in the [Understanding The Entity Component](#understanding-the-entity-component) section below. Hint: it's in a component!

### **The Component**
Alright, this is the real meat of the entire system! Each component is a *collection of data* that are grouped together based on function. In other words, we group data with similar purpose together. Data like Position, Rotation and Scale all describe an Object in 2D/3D space, hence they can be grouped together into a component called "Transform". Rinse and repeat for all other data, and you have a bunch of components!  
![ent-comp](/assets/img/ecs/entity-split.svg){: .center }  

Notice that we have split the huge Entity class into 4 distinct components now, and the Entity class is no more. 

#### **Understanding The Entity Component**
However, you may notice that I have dumped the Entity related data into "Entity Component". "So is that the entity?" you excitedly ask, light gleaming in your eyes. I put on my most condescending tone, and with my index finger up in the air, I let out the words: _"Erhm, ackchyually hor..."_  

I apologize for that. Moving on.  

The Entity is a collection of components. Without Components, an Entity does not exist. (because it physically has nothing!)  
![ent-comp](/assets/img/ecs/entity-spirit.svg){: .center }  
This kind of means that as long as there is 1 component, an entity should exist.  
![ent-comp](/assets/img/ecs/entity-spirit-tsfm.svg){: .center }  
However, in practice, this isn't very practical in a game engine.

#### **Understanding The Regular Component**

#### **Components in an array**

### The Entity (Pt 2)

### The Entity and Component Together

### The System

## Applications
## Further optimizations

