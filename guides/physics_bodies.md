# Guide to Physics Bodies

## Overview

Godot offers a variety of "collision objects" to choose from. Choosing which one to use for your project can be confusing. In this guide, we'll explore the pros and cons of each to help you make the right choice for your object.

As you read this guide, refer to the following table as a summary of how the various bodies are differentiated:

||Controlled by Physics Engine|Controlled by code|
|--|--|--|
|**Collide during movement**|`RigidBody2D`|`KinematicBody2D`|
|**No collision during movement**|`StaticBody2D`|`Area2D`|

> **Note:** While 2D objects are listed here, the same general principles apply to the equivalent 3D (`Spatial`) nodes.
 
## CollisionObject2D

`CollisionObject2D` is the base class for all of the bodies discussed here. You don't use this node directly, but all the node types below inherit properties from it.

### Adding Collision Shapes

`CollisionObject2D`s can hold any number of `Shape2D` objects as children. These shapes are used to define the object's collision bounds and detect contact with other objects. In order to detect collisions and/or contacts, at least one `Shape2D` must be assigned. The most common way to do this is by adding a `CollisionShape2D` or `CollisionPolygon2D` as a child. These nodes allow you to draw the shape directly in the editor workspace.

### Collision Layers and Masks



## Area2D

`Area2D` is a general-purpose node for area detection and area influence.

Common uses:

-   detecting "enter" and "exit" events for other shapes and bodies moving in the world
-   altering 2D physics properties such as gravity or damping
-   detection of other overlapping areas

## PhysicsBody2D

The remaining three body types derive from `PhysicsBody2D`, an abstract base class. You will never access this class directly, only choose one of the three body types below.

### StaticBody2D

A static body is a body that is intended to remain stationary relative to the game world. It participates in collision detection, but does not move or rotate after colliding.

StaticBodies are most often used for objects that are part of the environment, such as platforms or walls.

### KinematicBody2D

`KinematicBody2D` is for implementing bodies that are to be controlled by the user (or code).  They can detect collisions with other bodies when moving, but are not affected by physics.

When moving a `KinematicBody2D`, you cannot set its `position` directly.  Instead, you must use the `move_and_collide()` or `move_and_slide()` methods. These methods will move the body along a given vector, but will instantly stop if a collision is detected with another body.

#### Collision Response

After a KinematicBody2D has collided, it is common to calculate some kind of _collision response_. You may want the body to bounce, to slide along a wall, or to alter the properties of the object it hit. The way you handle collision response depends on which method you used to move the KinematicBody2D.

When using `move_and_collide`, the function will return a `KinematicCollision2D` object, which contains information about the collision and the colliding body.

Example:
```
func _physics_process(delta):
    var collision_info = move_and_collide(velocity * delta)
    if collision_info:
        var collision_point = collision_info.position
```

### RigidBody2D

This is the node that implements full 2D physics. This means that you do not control a `RigidBody2D` directly. Instead you can apply forces to it (gravity, impulses, etc.), and the physics simulation will calculate the resulting movement, collision, bouncing, rotating, etc.
