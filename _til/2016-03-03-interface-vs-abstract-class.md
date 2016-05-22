---
title:  "Interface vs. Abstract Class"
date:   2016-03-03 18:35:00
category: til
tags: [design-patterns, object-oriented, java, school]
---

TIL the difference between an `interface` and an `abstract class`.

Well...I guess I didn't really learn this today, but I wrote up some notes for my AP Computer Science class to help clarify differences between the two and when to use each.

### Interface
 - Declares __can-do__ relationships between classes.
 - When classes promise to implement an `interface` we can treat those classes similarly in our code.
   - Declare an object of reference type `<interface_name>`
 - In Java, classes can __implement many interfaces__.
 - Interface methods are __abstract and public__, meaning they do not contain any implementation detail.
 - You can leave off the public keyword in `interface` method headers, since they are always public.
 - __Cannot instantiate an object__ of type interface
 - __Can create object references__ of type interface with actual object types of the class that implements the interface.
   - This way we can achieve polymorphism through the common interface.

__Example: Shapes (triangle, circle, rectangle):__ Common methods `getArea` and `getPerimeter`

 - It is better to implement this as a Shape `interface`, because __none of the implementation of the common methods is shared between the shapes.__
   - Each method has different implementation details. Calculating the area of a square differs from a triangle differs from a circle...
   - Making it an interface also frees up the single inheritance for some other class as appropriate.

*Interface Header:* `interface <interface_name>`

*Class That Implements:* `public class <class_name> implements <interface_name>`

### Abstract class
 - Declares __is-a__ relationships between classes.
 - A __non-instantiatable__ class that serves as a super class for common *data* and *behaviors*.
   - Can declare data fields.
   - Can define implementation details for methods.
 - Use this if you don’t want the client code constructing objects out of a class in the hierarchy, but still want to share common data and code between subclasses via inheritance.
 - An `abstract class` can implement interfaces like a normal class
   - Any methods that are not implemented in the abstract class *will need to be implemented by a subclass that inherits from it*.
 - It may or may not actually contain abstract methods…
   - Even if no abstract methods exist in the class, it still can’t be instantiated as a concrete object type.
 - Can have class (static) members and methods. Interfaces cannot.
 - Code in an `abstract class` can call any of its abstract methods, __even if they don’t have implementation details in the abstract class__.
   - We can do this because the `abstract class` is never instantiated itself, and we can count on concrete classes that inherit from the abstract class to implement the methods.

*Abstract Class Header:* `public abstract class <abstract_class_name>`

*Class That Extends:* `public class <class_name> extends <abstract_class_name>`

__Note:__ in the class header above, the extending class __must provide implementation detail for all abstract methods__ in the `abstract class`, otherwise it must be declared as abstract itself.

### General Rules
 - If the base class has the potential to update frequently, `abstract classes` are preferred.
   - If an `abstract class` is updated (implementation detail, method signatures, etc...) then that new functionality will automatically propagate to the rest of the classes that inherit from it.
   - If an `interface` is updated (method signatures update or a new method is added) then __all classes that previously implemented that interface WILL BREAK.__
 - If you are creating something that provides common functionality to unrelated classes, an `interface` is a better bet.
   - This way, unrelated entities don’t need to inherit from the same base class (which implies an *is-a* relationship).

Writing this was actually quite helpful in clarifying some of the finer differences between `interfaces` and `abstract classes` in my own understanding. This is one of the reasons I enjoy teaching!
