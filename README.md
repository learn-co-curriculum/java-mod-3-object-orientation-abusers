# Object Orientation Abusers

## Learning Goals

- Explain the object orientation abusers
- Use the pattern in Java

## Introduction

These smells arise when object-oriented principles are used partially, therefore
not taking full advantage of their benefits.

### Alternative classes with different interfaces

This occurs when different classes exist for concepts that are very close to one
another. Alternatives to having multiple classes for close concepts is either
creating a common base class or, preferably, creating a set of interfaces to
model the desired behavior and let each class implement the proper set of
interfaces.

### Refused Bequest

Remember what the "L" stands for in the SOLID principles:

> The Liskov Substitution Principle (LSP) dictates that "functions that use
> references to base classes must be able to use objects of the derived class
> without knowing it". Said more simply, we should be able to replace an
> instance of a base class with an instance of one of its extension class in any
> method that uses the base class and have the system continue to function the
> same way.

When a subclass extends a base class and takes away functionality from that base
class, it's a Refused Bequest smell. It's a good indication that the wrong class
hierarchy is being implemented, so review your class structure if you detect
this smell.

### Switch Statements

As we have seen, conditional statements can be very useful, but they can also be
abused.

A "switch statement" code smell occurs when a conditional is used to decide
between different versions of similar functionality.

Consider our "camera" example from a previous section. We could have decided to
implement different camera manufacturers with an `if` or a `switch` statement
centered around our enum instead of setting up an interface and two different
implementations. The problem with using conditionals instead of proper
abstractions is that those conditionals must be re-used everywhere where logic
depends on the specific camera manufacturer, for example, and each place where
those conditionals are used must now be fully aware of the type of camera it's
using.

Whereas with our proper abstraction, we were able to use the `Camera` interface
in this generic way that doesn't have to change based on the implementation of
that interface that we want to use:

```java
    public Photographer() {
        // change this to use a camera from a different manufacturer - everything else stays the same
        this.camera = CameraFactory.makeCamera(CameraFactory.CameraManufacturer.CANON_FILM);
    }

    public void takePhotograph() {
        camera.takePhotograph(0.125);
    }
```

### Temporary Field

This occurs when a variable that is used by a single method is defined as a
class variable instead. This has several implications:

- It makes the code very hard to read because the variable definition is
  potentially far away from where it's actually used.
- It makes the code hard to unit test, since other methods could potentially
  access and change the value of this variable outside of the method actually
  targeted by the unit test.
- It means the method has side effects, i.e. it modifies code that is not
  exclusively in its scope, making it harder for other parts of the system to be
  unit tested properly.
