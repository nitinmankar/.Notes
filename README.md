# Phase 0 : Object-Oriented Programming and Fundamentals

## Core OOP principles: Encapsulation, Abstraction, Inheritance, and Polymorphism

### Encapsulation

Encapsulation means keeping the data inside the object private and only
letting it change and access through specific methods and properties, so
the object always follows its rules.<br>
Prevents invalid state.<br>
Encapsulation also allows validation logic before changing state, which
prevents invalid transitions.<br>
The big benefit of encapsulation is that it protects the object's data.
Since the fields are private, nobody can mess with them directly.
Instead, they have to go through methods or properties, which means I
can enforce the object's rules. That keeps the object in a valid state.
It also makes the code easier to maintain, because I can change how
things work inside without breaking the outside interface. And overall,
it makes the system safer, cleaner, and more reliable.

------------------------------------------------------------------------

### Abstraction

Abstraction means hiding the complex implementation details and showing
only the essential features that matter to the user.<br>
The benefit of abstraction is that it makes code easier to use and
understand. People don't need to know all the internal details, they
just work with a clean interface. It also reduces complexity, makes the
system more flexible, and allows me to change the implementation later
without affecting the outside code.

------------------------------------------------------------------------

### Inheritance

Inheritance means creating new classes based on existing ones. The child
class automatically gets the fields and methods of the parent class, and
I can add or override features as needed.<br>
The benefit of inheritance is that it promotes code reuse. I don't have
to rewrite common functionality, I can just inherit it. It also makes
code easier to organize, since related classes share a base. And it
helps with maintainability, because changes in the parent can flow down
to the children.

------------------------------------------------------------------------

### Polymorphism

Polymorphism means the same method or action can work differently
depending on the object. In other words, one interface can be used, but
the actual behavior changes based on the type.<br>
The benefit of polymorphism is flexibility. It lets me write code that
works with different types in a uniform way, without worrying about the
specific details. That makes the system easier to extend, reduces
duplication, and keeps the code cleaner.

There are two types of polymorphism:

-   Compile time: Method overloading<br>
-   Run time: Method overriding<br>

Virtual methods are resolved during runtime. Every object has a hidden
reference to its type's method table. When a virtual method is called
using a base class reference, the runtime uses that type's method table
and invokes the correct overridden method.

------------------------------------------------------------------------

## OOP

``` csharp
// Abstraction: common contract for all teams
public interface ITeam
{
    void PlayMatch(bool won);
    void ShowInfo();
}

// Base class: Team (Encapsulation + Inheritance)
public abstract class TeamBase : ITeam
{
    private int points; // Encapsulation: hidden field

    public string Name { get; }
    public int Points => points;

    protected TeamBase(string name) { Name = name; }

    public void PlayMatch(bool won)
    {
        points += won ? 3 : 1; // Rule: win=3, draw=1
    }

    public abstract void ShowInfo(); // Abstraction
}

// Polymorphism: different teams show info differently
public class LocalTeam : TeamBase
{
    public string City { get; }
    public LocalTeam(string name, string city) : base(name) { City = city; }

    public override void ShowInfo()
    {
        Console.WriteLine($"{Name} from {City} has {Points} points.");
    }
}

public class InternationalTeam : TeamBase
{
    public string Country { get; }
    public InternationalTeam(string name, string country) : base(name) { Country = country; }

    public override void ShowInfo()
    {
        Console.WriteLine($"{Name} representing {Country} has {Points} points.");
    }
}
```

------------------------------------------------------------------------

## When to use composition over inheritance?

I prefer composition when the relationship is 'has‑a' rather than
'is‑a.' For example, a car has an engine, tyres, and seats, it
doesn't make sense to say a car is an engine.<br>
Composition also helps when I want to use multiple objects together,
since multiple inheritance isn't allowed. It gives me flexibility and
keeps classes loosely coupled, whereas inheritance can create tight
coupling and complicated hierarchies if taken too far.

``` csharp
// Behavior interfaces (Abstraction)
public interface IFlyBehavior { void Fly(); }
public interface ISwimBehavior { void Swim(); }

// Concrete behaviors
public class CanFly : IFlyBehavior { public void Fly() => Console.WriteLine("Flying..."); }
public class CannotFly : IFlyBehavior { public void Fly() => Console.WriteLine("Cannot fly."); }
public class CanSwim : ISwimBehavior { public void Swim() => Console.WriteLine("Swimming..."); }

// Composition: Bird HAS behaviors
public class Bird
{
    private IFlyBehavior flyBehavior;
    private ISwimBehavior swimBehavior;

    public Bird(IFlyBehavior fly, ISwimBehavior swim)
    {
        flyBehavior = fly;
        swimBehavior = swim;
    }

    public void TryFly() => flyBehavior.Fly();
    public void TrySwim() => swimBehavior.Swim();
}

// Usage
var penguin = new Bird(new CannotFly(), new CanSwim());
penguin.TryFly();   // "Cannot fly."
penguin.TrySwim();  // "Swimming..."
```

------------------------------------------------------------------------

## Difference between Interface and Abstract

I will use the interface when I need loose coupling, dependency
injection, multiple inheritance and make unit testing easy and follow
SOLID principles.<br>
I will use an abstract class when I need a common base and want some
methods to be strictly inherited in child classes.

