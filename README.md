# 🧠 Phase 0 -- Object-Oriented Programming & Fundamentals (C#)

This phase covers the **core building blocks of OOP**, relationships,
access modifiers, constructors, polymorphism internals, and **SOLID
principles** with practical C# examples.

------------------------------------------------------------------------

# 📦 Core OOP Principles

## 🔐 Encapsulation

Encapsulation means **hiding internal data** and allowing access only
through controlled methods/properties.

### ✅ Benefits

-   Prevents invalid object state\
-   Enables validation before mutation\
-   Protects internal data\
-   Improves maintainability\
-   Keeps objects consistent and reliable

------------------------------------------------------------------------

## 🎭 Abstraction

Abstraction means **hiding complex implementation** and exposing only
essential behavior via interfaces or abstract classes.

### ✅ Benefits

-   Reduces complexity\
-   Clean API surface\
-   Implementation can change without breaking callers\
-   Improves flexibility

------------------------------------------------------------------------

## 🧬 Inheritance

Inheritance allows a **child class to reuse** fields and methods of a
base class.

### ✅ Benefits

-   Code reuse\
-   Logical hierarchy\
-   Easier maintenance

------------------------------------------------------------------------

## 🎭 Polymorphism

Same interface → **different behavior based on object type**

### Types

  Type           Mechanism
  -------------- --------------------------------------
  Compile-time   Method Overloading
  Run-time       Method Overriding (virtual/override)

### ⚙️ Runtime Polymorphism

Virtual methods are resolved at runtime using the method table (vtable)
of the actual object type.

------------------------------------------------------------------------

# ⚽ OOP Example -- Teams

``` csharp
public interface ITeam
{
    void PlayMatch(bool won);
    void ShowInfo();
}

public abstract class TeamBase : ITeam
{
    private int points;

    public string Name { get; }
    public int Points => points;

    protected TeamBase(string name) => Name = name;

    public void PlayMatch(bool won)
    {
        points += won ? 3 : 1;
    }

    public abstract void ShowInfo();
}

public class LocalTeam : TeamBase
{
    public string City { get; }

    public LocalTeam(string name, string city) : base(name)
        => City = city;

    public override void ShowInfo()
        => Console.WriteLine($"{Name} from {City} has {Points} points.");
}

public class InternationalTeam : TeamBase
{
    public string Country { get; }

    public InternationalTeam(string name, string country) : base(name)
        => Country = country;

    public override void ShowInfo()
        => Console.WriteLine($"{Name} representing {Country} has {Points} points.");
}
```

------------------------------------------------------------------------

# 🧩 Composition vs Inheritance

## Prefer Composition when → **HAS-A** relationship

> A car has an engine, tyres, seats\
> A car is not an engine ❌

### ✅ Advantages

-   Loose coupling\
-   Flexible behavior swapping\
-   Avoids deep inheritance trees\
-   Supports multiple behaviors

------------------------------------------------------------------------

## 🐦 Composition Example -- Strategy Pattern

``` csharp
public interface IFlyBehavior { void Fly(); }
public interface ISwimBehavior { void Swim(); }

public class CanFly : IFlyBehavior
{
    public void Fly() => Console.WriteLine("Flying...");
}

public class CannotFly : IFlyBehavior
{
    public void Fly() => Console.WriteLine("Cannot fly.");
}

public class CanSwim : ISwimBehavior
{
    public void Swim() => Console.WriteLine("Swimming...");
}

public class Bird
{
    private readonly IFlyBehavior flyBehavior;
    private readonly ISwimBehavior swimBehavior;

    public Bird(IFlyBehavior fly, ISwimBehavior swim)
    {
        flyBehavior = fly;
        swimBehavior = swim;
    }

    public void TryFly() => flyBehavior.Fly();
    public void TrySwim() => swimBehavior.Swim();
}

var penguin = new Bird(new CannotFly(), new CanSwim());
```

------------------------------------------------------------------------

# 🧱 Interface vs Abstract Class

  Feature                Interface             Abstract Class
  ---------------------- --------------------- ----------------
  Multiple inheritance   ✅ Yes                ❌ No
  State (fields)         ❌ No                 ✅ Yes
  Constructors           ❌ No                 ✅ Yes
  Default methods        ✅ C# 8+              ✅ Yes
  Loose coupling         ✅ High               ⚠️ Medium
  Versioning safety      ❌ Breaking changes   ✅ Safer

------------------------------------------------------------------------

# 🧍 Class vs Object

  Class                Object
  -------------------- -------------------
  Blueprint            Instance
  No memory for data   Holds actual data
  Defines behavior     Executes behavior

------------------------------------------------------------------------

# 🏗️ Constructors

  Type            Description
  --------------- ----------------------------
  Default         No parameters
  Parameterized   Initializes required state
  Static          Runs once per type

``` csharp
public class Example
{
    static Example() { }
    public Example() { }
    public Example(string name) { }
}
```

------------------------------------------------------------------------

# 🔁 virtual vs override vs new vs sealed

  Keyword           Purpose
  ----------------- ------------------------------
  virtual           Allows overriding
  override          Replaces base implementation
  new               Hides base method
  sealed override   Prevents further overriding

------------------------------------------------------------------------

# 🔐 Access Modifiers

  Modifier             Scope
  -------------------- ------------------------------------
  private              Inside class only
  protected            Derived classes
  internal             Same assembly
  protected internal   Same assembly OR derived elsewhere
  private protected    Derived in same assembly
  public               Everywhere

------------------------------------------------------------------------

# 🔗 Relationships

  Type          Description           Ownership
  ------------- --------------------- --------------------------
  Association   Uses another object   ❌ None
  Aggregation   Has-a (weak)          ❌ Independent lifecycle
  Composition   Has-a (strong)        ✅ Dependent lifecycle

------------------------------------------------------------------------

# 🧩 SOLID Principles

## S -- Single Responsibility

One class → one reason to change

## O -- Open/Closed

Open for extension, closed for modification

## L -- Liskov Substitution

Child must work wherever parent is expected

## I -- Interface Segregation

Prefer small, specific interfaces

## D -- Dependency Inversion

Depend on abstractions, not concretions

------------------------------------------------------------------------

# 💳 SOLID Example -- Payments

``` csharp
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
        => Console.WriteLine("Paying with card...");
}

public class OnlinePayment : IPayment, IRefundable
{
    public void Pay(decimal amount)
        => Console.WriteLine("Paying online...");

    public void Refund(decimal amount)
        => Console.WriteLine("Refunding online...");
}

public class PaymentService{
    private readonly IPayment _payment;

    public PaymentService(IPayment payment)
        => _payment = payment;

    public void ProcessPayment(decimal amount)
        => _payment.Pay(amount);
}
```

------------------------------------------------------------------------



