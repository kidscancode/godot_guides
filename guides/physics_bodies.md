# Guide to Physics Bodies

## Overview

Godot offers a variety of "collision objects" to choose from. Depending on your game's design, you might need Choosing which one to use can be confusing. In this guide, we'll explore the pros and cons of each to help you make the right choice for your object.

As you read this guide, refer to the following table as a quick summary of how the various objects are differentiated:

||Moved by engine|Moved by code|
|--|--|--|
|**Collide during movement**|`RigidBody2D`|`KinematicBody2D`|
|**No collision during movement**|`StaticBody2D`|`Area2D`|

> **Note:** While 2D objects are listed here, the same principles apply to the equivalent 3D (`Spatial`) nodes.
 
## CollisionObject2D

`CollisionObject2D` is the root node for all of the bodies discussed here. These nodes can hold any number of `Shape2D` objects as children, which are used to define the object's collision bounds. You don't make this node itself, but all the node types below inherit properties from it.

### Adding Collision Shapes

In order for these objects to detect collisions and/or contacts, they must have at least one `Shape2D` assigned. The most common way to do this is by adding a `CollisionShape2D` or `CollisionPolygon2D` as a child to draw the shape directly in the Godot scene editor.

## Area2D

`Area2D` is a general-purpose node for area detection and area influence.

Common uses include detecting "enter" and "exit" events for other shapes and bodies moving in the world, altering 2D physics properties such as gravity or damping, and detection of other overlapping areas.

## PhysicsBody2D

The remaining three body types derive from `PhysicsBody2D`, an abstract base class. You will never access this class directly, only choose one of the three body types below.

### StaticBody2D

A static body is a body that is intended to remain stationary relative to the game world.

### KinematicBody2D

`KinematicBody2D` is designed for implementing bodies that are to be controlled by the user (or code).  They can detect collisions with other bodies when moving, but are not affected by physics.

When moving a `KinematicBody2D`, you cannot set its `position` directly.  Instead, you must use the `move_and_collide()` or `move_and_slide()` methods. These methods will move the body along a given vector, but will instantly stop if a collision is detected with another body.

#### Collision Response




### RigidBody2D

This is the node that implements full 2D physics. This means that you do not control a `RigidBody2D` directly. Instead you can apply forces to it (gravity, impulses, etc.), and the physics simulation will calculate the resulting movement, collision, bouncing, rotating, etc.
