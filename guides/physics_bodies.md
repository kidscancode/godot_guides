# Guide to Physics Bodies

## Overview

Godot offers a variety of _physics bodies_ to choose from. Choosing the right body type for your object can be confusing. In this guide, we'll explore the pros and cons of each and help you

|.|moved by engine|moved by code|
|--|--|--|
|Physics during movement|RigidBody|KinematicBody|
|No physics during movement|StaticBody|Area|


## CollisionObject2D

`CollisionObject2D` is the root node for all of the bodies discussed here. This node can hold any number of `Shape2D` objects as children, which are used to define the body's collision bounds. Your can also use `CollisionShape2D` or `CollisionPolygon2D` to draw the shape directly in the Godot scene editor.

### Area2D

`Area2D` is a general-purpose node for area detection and area influence.

Common uses include detecting "enter" and "exit" events for other shapes and bodies moving in the world, altering 2D physics properties such as gravity or damping, and detection of other overlapping areas.

## PhysicsBody2D

The remaining three body types derive from the `PhysicsBody2D`, an abstract base class. You will never access this class directly, only choose one of the three body types below.

### StaticBody2D

A static body is a simple body that is intended to remain stationary relative to the game world.

### KinematicBody2D

`KinematicBody2D` is designed for implementing bodies that are to be controlled by the user (or code).  They can detect collisions with other bodies, but are not affected by physics.

### RigidBody2D

This is the node that implements full 2D physics. This means that you do not control a `RigidBody2D` directly. Forces can be applied to it (gravity, impulses), and the physics simulation will calculate the resulting collision, bouncing, rotating, etc.
