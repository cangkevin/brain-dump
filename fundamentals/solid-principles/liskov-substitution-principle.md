---
type: fundamentals
tags: fundamentals liskov-substitution-principle
---

# Liskov substitution principle

The Liskov substitution principle is the third letter of the SOLID principles.

## Meaning

Robert Martin provides a layman's interpretation of the Barbara Liskov's mathematical definition on strong behavioral subtyping. In short, "derived classes must be usable through the base class interface, without the need for clients to know the difference".

This is an extension of the open-closed principle with an emphasis on the behavior.

Interchangeability of subclasses that implement an interface without needing to know the specific type is really at the core of the idea. Needing to know what the types are for your inputs implies that there is a likely a dimension in your class/interface design where there isn't real interchangeability among all the subclasses.

Likewise, if the subclass doesn't benefit from inheriting the interface and ends having to really strange things that breaks away from the contract of the interface, then this would be a violation of the LSP.

## Example

Imagine that we were attempting to model birds in an application with the following:

```java
public class Bird {
  public void fly() {
    ...
  }
}

public class Duck extends Bird {}
```

This makes sense because a duck can fly since it is a bird.

However, imagine if Ostrich came into the picture:

```java
public class Ostrich extends Bird {}
```

An Ostrich is certainly a bird but it doesn't fly. The Ostrich class is a subtype of the class Bird but it should not be able to support the `fly()` method, hence it is a violation of the LSP.

One thing to keep in mind is that relationships in the real world does not necessarily translate well to code. Although it is true that an Ostrich is a bird, the `class` Ostrich is not an Ostrich - it's a piece of code; similarly, the `class` Bird is not a bird, it's also a piece of code. The classes Ostrich and Bird represents their respective entities but the representatives of things do not share the relationship of the things they represent.

A rather dark-humored analogy to understand the prior statement would be a divorce. Two spouses would have had two lawyers representing them, but the lawyers themselves were not the ones getting divorced.

One potential way to address the above issue would be to do something like:

```java
public class Bird {}

public class FlyingBird extends Bird {
  public void fly() {
    ...
  }
}

public class Duck extends FlyingBird {}

public class Ostrich extends Bird {}
```

This satisfies the LSP since there are no concerns about the substitutability when looking through the base class of `Bird` or `FlyingBird`. `Duck` and `Ostrich` inherits from the proper parent type which satisfies the behavioral expectations.

## References

- SO discussion on examples of LSP
  - <https://stackoverflow.com/questions/56860/what-is-an-example-of-the-liskov-substitution-principle>