Version controlling is difficult in inheritance since I want to add a
new method signature in the interface but all the child classes will
break because they must implement it. After C#8 we can create default
methods in the interface too.<br>
In abstract class I can make that new method without abstract keywords
which will prevent breaking anything.

An abstract class can have state, constructors, and implemented methods,
while an interface is a contract and supports multiple inheritance.

------------------------------------------------------------------------

## Distinction between Class and Object

A class is a blueprint or template that defines: - properties (data)<br>
- methods (behavior)

It does not hold actual data until an object is created.

An object is a real instance of a class.<br>
It: - holds actual data in memory<br>
- can call the class methods<br>
- represents a real entity

Example:<br>
In ASP.NET Core, a Controller is a class, and when a request comes in,
the framework creates an object of that controller to handle the
request.

------------------------------------------------------------------------

## Constructors (Default, Parameterized, and Static implementations)

A constructor is a special method that has the same name as the class,
has no return type, and is called when an object is created to
initialize its state.

-   A default constructor has no parameters, and if we don't define any
    constructor, C# provides one automatically.<br>
-   A parameterized constructor takes parameters and is used to
    initialize fields, enforce a valid object state, and support
    dependency injection.<br>
-   A static constructor is parameterless, runs only once at runtime
    before the class is first used, and is used to initialize static
    fields.

------------------------------------------------------------------------

## Application of virtual, override, new and sealed keywords

-   Virtual: Used in the base class to allow a method to be overridden
    in a derived class.<br>
-   Override: Used in the child class to provide a new implementation of
    a virtual method.<br>
-   New: Used when we hide a base class method instead of overriding
    it.<br>
    Because new → compile-time binding, not runtime polymorphism.<br>
-   Sealed: Used to stop further overriding of an overridden method.<br>
    We can use sealed only with override, not directly on a base virtual
    method.

------------------------------------------------------------------------

## Understanding Access Modifiers

-   private → only inside the class<br>
-   protected → only in derived classes<br>
-   internal → only inside the same assembly<br>
-   protected internal → same assembly OR derived in another assembly<br>
-   private protected → derived classes in the same assembly only<br>
-   public → accessible everywhere

Example:<br>
In .NET projects, we often use internal for classes that should not be
exposed outside the assembly, and protected for base class members meant
for extension by derived services.

------------------------------------------------------------------------

## Differentiating Association, Aggregation, and Composition

Association is a relationship where two objects use each other without
ownership. One object can receive the other through dependency injection
or method parameters, and both can exist independently.

Aggregation is a special type of association with a has-a relationship
and weak ownership. The parent stores a reference to the child object,
but the child can exist independently and its lifecycle is not
controlled by the parent.

Composition is a strong has-a relationship where the parent creates and
owns the child object, and the child's lifecycle depends on the parent.
If the parent is destroyed, the child is also destroyed.

------------------------------------------------------------------------

## Mastery of the SOLID principles

### S : Single Responsibility Principle

A class should have only one reason to change, meaning it should handle
only one responsibility.<br>
This improves maintainability, testability, and separation of concerns.

### O : Open/Closed Principle

Software entities should be open for extension but closed for
modification.<br>
New behavior should be added using abstractions and polymorphism without
changing existing tested code.

### L : Liskov Substitution Principle

Derived classes must be substitutable for their base classes without
altering the correctness of the program.<br>
They must honor the base class contract and not change expected
behavior.

### I : Interface Segregation Principle

Clients should not be forced to depend on methods they do not use.<br>
Prefer small, specific interfaces over large, general-purpose ones.

### D : Dependency Inversion Principle

High-level modules should not depend on low-level modules.<br>
Both should depend on abstractions.<br>
Dependency Injection is the implementation of this principle in
frameworks like ASP.NET Core.

------------------------------------------------------------------------

## Deep dive into the Dependency Inversion Principle

DIP lets us depend on interfaces, so we can inject mocks instead of real
repositories, removing database dependency and enabling fast unit
tests.<br>
It causes tight coupling, prevents mocking, violates OCP, and locks the
service to a specific database implementation.

DIP is a design principle; DI container is a tool that implements it.
DIP can be achieved manually using constructor injection.

``` csharp
namespace SOLID
{
    public interface IPayment
    {
        void Pay(decimal amount);
    }

    public interface IRefundable
    {
        void Refund(decimal amount);
    }

    public class CardPayment : IPayment
    {
        public void Pay(decimal amount)
        {
            Console.WriteLine("Paying with card...");
        }
    }

    public class OnlinePayment : IPayment, IRefundable
    {
        public void Pay(decimal amount)
        {
            Console.WriteLine("Paying online...");
        }

        public void Refund(decimal amount)
        {
            Console.WriteLine("Refunding online...");
        }
    }

    public class PaymentService
    {
        private readonly IPayment _payment;

        public PaymentService(IPayment payment)
        {
            _payment = payment;
        }

        public void ProcessPayment(decimal amount)
        {
            _payment.Pay(amount);
        }
    }

    IPayment payment = new CardPayment();
    payment.Pay(100);

    payment = new OnlinePayment();
    payment.Pay(200);

    if (payment is IRefundable refundablePayment)
    {
        refundablePayment.Refund(50);
    }
}
```
