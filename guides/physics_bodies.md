# Guide to CollisionObjects

## Overview

In game development you often need to know when two objects in the game space intersect or come into contact. This is known as _collision detection_. When a collision is detected, you typically want something to happen. This is known as _collision response_.

Godot offers a number of collision objects to provide both collision detection and response. Trying to decide which one to use for your project can be confusing. You can avoid problems and simplify development if you understand how each each works and what their pros and cons are.

In this guide you will learn:

-   Godot's four collision object types
-   How each collision object works
-   When and why to choose one type over another

## CollisionObject2D

> **Note:** While 2D objects are described here, the same general principles apply to the equivalent 3D (`Spatial`) nodes. 

`CollisionObject2D` is the base class for all of the bodies discussed here. You don't use this node directly, but all the node types below inherit properties from it. The most important property it provides is the _collision shape_.

### Collision Shapes

`CollisionObject2D`s can hold any number of `Shape2D` objects as children. These shapes are used to define the object's collision bounds and to detect contact with other objects. 

> Warning: In order to detect collisions, at least one `Shape2D` must be assigned to the object.

The most common way to assign a shape is by adding a `CollisionShape2D` or `CollisionPolygon2D` as a child of the object. These nodes allow you to draw the shape directly in the editor workspace.

> Note: Be careful to never scale your collision shapes in the editor. The `Scale` property in the Inspector should remain `(1, 1)`. When changing the size of the collision shape, you should always use the size handles, _not_ the `Node2D` scale handles. Changing the scale can result in unexpected collision behavior.

!player_coll_shape.png

### Collision Layers and Masks

One of the most powerful but frequently misunderstood collision features in Godot is the collision layer system. This system allows you to build up very complex interactions between a variety of objects. The key concepts are _layers_ and _masks_. Each CollisionObject2D has 32 different physics layers it can interact with. 

Let's look at each of the properties in turn:

-   **`collision_layer`**
This describes the layers that the object appears _in_. By default, all bodies are on layer `1`.
-   **`collision_mask`** 
This describes what layers the body will _scan_ for collisions. If an object isn't in one of the mask layers, the body will ignore it. By default, all bodies scan layer `1`.

These properties can be configured via code, or directly in the Inspector:

!collision_layers.png

**Example**
You have three nodes with the following configuration:

||Layer Bits|Mask Bits|
|--|--|--|
|**Player**|`1`|`2, 3`|
|**Enemy**|`2`|`1`|
|**Coin**|`3`|`1`|

In this scenario, the `Player` node would detect collisions with both `Enemy` and `Coin` nodes (because they are in layers it scans). However, `Enemy` and `Coin` nodes would not detect each other, because they only scan layers they are _not_ in.

## Area2D

`Area2D` nodes provide _detection_ and _influence_. They can detect when objects overlap and emit signals when bodies enter or exit. `Area2D`s can also be used to override physics properties such as gravity or damping in a defined area.

### When to Use

You should use an `Area2D` when you want to change the properties of a region in the game or when you want to detect that other objects have entered or exited.

Example uses for `Area2D`s:

-   An icy patch (reduced friction)
-   Testing for an empty spawn location (no objects are in the area)
-   Hurtboxes on a character (in a fighting game)

## PhysicsBody2D

The remaining three body types derive from `PhysicsBody2D`, an abstract base class. You will never access this class directly, only choose one of the three body types below.

### StaticBody2D

A static body is one that is not moved by the physics engine. It participates in collision detection, but does not move in response to the collision. However, it can impart motion or rotation to a colliding body _as if_ it were moving, using its `constant_linear_velocity` and `constant_angular_velocity` properties.

`StaticBody2D`s are most often used for objects that are part of the environment or that do not need to have any dynamic behavior.

#### When to Use

Example uses for `StaticBody2D`s:

-   Platforms (including moving platforms)
-   Conveyor belts
-   Walls and other obstacles

### KinematicBody2D

`KinematicBody2D` is for implementing bodies that are to be controlled by the user or via code. They detect collisions with other bodies when moving, but are not affected by physics properties like gravity or friction.

> Note: A `KinematicBody2D` can be affected by gravity and other forces, but you must calculate the movement in code. The physics engine will not move a `KinematicBody2D`.

When moving a `KinematicBody2D`, you should not set its `position` directly. Instead, you use the `move_and_collide()` or `move_and_slide()` methods. These methods move the body along a given vector, but it will instantly stop if a collision is detected with another body. After a KinematicBody2D has collided, any _collision response_ must be coded manually.

#### Collision Response

 After a collision, you may want the `KinematicBody2D` to bounce, to slide along a wall, or to alter the properties of the object it hit. The way you handle collision response depends on which method you used to move the KinematicBody2D.

When using `move_and_collide`, the function will return a [KinematicCollision2D] object, which contains information about the collision and the colliding body. You can use this information to determine the response.

For example, if you want to find the point in space where the collision occurred:
```
func _physics_process(delta):
    var collision_info = move_and_collide(velocity * delta)
    if collision_info:
        var collision_point = collision_info.position
```

See [KinematicBody2D] for more details and examples.

### RigidBody2D

This is the node that implements simulated 2D physics. You do not control a `RigidBody2D` directly. Instead you apply forces to it (gravity, impulses, etc.) and the physics engine calculates the resulting movement, including collisions, bouncing, rotating, etc.

You can modify a RigidBody's behavior via properties such as "Mass", "Friction", or "Bounce", which can be set in the Inspector:

![RigidBody2D Properties](img/rigidbody_properties.png)

The body's behavior is also affected by the world, via the `Project Settings -> Physics` properties, or by entering an [Area2D] that is overriding the global physics properties.

#### RigidBody Modes

A RigidBody can be set to one of four modes:

-   **Rigid** - The body behaves as a physical object. It collides with other bodies and responds to forces applied to it. This is the default mode.
-   **Static** - The body behaves like a [StaticBody2D] and does not move.
-   **Character** - Similar to `Rigid` mode, but the body can not rotate.
-   **Kinematic** - The body behaves like a [KinematicBody2D] and must be moved by code.

#### Using RigidBody2D

One of the benefits of using a rigid body is that a lot of behavior can be gotten "for free" without writing any code. For example, if you were making an "Angry Birds"-style game with falling blocks, you would only need to create RigidBody2Ds and adjust their properties. Stacking, falling, and bouncing would automatically be calculated by the physics engine.

However, if you do wish to have some control over the body, you should take care - altering the `position` or `linear_velocity` of a RigidBody2D can result in unexpected behavior. If you need to alter any of the physics-related properties, you should use the `_integrate_forces()` callback instead of `_physics_process`.

For example, here is the code for an "Asteroids" style spaceship:

```
extends RigidBody2D

var thrust = Vector2(0, 250)
var torque = 20000

func _integrate_forces(state):
    if Input.is_action_pressed("ui_up"):
        set_applied_force(thrust.rotated(rotation))
    else:
        set_applied_force(Vector2())
    var rotation_dir = 0
    if Input.is_action_pressed("ui_right"):
        rotation_dir += 1
    if Input.is_action_pressed("ui_left"):
        rotation_dir -= 1
    set_applied_torque(rotation_dir * torque)
```

Note that we are not setting the `linear_velocity` or `angular_velocity` properties directly, but rather applying forces (`thrust` and `torque`) to the body and letting the physics engine calculate the resulting movement.

#### When to Use

Example uses for `RigidBody2D`s:

-   Crates and boxes (a la "Angry Birds")
-   Spacecraft
-   Bouncing ball (Pong or Breakout)
