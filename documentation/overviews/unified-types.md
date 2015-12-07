---
layout: page
title: Unified Types
tutorial: scala-tour
num: 2
tutorial-next: classes
tutorial-previous: tour-of-scala
---

In contrast to Java, all values in Scala are objects (including numerical values and functions). Since Scala is class-based, all values are instances of a class. The diagram below illustrates the class hierarchy.

![Scala Type Hierarchy]({{ site.baseurl }}/resources/img/classhierarchy.png)

## Scala Class Hierarchy ##

The superclass of all classes `scala.Any` has two direct subclasses `scala.AnyVal` and `scala.AnyRef` representing two different class worlds: value classes and reference classes. Value classes correspond to the primitive types of Java-like languages. User-defined classes define reference types by default; i.e. they always (indirectly) subclass `scala.AnyRef`. If Scala is used in the context of a Java runtime environment, then `scala.AnyRef` corresponds to `java.lang.Object`.
Please note that the diagram above also shows implicit conversions (called _views_) between value classes.
Here is an example that demonstrates that both numbers, characters, boolean values, and functions are objects just like every other object:
 
    object UnifiedTypes extends App {
      val set = new scala.collection.mutable.LinkedHashSet[Any]
      set += "This is a string"  // add a string
      set += 732                 // add a number
      set += 'c'                 // add a character
      set += true                // add a boolean value
      set += main _              // add the main function
      val iter: Iterator[Any] = set.iterator
      while (iter.hasNext) {
        println(iter.next.toString())
      }
    }
 
The program declares an application `UnifiedTypes` in form of a top-level [singleton object](objects) extending `App`. The application defines a local variable `set` which refers to an instance of class `LinkedHashSet[Any]`. The program adds various elements to this set. The elements have to conform to the declared set element type `Any`. In the end, string representations of all elements are printed out.

Here is the output of the program:

    This is a string
    732
    c
    true
    <function>
