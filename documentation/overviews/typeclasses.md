---
layout: tutorial
title: Typeclasses

disqus: true

---

## Typeclass in brief

Typeclasses are not a language feature; they are a pattern. The pattern is useful for adding functionality to classes without modification. For example, we can add the ability to serialize classes without adding a serialize method to the class.

*disclaimer: the name "typeclass" does not make sense. It is a historic name, not an intuitive one. Do not try to make sense of it.*

## Why do we have Typeclasses?

Typeclasses let us add functionlity to classes you didn't write, including built-ins. Typeclasses are also great for separation of concerns. 


## Example

Pretend we have a custom serialization protocol called Crunch. We want
to use this to transmit all kinds of things between our applications. We
write a library that includes a method for transmitting data using the
Crunch protocol -- call it crunchinate. Which classes can be converted
to Crunch format?

We could require that classes define a .toCrunch method, but we can't do that
for built-in or library types. The typeclass pattern lets us define
how to Crunch a class outside of the class itself. We can say,
"crunchinate accepts any type T, as long as something can convert it
to Crunch." Here is how we specify that:

    object Crunchinator {
      def crunchinate[T : Cruncher[T]](food: T): Unit = {
        // om nom
      }
    }

The : in the type parameter creates a [context bound](whereisit), a
constraint for "somewhere, there is a Cruncher for this type."

What is a Cruncher? We define that trait ourselves, as "a thing that knows how
to convert a T to Crunch," something like

    trait Cruncher[T] {
      def toCrunch(food: T): Array[Byte]
    }

If we want to be able to crunchinate Strings, we can create a Cruncher
for Strings:

    package object Cruncher {
      implicit val stringCruncher = new Cruncher[String] {
        def toCrunch(food: String): Array[Byte] = { ... }
      }
    }

The "implicit" keyword is important. That puts the value in the
compiler's magic hat. That magic hat is where the compiler looks to
satisfy the context bound. To crunchinate a String, we have to bring
that Cruncher into scope.

    import Cruncher._ // bring stringCruncher into the magic hat

    Crunch.crunchinate("any String we want")

The compiler finds the Cruncher[String] and decides that String is a
valid T for the context bound in crunchinate[T : Cruncher[T]].

In this example, Cruncher is a typeclass. stringCruncher is an instance
of the Cruncher typeclass for String. The context bound syntax `T :
Cruncher[T]` says (formally) "some type T for which an instance of the Cruncher
typeclass exists," or colloquially, "T with typeclass Cruncher."

... There is more to say about typeclasses that makes them more useful
than the implicit conversion pattern for adding methods.



