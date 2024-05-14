---
title: Entity Component System (Part 1)
description: "ECS is for you n' me! :)"
date: 2023-12-02T10:37:58.771Z
preview: ""
tags: ["game engine", "c++", "architecture"]
categories: ["Programming"]
image: /assets/img/ecs/preview.png
pin: false
---
# What is ECS
Entity Component System (ECS) is a way to organize data for objects with modularity, expandability and performance in mind. 

## Dealing with many objects
Imagine having many objects in a game. Each object will have to contain some amount of data to work with to make a game work (e.g. Position, rotation, velocity etc.). How do we store this data for each object? We could simply create a class that contains all the data we need, and make an array of that.

This is a quick and simple solution. However, problems will soon arise as the game becomes more complex. More data will have to be added to that one object class, making the object class larger in size, sharply increasing the total size of the array. With a large amount of game objects, memory consumption quickly becomes a huge issue.

## Flaws of a naive solution
An obvious flaw in this simple method would be that not every game object needs every piece of data. Some objects require some bits of data and not others, and other objects may require a different mix of data.  For example, an object meant to be a wall will require collision data, but not physics data. A ball object that can bounce around would need both collision data and physics data. This leads to much wasted memory space for game objects that use only a tiny fraction of the data, which can lead to other performance related issues too.

## Component System Explained
How do we improve this? The concept of a Component System comes in useful here. A component system will allow the addition and removal of bits of data from an object as needed, so the user can mix and match the types of data each individual object should have. Relevant bits of data are grouped together into a component, hence the name component system. In game engines, relevant data are grouped together by function. For example, position, rotation and scale data may be grouped together into a component called transform, velocity and related physics data can be grouped into a component called rigidbody etc.

## Component System & ECS
So, what's ECS exactly? ECS is a specific way to implement a component system. There may be many ways to implement the generic idea of a component system, and ECS is one effective implementation of a component system.

# General Concept of ECS
### Components
Optimizing the way data is stored is the unique feature of ECS. Components of the same type are stored together in contiguous memory blocks, usually in an array or vector. The data must be packed such that there is no "empty" or unused array slots in between components in-use. The benefits of packed data will be apparent later.

There will be multiple component arrays, each containing components of the same type. It would be good to have a management system to handle, add and remove all of these arrays easily. Templates come in handy here but are not required.
### Entities
Entities are, by simplest implementation, unique indices that helps to identify which component in the component arrays belong to which entity. For example, you can have an entity with an index of 369. This value on its own isn't required to be stored anywhere. When something wants to grab all the data related to the entity 369, all of the component arrays will be "asked" if they contain data for entity 369. If there exists data for entity 369 in a component array, that component is provided. If that component array does not have data for entity 369, then it means this entity does not contain this specific component. Rinse and repeat this procedure for all component arrays, and you will end up with all components belonging to this entity.
>The process of asking all component arrays if they have a specific component can get slow. It is possible to store what component types this specific entity is using in the entity itself, so that only relevant component arrays will be looked up. Performance benefits may vary depending on implementation. Profile and debug to optimize the performance!

Entities take up no data space, but it would be nice to have somewhere to store entity-related data (e.g. name, guid, etc.). This can be accomplished via storing this data as a component or something similar.
### Systems
Systems are "modules" that operate logic on a set of data, usually from an array of component data. Systems usually do not contain data themselves, systems only work on existing data of each entity.

An example to illustrate this would be a Physics system. It would do the simple job of applying gravity to all entities in the game. Here's how a System should interact with data:
 1. Every frame, the Physics system will request for the component array that stores Transform data.
 2. Once it has the array of Transforms, the system will iterate over this array and retrieve the Position value from each Transform component.
 3. The Y value of each entity will be modified. Perhaps reduce the Y value by 0.1 with each frame.
 4. The updated position value is written back into the component. This process is repeated for every component in the Transform component array.
 There are a few points of interest to note here:
 - While iterating over the component array, there are no "skips" over empty component slots. All components currently in the front portion component array are valid, because the component array is packed. This reduces the number of iterations required. Unused component slots should be at the tail end of the array, and the system should never have to iterate over those.
 - Doing the same process on multiple components that are stored contiguously greatly increases performance (usually). This method is super cache-friendly.

Thus, the System, when accessing components this way, provides a huge performance benefit. However, if the System requires more than one type of component, this benefit takes a hit in performance. 

For example, if your Physics system needs to access collision data in a Collider component, along with the data from the Transform component, your system would have to access data in another component array. Your system may iterate over the Transform component array, and for each Transform component, the system would have to request for the Collider component array and locate the _specific_ component that corresponds to the same entity. 

Locating the specific Collider component of the desired entity can be mitigated by using a lookup table to avoid long iterations. However, the spatial data locality would be degraded, as the other component array may not be in close proximity to the initial array. However, the performance impact may be minimal enough that this can be a non-issue. Profile your implementation to know for sure if this is an area to optimize.
>[Unity's ECS implementation](https://docs.unity3d.com/Packages/com.unity.entities@0.2/manual/ecs_core.html) looks into this issue by grouping often-used component types together and making an array out of these groups, called Archetypes. This eliminates having to jump too far away in memory space when accessing different component types.

In conclusion, systems are the "processors" that take in data from components and does some logic on them, and then writes back the result into the same component or another component.

# ECS Implementation
## Prerequisites
This guide assumes you have adequate knowledge in these things:
- Intermediate C++ experience
- Knowledge to set up your own C++ project and workflow chain (MSVC, gcc, etc.)
- C++20 (not strictly required but guide may use C++20 features)
- Familiar with C++ containers (vectors, arrays, maps)
- Fair knowledge in basic memory optimization (spatial locality is good)
- Good to have: C++ template usage
The guide will be in C++, but it is possible to implement the ECS concept in other languages too.

## The big picture
Our ECS system will look something like this:
- ECS Manager
- Component
- xxx

# ECS applications
Typically, it's used in game engines, where most game objects have a different mix of components. ECS can be used outside of game engine applications that require this unique trait.

# References & Further Reading
[Austin Morlan's simple ECS implementation in C++](https://austinmorlan.com/posts/entity_component_system/)
- Popular resource with example code in C++ for a basic ECS implementation
- Requires template knowledge
- Not the best implementation. It's intended to be a starting point, not a copy-paste template.
- Lacks diagrams

[Hexops ECS implementation in Zig](https://devlog.hexops.com/2022/lets-build-ecs-part-1/#next-up-starting-our-ecs-implementation)
- Includes some diagrams
- Implemented in Zig. Not a very common language but it proves that the ECS concept can be used in other languages too.
- Explains the thought process and reasoning for using an ECS architecture very well

[Coding With Thomas's C++ ECS](https://www.codingwiththomas.com/blog/an-entity-component-system-from-scratch)
- Extremely bare bones ECS. Zero bells and whistles, just arrays of components and an entity. Simple to understand.

[Tobias Stein' C++ Entity Component System](https://www.gamedeveloper.com/design/the-entity-component-system---an-awesome-game-design-pattern-in-c-part-1-)
- Most in-depth article on ECS in my opinion
- Advanced guide covering memory management, logging, object pooling and more
- Includes diagrams
- Too complicated as an introduction to ECS, but a great resource nevertheless
