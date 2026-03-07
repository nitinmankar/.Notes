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

Interfaces can now have behavior (methods), they still cannot hold instance state (fields). Abstract classes remain the only choice when you need to share non-static data fields across a hierarchy.

------------------------------------------------------------------------

## Distinction between Class, Object, Struct, Enum and Record

Class<br>
A class is a blueprint or template that defines: - properties (data)<br>
- methods (behavior)

It does not hold actual data until an object is created.

Object<br>
An object is a real instance of a class.<br>
- holds actual data in memory<br>
- can call the class methods<br>
- represents a real entity

Example:<br>
In ASP.NET Core, a Controller is a class, and when a request comes in,
the framework creates an object of that controller to handle the
request.

Struct<br>
Structs are value types. Their memory is allocated wherever they are declared (stack for local variables, heap for class fields).  
Example: DateTime, Point, Money
```csharp
public struct Money
{
    public decimal Amount { get; }
    public string Currency { get; }

    public Money(decimal amount, string currency)
    {
        Amount = amount;
        Currency = currency;
    }
}
```
Record(C# 9+)<br>
Record is a reference type with value-based equality and built-in immutability support.
Used for:
- DTOs
- API models
- Immutable data
- Comparing by value instead of reference
```csharp
public record Person(string Name, int Age);

var p1 = new Person("Nitin", 25);
var p2 = new Person("Nitin", 25);

Console.WriteLine(p1 == p2); // true ✅ value equality
```
Record struct<br>
It is just record + struct and instead of reference type it is value type.<br>
With the help of read only we can make it immutable.<br>

Enum<br>
Enum is a named constant set of numeric values.
```csharp
enum OrderStatus
{
    Pending,
    Shipped,
    Delivered
}

enum Status : byte
{
    Active = 1,
    Inactive = 2
}
```
------------------------------------------------------------------------
## set, init, with
- set : can change anytime
- init : can only set during object creation
- with : creates a copy of a record with modified values

```csharp
public class User
{
    public string Name { get; set; }
}

var u = new User();
u.Name = "Nitin"; // allowed
u.Name = "Rahul"; // allowed ❌ mutable
```
```csharp
public class User
{
    public string Name { get; init; }
}

var u = new User { Name = "Nitin" }; // allowed
u.Name = "Rahul"; // ❌ compile-time error
```
```csharp
public record Person(string Name, int Age);

var p1 = new Person("Nitin", 25);

var p2 = p1 with { Age = 26 };
```
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
    it.(Shadowing)<br>
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
If the parent is destroyed, the child is also destroyed. The child object usually cannot exist without the parent (e.g., a Room in a House).

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
A common violation of LSP is when a derived class throws a NotImplementedException for a method inherited from a base class.  

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
It helps in loose coupling, mocking.

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

# Phase 1 : C# Foundational Concepts

## Boxing-Unboxing

Boxing - Converting a value type (like int, struct) into a reference type (object). 
```csharp
int num = 42;
object boxed = num; // Boxing
```
Unboxing - Extracting the value type from the object
```csharp
object boxed = 42;
int num = (int)boxed; // Unboxing
```

------------------------------------------------------------------------

## Data types, Variables, and Operators.

Data types define the kind of data a variable can hold and how much memory it uses.<br>
Value types → stored on stack → fast → fixed size → no GC overhead<br>
var, int, double, bool, struct, enum<br>
Reference types → stored on heap → accessed via reference → GC managed<br>
string, class, object, record, collections<br>
Variables are named memory locations with a specific type.<br>
Operators perform operations on variables:
arithmetic, relational, logical, bitwise, assignment, null-coalescing.<br>
- The is and as operators in C# are both used for type checking and casting
```csharp
object obj = "Hello";
if (obj is string)   // ✅ true
    Console.WriteLine("obj is a string");

if (obj is int)      // ❌ false
    Console.WriteLine("obj is an int");
object obj = "Hello";
```

```csharp
string str = obj as string;   // ✅ str = "Hello"
int? num = obj as int?;       // ❌ num = null

if (str != null)
    Console.WriteLine(str.ToUpper()); // "HELLO"
```

- The checked and unchecked keywords in C# control how the compiler/runtime handles arithmetic overflow for integral types (int, long, etc.)
```csharp
checked
{
    int x = int.MaxValue;
    int y = x + 1; // ❌ OverflowException
}
```
```csharp
unchecked
{
    int x = int.MaxValue;
    int y = x + 1; // ✅ No exception, y = int.MinValue
}
```
- The dynamic keyword in C# is used to declare variables whose type is resolved at runtime instead of compile time.
```csharp
dynamic value = 10;
Console.WriteLine(value + 5);   // Works fine

value = "Hello";
Console.WriteLine(value.ToUpper()); // Works fine
```
------------------------------------------------------------------------

## Control flow (if, switch, loops)

- if / else
  - Few conditions
  - Complex boolean expressions
  - Short-circuiting (&&, ||)
- switch
Old code
```csharp
switch (status)
{
    case 1:
        return "Pending";
    case 2:
        return "Completed";
    default:
        return "Unknown";
}
```
Modern
```csharp
string result = status switch
{
    1 => "Pending",
    2 => "Completed",
    _ => "Unknown"
};
```
For integral types (int, char, enums), the compiler often generates a jump table.<br>
Instead of checking each case one by one, it computes an offset and jumps directly to the matching case.<br>
This makes execution O(1) instead of O(n)<br>
- For loop
```csharp
for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine(arr[i]);
}
```
- For each loop
```csharp
foreach (var item in list)
{
Console.WriteLine(item);
}
```
Difference Between for and foreach Internally<br>
- for loop:<br>
Uses an index to iterate.<br>
Works best with arrays or collections that support indexing (List).<br>
No extra overhead — direct access via index.<br>
- foreach loop:<br>
Uses an enumerator (IEnumerator) under the hood.<br>
Calls GetEnumerator(), then repeatedly MoveNext() and Current.<br>
Slight overhead compared to for, especially for arrays, because of enumerator setup.<br>
Performance:<br>
For arrays → for is typically faster because it avoids enumerator overhead.<br>
For non-indexed collections (like HashSet) → foreach is more natural and sometimes faster, since these don’t support indexing.<br>
foreach relies on an enumerator that tracks the collection’s state. If the collection is modified (add/remove elements) while enumerating: The enumerator becomes invalid. A InvalidOperationException is thrown.

------------------------------------------------------------------------

## Understanding Variance (in, out keywords).

- ref: ref passes a variable by reference. The method can read and modify the caller’s variable.<br>
The variable must be initialized before being passed.
```csharp
void Increment(ref int x)
{
    x++;
}

int num = 5;
Increment(ref num);
Console.WriteLine(num); // 6
```
- out: out also passes a variable by reference, but specifically for output.<br>
The method must assign a value before returning.<br>
The variable does not need to be initialized before being passed.
```csharp
void GetValues(out int a, out int b)
{
    a = 10;
    b = 20;
}

int x, y;
GetValues(out x, out y);
Console.WriteLine($"{x}, {y}"); // 10, 20
```
- in: in passes a variable by reference, but as read-only.<br>
The method can read the value but cannot modify it.<br>
Useful for performance when passing large structs (avoids copying).<br>
```csharp
void PrintPoint(in Point p)
{
    Console.WriteLine($"{p.X}, {p.Y}");
    // p.X = 10; ❌ Error: cannot modify because it's 'in'
}

Point pt = new Point(1, 2);
PrintPoint(in pt);
```

------------------------------------------------------------------------

## Indexers & Iterators (yield return)

An indexer allows an object to be accessed like an array using []. Internally it compiles into get_Item() and set_Item() methods. It is syntactic sugar over a property.<br>
The yield keyword in C# is used to simplify the creation of iterators — methods that return elements one at a time without needing to build an entire collection up front.<br>
Purpose: Produces values lazily, one at a time.<br>
No container created by yield itself.<br>
Compiler generates a hidden enumerator (state machine).<br>
Iteration executes when you use foreach, .ToList(), etc.<br>
<br>
yield return transforms the method into a compiler-generated state machine class implementing implementing IEnumerable and IEnumerator. It maintains state between calls, allowing execution to pause at each yield and resume from the same point during the next iteration.
```csharp
IEnumerable<int> GetNumbers()
{
    yield return 1;
    yield return 2;
    yield return 3;
}
List<int> list = GetNumbers().ToList(); 
// list = [1, 2, 3]
```
yield supplies values → .ToList() collects them into a list
you can do multiple returns with yield.

------------------------------------------------------------------------

## Basic concepts of Attributes and Reflection.

- Attributes
An attribute is metadata attached to code elements (class, method, property, etc.).
It provides declarative information that can be read at runtime.
Example:
```csharp
[Serializable]
public class User
{
   [Required]
   public string Name { get; set; }
}
```
Internally<br>
Attribute = class inheriting from System.Attribute<br>
Compiled into metadata<br>
Not executed automatically<br>

Example:
```csharp
public class LogAttribute : Attribute
{
   public string Message { get; }

   public LogAttribute(string message)
   {
       Message = message;
   }
}

Usage:
```csharp
[Log("Processing")]
public void Process() { }
```
Important Points
Attribute is passive metadata<br>
Needs reflection (or framework) to read it<br>
Stored in assembly metadata<br>
Constructor runs only when attribute is accessed<br>

Real ASP.NET Core Usage
 - [HttpGet] → Routing
 - [Authorize] → Authorization
 - [Required] → Validation
 - [ApiController] → Behavior configuration
Framework reads them using reflection.
- Reflection
What is Reflection?<br>
Reflection allows inspecting types, methods, properties, and attributes at runtime.<br>
It enables dynamic behavior when type is not known at compile time.<br>
 Basic Example
```csharp
Type type = typeof(User);

foreach (var prop in type.GetProperties())
{
   Console.WriteLine(prop.Name);
}
```
Creating Object Dynamically
```csharp
var instance = Activator.CreateInstance(typeof(User));
```
Slower than new.<br>

Reading Attributes
```csharp
var method = typeof(Service).GetMethod("Execute");
var attr = method.GetCustomAttribute<LogAttribute>();
```
Why Reflection is Slower?<br>
Runtime type resolution<br>
Metadata lookup<br>
No compile-time optimization<br>
No inlining<br>
Possible boxing/unboxing<br>

How ASP.NET Core Stays Fast?
Reflection mostly at startup<br>
Metadata cached<br>
Compiled delegates used<br>
Minimal reflection per request<br>

------------------------------------------------------------------------

## Standard Collections: List, Dictionary, HashSet.

- List<T>
List<T> is a dynamic array that automatically resizes when capacity is exceeded.<br>
Maintains order<br>
Allows duplicates<br>
Provides index-based access<br>

Example
```csharp
List<int> numbers = new List<int>();

numbers.Add(10);
numbers.Add(20);
numbers.Add(30);

Console.WriteLine(numbers[1]); // 20
```

Important Properties<br>
Count: Number of elements<br>
Capacity: Internal array size<br>

Important Methods
Add element
```csharp
numbers.Add(10);
```
Add multiple elements
```csharp
numbers.AddRange(new int[] { 1, 2, 3 });
```
Insert element
```csharp
numbers.Insert(1, 50);
```
Remove element
```csharp
numbers.Remove(10);
```
Remove at index
```csharp
numbers.RemoveAt(2);
```
Remove all matching condition
```csharp
numbers.RemoveAll(x => x > 10);
```
Check element exists
```csharp
numbers.Contains(20);
```
Find element
```csharp
numbers.Find(x => x > 10);
```
Find index
```csharp
numbers.IndexOf(20);
```
Sort
```csharp
numbers.Sort();
```
Reverse
```csharp
numbers.Reverse();
```
Convert to array
```csharp
numbers.ToArray();
```
Clear list
```csharp
numbers.Clear();
```

Time Complexity
| Operation | Complexity |
|----------|------------|
| Add | O(1) average |
| Insert | O(n) |
| Remove | O(n) |
| Search | O(n) |
| Index access | O(1) |

- Dictionary<TKey, TValue>
A key-value collection implemented using hash table.<br>
Keys must be unique<br>
Lookup is very fast<br>

Example
```csharp
Dictionary<int, string> users = new Dictionary<int, string>();

users.Add(1, "Nitin");
users.Add(2, "Rahul");
```

Important Properties<br>
Count: Number of key-value pairs<br>
Keys: All keys<br>
Values: All values<br>

Important Methods
Add key-value
```csharp
users.Add(1, "Nitin");
```
Alternative add
```csharp
users[2] = "Rahul";
```
Check key exists
```csharp
users.ContainsKey(1);
```
Check value exists
```csharp
users.ContainsValue("Nitin");
```
Safe retrieval
```csharp
users.TryGetValue(1, out var name);
```
Remove key
```csharp
users.Remove(1);
```
Iterate dictionary
```csharp
foreach (var pair in users)
{
   Console.WriteLine(pair.Key);
   Console.WriteLine(pair.Value);
}
```
Clear dictionary
```csharp
users.Clear();
```
Time Complexity
| Operation | Complexity |
|----------|------------|
| Add | O(1) |
| Lookup | O(1) |
| Remove | O(1) |
| Worst case | O(n) |

- HashSet<T>
A collection that stores only unique elements.<br>
No duplicates allowed<br>
Uses hash table<br>
Fast lookup<br>

Example
```csharp
HashSet<int> set = new HashSet<int>();

set.Add(10);
set.Add(20);
set.Add(10);
Result → only 10, 20
```
Important Property<br>
Count: Number of unique elements

Important Methods
Add element
```csharp
set.Add(10);
//Returns false if duplicate.
```
Remove element
```csharp
set.Remove(10);
```
Check existence
```csharp
set.Contains(20);
```
Clear set
```csharp
set.Clear();
```
Set operations<br>
Union
```csharp
set.UnionWith(otherSet);
```
Intersection
```csharp
set.IntersectWith(otherSet);
```
Difference
```csharp
set.ExceptWith(otherSet);
```
- Queue
```csharp
Queue<int> queue = new Queue<int>();

queue.Enqueue(10);
queue.Enqueue(20);
queue.Dequeue();
```
Used for:<br>
BFS<br>
Job queues<br>
Task scheduling<br>

- Stack
```csharp
Stack<int> stack = new Stack<int>();

stack.Push(10);
stack.Push(20);
stack.Pop();
```
Used for:<br>
DFS<br>
Undo operations<br>
Expression evaluation<br>

------------------------------------------------------------------------

## IEnumerable
IEnumerable<T> represents a read-only sequence of elements that can be iterated using foreach.
It provides sequential access only.
Example:
```csharp
IEnumerable<int> numbers = new List<int> { 1, 2, 3 };

foreach (var n in numbers)
{
   Console.WriteLine(n);
}
```

Important Points<br>
Supports iteration<br>
No index access<br>
Usually lazy execution<br>
Minimal operations<br>
Used heavily in LINQ<br>

Important Method<br>
GetEnumerator(): This returns an IEnumerator<T> used by foreach.

------------------------------------------------------------------------

## Delegates, Func, Action, and Predicate usage.

- Delegate
A delegate is a type-safe reference to a method.<br>
It allows methods to be passed as parameters, stored in variables, and invoked dynamically.<br>
Definition:<br>
A delegate is a type-safe function pointer that holds a reference to a method with a specific signature.<br>

Syntax
```csharp
public delegate ReturnType DelegateName(ParameterTypes);
```
Example:
```csharp
public delegate int Operation(int a, int b);
```
Meaning:<br>
Delegate name : Operation<br>
Parameters    : int, int<br>
Return type   : int<br>

Assigning a Method to a Delegate
```csharp
public delegate int Operation(int a, int b);

static int Add(int a, int b)
{
   return a + b;
}

Operation op = Add;
int result = op(3,4);
```
Flow:<br>
op → reference to Add<br>
op(3,4) → calls Add(3,4)<br>

Passing Delegates as Parameters<br>
Delegates allow passing behavior to methods.
```csharp
static void Calculate(int a, int b, Operation operation)
{
   Console.WriteLine(operation(a,b));
}
```
Usage:
```csharp
Calculate(5,3,Add);
```
Benefit:<br>
Method behavior becomes flexible.<br>

Anonymous Methods<br>
Instead of defining a separate method:<br>
```csharp
Operation op = delegate(int a, int b)
{
   return a + b;
};
```
This creates a method without a name.<br>

Lambda Expressions (Modern C#)<br>
Lambda's are the most common way to create delegates.
```csharp
Operation op = (a,b) => a + b;
```
Structure:<br>
(parameters) => expression<br>
Example:
```csharp
Func<int,int> square = x => x * x;
```

Built-in Delegates<br>
Instead of creating custom delegates, .NET provides generic delegates.<br>
- Action
Used when a method returns void.<br>
Example:
```csharp
Action<string> greet = name => Console.WriteLine(name);
```
Meaning:<br>
input  : string<br>
return : void<br>

- Func
Used when a method returns a value.<br>
Structure:<br>
Func<T1, T2, TResult><br>
Last type = return type<br>
Example:
```csharp
Func<int,int,int> add = (a,b) => a + b;
```
Meaning:<br>
inputs : int, int<br>
return : int<br>
Example with multiple parameters:<br>
Func<int,int,int,int,string><br>
Meaning:<br>
inputs : int,int,int,int<br>
return : string<br>
Maximum supported: 16 parameters + 1 return type<br>

- Predicate
Represents a method that returns bool.<br>
Definition:
```csharp
public delegate bool Predicate<T>(T obj);
```
Example:
```csharp
Predicate<int> isEven = x => x % 2 == 0;
```
Meaning:<br>
input  : int<br>
return : bool<br>
Note: Predicate supports only ONE parameter.<br>
In modern C#, Func<T,bool> is used more often.<br>

Multicast Delegates<br>
A delegate can reference multiple methods.<br>
Example:
```csharp
public delegate void Notify();

Notify notify = Email;
notify += SMS;

notify();
```
Output:<br>
Email<br>
SMS<br>
Remove a method:
```csharp
notify -= SMS;
```
Delegate Invocation List<br>
Multicast delegates maintain a list of method references.<br>
Delegate<br>
 ↓
Method1<br>
Method2<br>
Method3<br>
Retrieve invocation list:
```csharp
notify.GetInvocationList();
```
Return Value in Multicast Delegates<br>
If multiple methods return values: Only the last method’s return value is returned.<br>

Delegates vs Interfaces<br>
| Delegates | Interfaces |
|----------|-----------|
| Used to pass a single method | Used to define multiple related methods |
| Lightweight | Used for full abstraction |
| Common in callbacks | Common in service design |

Where Delegates Are Used<br>
Delegates power many .NET features.<br>
Examples:<br>
LINQ
```csharp
numbers.Where(x => x > 5);
```
Events
```csharp
button.Click += OnClick;
```
Sorting
```csharp
list.Sort((a,b) => a.CompareTo(b));
Tasks
Task.Run(() => DoWork());
```

Delegate Mental Model<br>
Delegates allow passing behavior instead of data<br>
Example:<br>
Where(condition)<br>
Select(transformation)<br>
OrderBy(keySelector)<br>
Each parameter is a delegate.<br>

Key Delegate Patterns (Important for LINQ)<br>
Func<T,bool>     → filtering<br>
Func<T,TResult>  → transformation<br>
Func<T,TKey>     → sorting/grouping<br>
Example:
```csharp
numbers.Where(x => x > 5)
      .Select(x => x * 2)
      .OrderBy(x => x);
```

Quick Revision Cheat Sheet<br>
Delegate = reference to a method<br>
Action      → returns void<br>
Func        → returns value<br>
Predicate   → returns bool<br>
Delegates enable:<br>
✔ callbacks<br>
✔ events<br>
✔ LINQ<br>
✔ flexible method behavior<br>

------------------------------------------------------------------------

## LINQ operations: Select, Where, OrderBy, GroupBy, Any/All.

LINQ<br>
LINQ (Language Integrated Query) is a feature in C# that allows querying data using C# syntax instead of manual loops.<br>
It works with collections like:<br>
Arrays<br>
List<T><br>
Dictionary<TKey,TValue><br>
HashSet<T><br>
Database queries (Entity Framework)<br>
XML<br>
Example without LINQ:
```csharp
List<int> numbers = new List<int> {1,2,3,4,5};

List<int> result = new List<int>();

foreach(var n in numbers)
{
   if(n > 3)
       result.Add(n);
}
```
With LINQ:
```csharp
var result = numbers.Where(x => x > 3);
```

LINQ Syntax Types<br>
Method Syntax (Most Common)<br>
Uses extension methods + lambda expressions
```csharp
var result = numbers.Where(x => x > 3);
```

Query Syntax (SQL-like)
```csharp
var result =
   from n in numbers
   where n > 3
   select n;
```
Both produce the same result.<br>
LINQ works on collections implementing: IEnumerable<T><br>
Examples:<br>
List<int><br>
int[]<br>
Dictionary<TKey,TValue><br>

Basic LINQ Pipeline<br>
LINQ operations form a pipeline<br>
Collection<br>
  ↓<br>
Filter<br>
  ↓<br>
Transform<br>
  ↓<br>
Sort<br>
  ↓<br>
Result<br>
Example:<br>
```csharp
var result = numbers
           .Where(x => x > 2)
           .Select(x => x * 10)
           .OrderBy(x => x);
```
Core Delegate Patterns in LINQ<br>
Most LINQ methods rely on Func delegates.<br>
Filtering<br>
Func<T,bool><br>
Example:
```csharp
numbers.Where(x => x > 3);
```

Transformation<br>
Func<T,TResult><br>
Example:
```csharp
numbers.Select(x => x * 2);
```

Key Selection (Sorting / Grouping)<br>
Func<T,TKey><br>
Example:
```csharp
people.OrderBy(p => p.Age);
```
Most Important LINQ Methods<br>
Where – Filtering<br>
Keeps elements that satisfy a condition.
```csharp
numbers.Where(x => x > 3);
```
Example:<br>
[1,2,3,4,5]<br>
→ Where(x > 3)<br>
→ [4,5]<br>

Select – Transformation<br>
Transforms elements into new values.
```csharp
numbers.Select(x => x * 2);
```
Example:<br>
[1,2,3,4]<br>
→ Select(x * 2)<br>
→ [2,4,6,8]<br>
Important:<br>
Select does NOT modify the original collection. It creates a new sequence.<br>

OrderBy – Sorting<br>
Sort elements using a key.
```csharp
numbers.OrderBy(x => x);
```
Descending:
```csharp
numbers.OrderByDescending(x => x);
```
Example:
```csharp
people.OrderBy(p => p.Age);
```

GroupBy – Grouping<br>
Groups elements based on a key.
```csharp
numbers.GroupBy(x => x % 2);
```
Result:<br>
Group 0 → even numbers<br>
Group 1 → odd numbers<br>
Return type:<br>
IGrouping<TKey, TElement><br>

Any – Check Existence<br>
Returns true if at least one element matches condition.
```csharp
numbers.Any(x => x > 4);
```
Example:<br>
[1,2,3,4,5]<br>
Any(x > 4) → true<br>

All – Check All Elements<br>
Returns true if all elements satisfy condition.
```csharp
numbers.All(x => x > 0);
```

First / FirstOrDefault<br>
Returns first matching element.
```csharp
numbers.First(x => x > 3);
```
Safe version:
```csharp
numbers.FirstOrDefault(x => x > 10);
```
Returns default value if none found.<br>

Aggregation Methods<br>
Used to calculate summary values.<br>
numbers.Count()<br>
numbers.Sum()<br>
numbers.Average()<br>
numbers.Max()<br>
numbers.Min()<br>
Example:<br>
```csharp
numbers.Sum();
```

8. Distinct – Remove Duplicates
```csharp
numbers.Distinct();
```
Example:<br>
[1,2,2,3,3]<br>
→ Distinct<br>
→ [1,2,3]<br>

9. Skip and Take – Pagination<br>
Used for paging results.
```csharp
numbers.Skip(10).Take(10);
```
Example:<br>
Page 2 results<br>
Common in:<br>
API pagination<br>
database queries<br>

SelectMany – Flatten Nested Collections<br>
Used when working with nested collections.<br>
Example:
```csharp
List<List<int>> numbers =
[
   new List<int>{1,2},
   new List<int>{3,4}
];
```
Flatten:
```csharp
numbers.SelectMany(x => x);
```
Result:<br>
1,2,3,4<br>

Join – Combine Two Collections<br>
Used to join related data.<br>
Example:
```csharp
users.Join(orders,
          u => u.Id,
          o => o.UserId,
          (u,o) => new {u.Name, o.Amount});
```
Similar to SQL JOIN.<br>

Deferred Execution<br>
LINQ queries do not execute immediately.<br>
Example:
```csharp
var query = numbers.Where(x => x > 3);
```
Execution happens when:<br>
foreach<br>
ToList()<br>
ToArray()<br>
Count()<br>
First()<br>
Example:
```csharp
var result = numbers.Where(x => x > 3).ToList();
```

Immediate Execution Methods<br>
These force execution.<br>
ToList()<br>
ToArray()<br>
Count()<br>
Sum()<br>
First()<br>

IEnumerable vs IQueryable
| Feature | IEnumerable | IQueryable |
|--------|-------------|------------|
| Execution | In memory | In database |
| Data source | Collections | Remote sources |
| Example | List<T> | Entity Framework |

Example:
```csharp
dbContext.Users.Where(u => u.Age > 25);
```
Converted to SQL.<br>

Real Backend LINQ Example<br>
Example class:
```csharp
class User
{
   public string Name;
   public int Age;
}
```
Query:
```csharp
var result = users
           .Where(u => u.Age > 25)
           .OrderBy(u => u.Name)
           .Select(u => u.Name);
```
Meaning:<br>
Filter users older than 25<br>
Sort by name<br>
Return only names<br>

LINQ Quick Revision Cheat Sheet<br>
Common operators:<br>
Where           → filtering<br>
Select          → transformation<br>
OrderBy         → sorting<br>
GroupBy         → grouping<br>
Any             → existence check<br>
All             → check all elements<br>
First           → first element<br>
FirstOrDefault  → safe first element<br>
Count           → number of elements<br>
Sum/Average     → aggregation<br>
Distinct        → remove duplicates<br>
Skip/Take       → pagination<br>
SelectMany      → flatten collections<br>
Join            → combine collections<br>

How LINQ Works Internally (Interview)<br>
LINQ is implemented using:<br>
Extension methods<br>
Delegates (Func<T>)<br>
Deferred execution<br>
IEnumerable<T><br>
Example:
```csharp
numbers.Where(x => x > 3);
```
Internally:
Enumerable.Where(numbers, predicate)<br>

Final LINQ Mental Model<br>
Collection<br>
  ↓<br>
Where (filter)<br>
  ↓<br>
Select (transform)<br>
  ↓
OrderBy (sort)<br>
  ↓<br>
Result<br>
LINQ allows writing queries on collections using delegates and lambda expressions.<br>

------------------------------------------------------------------------

## Events and EventHandler in C#
Event<br>
An event is a mechanism that allows a class (publisher) to notify other classes (subscribers) when something happens.<br>
Events are built on top of delegates.<br>
Example scenarios:
- Button click
- File created
- Download completed
- User logged in

Publisher–Subscriber Model<br>
Events follow the Publisher–Subscriber pattern.<br>
Publisher: Class that raises the event<br>
Subscriber: Class that listens and reacts to the event<br>

Flow:<br>
Subscriber subscribes to event<br>
       ↓<br>
Publisher performs action<br>
       ↓<br>
Publisher raises event<br>
       ↓<br>
All subscribers are notified<br>

Event Declaration<br>
Basic syntax:
```csharp
public event DelegateType EventName;
```
Example:
```csharp
public event Action DownloadCompleted;
```

Raising an Event<br>
Events are raised using Invoke().<br>
Recommended way:
```csharp
DownloadCompleted?.Invoke();
```
?. ensures the event is invoked only if there are subscribers, preventing NullReferenceException.<br>
Equivalent safe code:
```csharp
if (DownloadCompleted != null)
{
   DownloadCompleted.Invoke();
}
```

Subscribing and Unsubscribing<br>
Subscribe:
```csharp
obj.EventName += HandlerMethod;
```
Unsubscribe:
```csharp
obj.EventName -= HandlerMethod;
```
Example:
```csharp
downloader.DownloadCompleted += OnDownloadCompleted;
```

Multiple Subscribers<br>
Events support multiple subscribers because delegates are multicast.<br>
Example:
```csharp
event += Handler1;
event += Handler2;
event += Handler3;
```
When invoked:<br>
Handler1()<br>
Handler2()<br>
Handler3()<br>
All handlers execute sequentially.<br>

Why Events Instead of Delegates?<br>
Delegates allow external classes to:<br>
delegate = null<br>
delegate()<br>
delegate = otherMethod<br>
This breaks encapsulation.<br>
Events restrict external access.<br>
External classes can only:<br>
+= (subscribe)<br>
-= (unsubscribe)<br>
Only the publisher can invoke the event.<br>

Standard .NET Event Pattern<br>
.NET recommends using EventHandler or EventHandler<T>.<br>
Event declaration
```csharp
public event EventHandler DownloadCompleted;
```
Raising event
```csharp
DownloadCompleted?.Invoke(this, EventArgs.Empty);
```

EventHandler Delegate<br>
EventHandler is a predefined delegate in .NET.<br>
Definition:
```csharp
public delegate void EventHandler(object sender, EventArgs e);
```
Parameters:<br>
sender: Object that raised the event<br>
EventArgs: Contains event data<br>

EventHandler<T> (Generic Version)<br>
Used when the event needs to send custom data.<br>
Example:
```csharp
public event EventHandler<DownloadEventArgs> DownloadCompleted;
```
Custom EventArgs class:
```csharp
class DownloadEventArgs : EventArgs
{
   public int FileSize { get; set; }
}
```
Raise event:
```csharp
DownloadCompleted?.Invoke(this, new DownloadEventArgs { FileSize = 500 });
```
Subscriber:
```csharp
void OnDownloadCompleted(object sender, DownloadEventArgs e)
{
   Console.WriteLine(e.FileSize);
}
```

Event Invocation Pattern (Best Practice)<br>
Best practice in .NET is to raise events using a protected virtual method.<br>
Example:
```csharp
protected virtual void OnDownloadCompleted(EventArgs e)
{
   DownloadCompleted?.Invoke(this, e);
}
```
Advantages:<br>
Allows derived classes to override event behavior<br>
Improves maintainability<br>

Real Examples in .NET<br>
Button click event:
```csharp
button.Click += Button_Click;
```
File system watcher:
```csharp
watcher.Created += OnFileCreated;
```
ASP.NET lifecycle events:<br>
Page_Load<br>
Application_Start<br>

Event vs Delegate
| Feature | Delegate | Event |
|--------|----------|-------|
| Purpose | Method reference | Notification system |
| Invocation | Anyone can invoke | Only publisher |
| Assignment | Allowed | Not allowed |
| Encapsulation | Weak | Strong |
| Subscribers | Multiple | Multiple |

Important Interview Points<br>
Events: Are based on delegates<br>
Follow publisher–subscriber pattern<br>
Support multiple subscribers<br>
Use EventHandler / EventHandler<T> pattern<br>
Must be invoked using ?.Invoke()<br>
Recommended event declaration:
```csharp
public event EventHandler<TEventArgs> EventName;
```

------------------------------------------------------------------------

## Lambda
A lambda expression is a concise way to write anonymous functions that can be assigned to delegates or passed as parameters.<br>
Syntax:<br>
(parameters) => expression<br>
Example:
```csharp
x => x * 2
```
Meaning:<br>
Takes x and returns x * 2<br>

Lambda and Delegates<br>
Lambda expressions are commonly used to create delegate instances.<br>
Example:
```csharp
Func<int, int> square = x => x * x;
```
Equivalent method:
```csharp
int Square(int x)
{
   return x * x;
}
```

Lambda with Multiple Parameters
```csharp
Func<int, int, int> add = (a, b) => a + b;
```
Meaning:<br>
input: int, int<br>
return: int<br>

Expression Lambda<br>
Single expression lambda.<br>
x => x * 2<br>
Automatically returns the expression result.<br>
Example:
```csharp
Func<int, int> doubleValue = x => x * 2;
```

Statement Lambda<br>
Used when multiple statements are required.
```csharp
x =>
{
   int result = x * x;
   return result;
}
```
Example:
```csharp
Func<int, int> square = x =>
{
   int result = x * x;
   return result;
};
```

Lambda with LINQ<br>
Lambda expressions are heavily used in LINQ queries.<br>
Filtering:
```csharp
numbers.Where(x => x > 5);
```
Transformation:
```csharp
numbers.Select(x => x * 2);
```
Sorting:
```csharp
numbers.OrderBy(x => x);
```

Lambda with Events<br>
Lambda expressions can be used as event handlers.<br>
Example:
```csharp
button.Click += (s, e) => Console.WriteLine("Clicked");
```
Instead of defining a separate method.<br>

Common Delegates Used with Lambdas<br>
`Func` represents a delegate that returns a value.
```csharp
Func<int, int> square = x => x * x;
```
Rule:<br>
Func<T1, T2, TResult><br>
Last type → return value.<br>

`Action` represents a delegate that returns nothing.
```csharp
Action<int> print = x => Console.WriteLine(x);
```

`Predicate` represents a delegate that returns bool.
```csharp
Predicate<int> isEven = x => x % 2 == 0;
```

Key Points<br>
Lambda expressions create anonymous functions.<br>
Commonly used with delegates and LINQ.<br>
Reduce boilerplate code.<br>
Often used with Func, Action, and Predicate delegates.<br>

Short Interview Definition<br>
Lambda Expression
A lambda expression is a concise syntax used to create anonymous functions that can be assigned to delegates or passed as parameters.

------------------------------------------------------------------------

## Generics
Generics allow classes, methods, and delegates to work with different data types while maintaining type safety.<br>
Syntax:<br>
<T><br>
Example:<br>
List<int><br>
Dictionary<string, int><br>
T represents a type parameter that is specified when the class or method is used.<br>

Why Generics Exist<br>
Before generics, collections used object type.<br>
Example (non-generic):
```csharp
ArrayList list = new ArrayList();

list.Add(10);
list.Add("Hello");
```
Problems:<br>
No type safety<br>
Requires casting<br>
Runtime errors possible<br>
Boxing / unboxing<br>

Example:
```csharp
int num = (int)list[1]; // runtime error
```

Generic Collections<br>
Generics allow specifying the type at compile time.<br>
Example:
```csharp
List<int> numbers = new List<int>();

numbers.Add(10);
numbers.Add(20);
```
Invalid:
```csharp
numbers.Add("Hello"); // compile-time error
```
Benefits:<br>
Type safety<br>
No casting<br>
Better performance<br>

Generic Class<br>
A class can be defined using a type parameter.
```csharp
class Box<T>
{
   public T Value;
}
```
Usage:
```csharp
Box<int> intBox = new Box<int>();
intBox.Value = 10;

Box<string> strBox = new Box<string>();
strBox.Value = "Hello";
```

Generic Method<br>
Methods can also be generic.
```csharp
public T GetFirst<T>(T[] items)
{
   return items[0];
}
```
Usage:
```csharp
int num = GetFirst(new int[] {1,2,3});
string word = GetFirst(new string[] {"A","B"});
```

Multiple Generic Parameters<br>
Generics can accept multiple type parameters.<br>
```csharp
class Pair<TKey, TValue>
{
   public TKey Key;
   public TValue Value;
}
```
Usage:
```csharp
Pair<int, string> pair = new Pair<int, string>();
```

```csharp
pair.Key = 1;
pair.Value = "Apple";
```
Example from .NET:<br>
Dictionary<TKey, TValue><br>

Generic Delegates<br>
Delegates can also use generics.<br>
Common examples:<br>
Func: Returns a value.
```csharp
Func<int, int> square = x => x * x;
```
Rule:<br>
Func<T1, T2, TResult><br>
Last type → return value.<br>

Action: Returns nothing.<br>
```csharp
Action<int> print = x => Console.WriteLine(x);
```

Predicate: Returns a boolean.<br>
```csharp
Predicate<int> isEven = x => x % 2 == 0;
```

Generic Constraints<br>
Constraints restrict what types can be used<br>
Syntax:<br>
where T : constraint<br>
Examples:<br>
Reference type<br>
where T : class<br>
Value type<br>
where T : struct<br>
Parameterless constructor<br>
where T : new()<br>
Base class<br>
where T : BaseClass<br>
Interface<br>
where T : IDisposable<br>

Variance in Generics<br>
Variance defines how generic types behave with inheritance.<br>
Covariance (out)<br>
Allows using a more derived type.
Example:
```csharp
IEnumerable<string> strings = new List<string>();
IEnumerable<object> objects = strings;
```
Used when the type produces values (output).<br>
Example:<br>
IEnumerable<out T><br>

Contravariance (in)<br>
Allows using a less derived type.<br>
Example:
```csharp
Action<object> act = obj => Console.WriteLine(obj);
Action<string> strAct = act;
```
Used when the type consumes values (input).<br>
Example:<br>
Action<in T><br>

Invariance<br>
No conversion between generic types.<br>
Example:
```csharp
List<string> strings = new List<string>();
List<object> objects = strings; // not allowed
```
Because List<T> both reads and writes values.<br>

Advantages of Generics<br>
Compile-time type safety<br>
Eliminates casting<br>
Improves performance<br>
Reusable code for multiple data types<br>

Common Generic Types in .NET<br>
Examples used frequently:<br>
List<T><br>
Dictionary<TKey,TValue><br>
HashSet<T><br>
Queue<T><br>
Stack<T><br>
Func<T><br>
Action<T><br>
Predicate<T><br>
Task<T><br>
EventHandler<T><br>
IEnumerable<T><br>

Short Interview Definition<br>
Generics allow classes, methods, and delegates to work with multiple data types while ensuring compile-time type safety.

------------------------------------------------------------------------

## Exception Handling strategies (best practices and implementing custom exceptions).
An exception is a runtime error that disrupts the normal execution of a program.
Examples:
Division by zero<br>
Null reference<br>
Invalid input<br>
File not found<br>

Example:
```csharp
int a = 10;
int b = 0;

int result = a / b; // DivideByZeroException
```

try–catch Block<br>
Used to handle exceptions and prevent application crashes.
```csharp
try
{
   int result = 10 / 0;
}
catch (Exception ex)
{
   Console.WriteLine(ex.Message);
}
```
Structure:<br>
try → code that may throw exception<br>
catch → handles the exception<br>

Multiple Catch Blocks<br>
Different exceptions can be handled separately.<br>
```csharp
try
{
   int[] arr = new int[2];
   Console.WriteLine(arr[5]);
}
catch (IndexOutOfRangeException ex)
{
   Console.WriteLine("Index error");
}
catch (Exception ex)
{
   Console.WriteLine("General error");
}
```
Rule:<br>
Specific exceptions → first<br>
General exceptions → last<br>

finally block<br>
The finally block always executes whether an exception occurs or not.<br>
```csharp
try
{
   int result = 10 / 2;
}
catch (Exception ex)
{
   Console.WriteLine(ex.Message);
}
finally
{
   Console.WriteLine("Cleanup code");
}
```
Common uses:<br>
Closing files<br>
Releasing resources<br>
Database cleanup<br>

throw keyword<br>
Used to manually raise an exception.<br>
```csharp
if (age < 18)
{
   throw new Exception("Age must be at least 18");
}
```

Rethrowing Exceptions<br>
Correct way:<br>
```csharp
catch (Exception)
{
   throw;
}
```
Incorrect:
```csharp
catch (Exception ex)
{
   throw ex;
}
```
Reason:<br>
throw → preserves original stack trace<br>
throw ex → resets stack trace<br>

Custom Exceptions<br>
Custom exceptions are used for application-specific errors.<br>
Example:
```csharp
public class InsufficientBalanceException : Exception
{
   public InsufficientBalanceException(string message): base(message)
   {
   }
}
```
Usage:
```csharp
if (balance < amount)
{
   throw new InsufficientBalanceException("Insufficient balance");
}
```

Best Practices<br>
Catch specific exceptions<br>
Good:<br>
catch (FileNotFoundException ex)<br>
Avoid catching Exception unless necessary.<br>

Do not use exceptions for normal program flow<br>
Bad:<br>
```csharp
try
{
   int.Parse(input);
}
catch
{
}
```
Better:
```csharp
int.TryParse(input, out int result);
```

Do not swallow exceptions<br>
Bad:
```csharp
catch
{
}
```
Always log or handle exceptions properly.<br>

Throw meaningful exceptions<br>
Bad:
```csharp
throw new Exception("Error");
```
Better:
```csharp
throw new ArgumentException("Invalid input");
```

Common Built-in Exceptions<br>
Frequently used exceptions:<br>
NullReferenceException<br>
ArgumentException<br>
InvalidOperationException<br>
DivideByZeroException<br>
IndexOutOfRangeException<br>
FileNotFoundException<br>

Short Interview Definition<br>
Exception handling is a mechanism used to detect and handle runtime errors so that a program can continue executing safely.<br>

------------------------------------------------------------------------

## The IDisposable interface and the using pattern.

- IDisposable
IDisposable is an interface used to release unmanaged resources manually.<br>
Definition:
```csharp
public interface IDisposable
{
   void Dispose();
}
```
Classes implement IDisposable when they use resources like:<br>
Files<br>
Database connections<br>
Network sockets<br>
Streams<br>
Unmanaged memory<br>

Why IDisposable is Needed<br>
The Garbage Collector (GC) only manages managed memory.<br>
It does not immediately release external resources like:<br>
File handles<br>
Database connections<br>
OS resources<br>
So we use Dispose() to release them explicitly.<br>

Implementing IDisposable<br>
Example:
```csharp
public class FileManager : IDisposable
{
   public void Dispose()
   {
       Console.WriteLine("Resources released");
   }
}
```
Usage:
```csharp
FileManager fm = new FileManager();
fm.Dispose();
```

- The Using Statement
The using statement ensures that Dispose() is automatically called.<br>
Example:
```csharp
using (StreamReader reader = new StreamReader("file.txt"))
{
   string content = reader.ReadToEnd();
}
```
Equivalent code:
```csharp
StreamReader reader = new StreamReader("file.txt");
```
```csharp
try
{
   string content = reader.ReadToEnd();
}
finally
{
   reader.Dispose();
}
```
So using guarantees resource cleanup even if an exception occurs.<br>

Common Classes That Implement IDisposable<br>
Many .NET classes implement IDisposable.<br>
Examples:<br>
StreamReader<br>
StreamWriter<br>
FileStream<br>
SqlConnection<br>
HttpClient<br>
MemoryStream<br>
Example:
```csharp
using (SqlConnection conn = new SqlConnection(connectionString))
{
   conn.Open();
}
```

When to Implement IDisposable<br>
A class should implement IDisposable if it:<br>
Uses unmanaged resources<br>
Uses objects that implement IDisposable<br>
Needs deterministic resource cleanup<br>
Example:
```csharp
class DataService : IDisposable
{
   private SqlConnection connection;

   public void Dispose()
   {
       connection.Dispose();
   }
}
```

Dispose Pattern (Basic)<br>
When a class owns disposable resources, it should release them inside Dispose().
Example:
```csharp
public class Service : IDisposable
{
   private FileStream fileStream;

   public void Dispose()
   {
       fileStream?.Dispose();
   }
}
```

Benefits of Using IDisposable<br>
Releases resources immediately<br>
Prevents resource leaks<br>
Ensures proper cleanup<br>
Improves application stability<br>

Best Practices<br>
Always dispose objects that implement IDisposable<br>
Example:
```csharp
using (StreamReader reader = new StreamReader("file.txt"))
{
}
```

Prefer using over manual Dispose<br>
Good:
```csharp
using (var stream = new FileStream("file.txt", FileMode.Open))
{
}
```
Avoid:
```csharp
var stream = new FileStream("file.txt", FileMode.Open);
stream.Dispose();
```

Dispose dependent resources<br>
If your class contains disposable objects, dispose them inside Dispose().<br>

Short Interview Definition<br>
IDisposable is an interface that provides a mechanism for releasing unmanaged resources through the Dispose() method.<br>

Key Interview Point<br>
using → ensures Dispose() is called automatically<br>
Equivalent pattern:<br>
using → try + finally + Dispose()<br>

------------------------------------------------------------------------

## Asynchronous programming with async/await.
Asynchronous programming allows a program to perform long-running tasks without blocking the main thread.<br>
Examples of long-running tasks:<br>
File operations<br>
Database queries<br>
API calls<br>
Network requests<br>
Instead of waiting for the operation to complete, the program can continue executing other work.<br>

2. Why Async Programming is Needed
Without async:
```csharp
Thread waits → application becomes slow or unresponsive
```
Example:
```csharp
DownloadFile();
ProcessData();
```
If DownloadFile() takes 5 seconds, everything else waits.<br>
Async allows:<br>
Start operation → continue execution → resume when result is ready<br>

async and await Keywords<br>
C# provides two keywords:<br>
async → marks a method as asynchronous<br>
await → waits for an asynchronous operation<br>
Example:
```csharp
public async Task DownloadData()
{
   await Task.Delay(2000);
   Console.WriteLine("Download completed");
}
```

Task and Task<T><br>
Async methods usually return Task.<br>
Task represents an operation that does not return a value.
```csharp
public async Task ProcessData()
{
   await Task.Delay(1000);
}
```

Task<T> represents an operation that returns a value.
```csharp
public async Task<int> GetNumber()
{
   await Task.Delay(1000);
   return 10;
}
```
Usage:
```csharp
int num = await GetNumber();
```

Basic Async Example
```csharp
public async Task DownloadFile()
{
   Console.WriteLine("Downloading...");

   await Task.Delay(3000);

   Console.WriteLine("Download completed");
}
```
Calling the method:
```csharp
await DownloadFile();
```

await Behavior<br>
await:<br>
Pauses the method<br>
Releases the thread<br>
Resumes execution when the task completes<br>
Example flow:<br><br>
Method starts<br>
↓<br>
await Task<br>
↓<br>
Thread freed<br>
↓<br>
Task completes<br>
↓<br>
Method resumes<br>

Async Method Rules<br>
Async methods usually return:<br>
Task<br>
Task<T><br>
Example:
```csharp
public async Task<int> GetValue()
{
   return await Task.FromResult(5);
}
```
Avoid using:<br>
async void, except for event handlers.<br>

Async Void (Special Case)<br>
Used mainly for event handlers.<br>
Example:
```csharp
private async void Button_Click(object sender, EventArgs e)
{
   await Task.Delay(1000);
}
```
Reason:<br>
Event handlers require void return <br>

Running Tasks in Parallel<br>
Example:
```csharp
Task t1 = DownloadFile();
Task t2 = ProcessData();

await Task.WhenAll(t1, t2);
```
Both tasks run concurrently.<br>

Exception Handling in Async<br>
Exceptions are handled using try-catch.<br>
Example:
try
```csharp
{
   await DownloadFile();
}
catch (Exception ex)
{
   Console.WriteLine(ex.Message);
}
```

Advantages of Async Programming<br>
Improves responsiveness<br>
Better scalability<br>
Efficient thread usage<br>
Handles I/O operations efficiently<br>

Common Async Methods in .NET<br>
Examples:<br>
Task.Delay()<br>
HttpClient.GetAsync()<br>
Stream.ReadAsync()<br>
File.ReadAllTextAsync()<br>
Example:
```csharp
string data = await File.ReadAllTextAsync("data.txt");
```

Short Interview Definition<br>
Asynchronous programming allows a program to execute long-running operations without blocking the main thread.<br>

Key Interview Points<br>
async → marks asynchronous method<br>
await → waits for task completion<br>
Task → represents asynchronous operation<br>
Task<T> → asynchronous operation with result<br>
Avoid:<br>
async void<br>
Prefer:<br>
Task / Task<T><br>

------------------------------------------------------------------------

## Distinguishing Task from Thread and utilizing CancellationToken.

- Thread
A Thread is a low-level OS execution unit used to run code concurrently.<br>
Example:
```csharp
Thread thread = new Thread(() =>
{
   Console.WriteLine("Running in thread");
});

thread.Start();
```
Characteristics:<br>
OS managed<br>
Heavyweight<br>
Manual lifecycle management<br>
Consumes more resources<br>
<br>
Threads give full control over execution, but they are harder to manage.

Task<br>
A Task represents an asynchronous operation.<br>
It is built on top of the ThreadPool and managed by the Task Parallel Library (TPL).
Example:
```csharp
Task.Run(() =>
{
   Console.WriteLine("Running in task");
});
```
Characteristics:<br>
High-level abstraction<br>
Managed by .NET runtime<br>
Uses ThreadPool<br>
Lightweight<br>
Works well with async/await<br>
<br>
Tasks are the recommended way for asynchronous work in modern .NET.<br>

Task vs Thread<br>

| Feature | Thread | Task |
|--------|--------|------|
| Level | Low-level | High-level abstraction |
| Management | OS managed | .NET runtime |
| Resource usage | Heavy | Lightweight |
| ThreadPool usage | No | Yes |
| Async support | No | Yes |
| Recommended | Rarely | Yes |


Creating Tasks<br>
Task.Run()<br>
Used to run work on a background thread.<br>
```csharp
Task.Run(() =>
{
   Console.WriteLine("Task running");
});
```
Here:
```csharp
() => { ... }
```
is a lambda expression that creates an anonymous Action delegate.<br>

Task.Run with CancellationToken<br>
Task.Run has an overload:
```csharp
Task.Run(Action action, CancellationToken cancellationToken)
```
Example:
```csharp
Task.Run(() =>
{
   while (!cts.Token.IsCancellationRequested)
   {
       Console.WriteLine("Working...");
       Thread.Sleep(500);
   }
}, cts.Token);
```
Explanation:<br>
First parameter  → lambda expression (anonymous Action delegate)<br>
Second parameter → CancellationToken<br>

CancellationToken<br>
CancellationToken enables cooperative cancellation of tasks.<br>
It allows a task to check if cancellation was requested and stop gracefully.<br>

CancellationTokenSource<br>
Cancellation begins with CancellationTokenSource.<br>
Example:
```csharp
var cts = new CancellationTokenSource();

CancellationToken token = cts.Token;
```
CancellationTokenSource → issues cancellation signal<br>
CancellationToken → passed to tasks<br>

Cancelling a Task<br>
Example:
```csharp
var cts = new CancellationTokenSource();

Task.Run(() =>
{
   while (!cts.Token.IsCancellationRequested)
   {
       Console.WriteLine("Working...");
       Thread.Sleep(500);
   }
}, cts.Token);

Thread.Sleep(2000);

cts.Cancel();
```
Flow:<br>
Task starts<br>
↓<br>
Task checks token<br>
↓<br>
cts.Cancel() is called<br>
↓<br>
Token signals cancellation<br>
↓<br>
Task stops execution<br>

Cancellation Exception<br>
Tasks may throw:<br>
OperationCanceledException<br>
Example:
```csharp
token.ThrowIfCancellationRequested();
```

Key Points<br>
Thread → low-level OS thread<br>
Task → high-level async abstraction<br>
Task uses ThreadPool<br>
CancellationToken → cooperative cancellation<br>
CancellationTokenSource → triggers cancellation<br>

Short Interview Definitions<br>
Thread: A thread is a low-level OS-managed execution unit used for concurrent execution.<br>
Task: A task represents an asynchronous operation managed by the .NET Task Parallel Library.<br>
CancellationToken: A CancellationToken is used to signal and cooperatively cancel asynchronous operations.<br>

------------------------------------------------------------------------

## Memory organization: Heap versus Stack.
Stack<br>
The stack stores:<br>
Method calls<br>
Value type variables<br>
References to objects<br>

Characteristics:<br>
Fast allocation<br>
LIFO structure<br>
Automatically cleaned when method ends<br>

Heap<br>
The heap stores:<br>
Objects<br>
Reference type instances<br>
Characteristics:<br>
Managed by Garbage Collector<br>
Slower allocation than stack<br>
Objects live until GC removes them<br>
Example:
```csharp
Person p = new Person();
```
Memory:
Stack                Heap
-----                -----
p  ───────────────→  Person object

Key Rule<br>
Value types → value stored directly<br>
Reference types → reference stored, object in heap<br>

Important Difference<br>
Value type assignment → copies value<br>
Reference type assignment → copies reference<br>
Example:
```csharp
int a = 5;
int b = a;     // value copied
```

```csharp
Person p1 = new Person();
Person p2 = p1; // reference copied
```

Interview Summary<br>
Stack → value types, method calls, references<br>
Heap → objects and reference type instances<br>

------------------------------------------------------------------------

## Garbage Collection (GC) and generation concepts.

Garbage Collection (GC) is an automatic memory management system in .NET that reclaims memory used by objects that are no longer needed.<br>
GC works only on heap memory.<br>
Purpose:<br>
Free unused objects<br>
Prevent memory leaks<br>
Manage heap memory automatically<br>

When Does GC Run?<br>
The Garbage Collector runs when:<br>
Heap memory becomes full<br>
System memory pressure occurs<br>
GC is triggered manually<br>
Manual trigger (not recommended normally):<br>
```csharp
GC.Collect();
```
Normally, the .NET runtime decides when to run GC.<br>

How GC Works<br>
Simplified process:<br>
Find objects that are still referenced<br>
Mark them as alive<br>
Remove unreferenced objects<br>
Reclaim memory<br>
Objects without references become eligible for garbage collection.<br>
Example:
```csharp
Person p = new Person();
p = null;
```
Now the object is no longer referenced and can be collected.<br>

GC Generations<br>
To improve performance, GC organizes objects into generations.<br>
Generation 0<br>
Generation 1<br>
Generation 2<br>

- Generation 0
Newly created objects<br>
Collected most frequently<br>
Small and fast to clean<br>

Example:<br>
Short-lived objects<br>
Temporary variables<br>

- Generation 1
Objects that survive Gen 0 collection move to Gen 1.<br>
Purpose:<br>
Buffer between short-lived and long-lived objects<br>

- Generation 2
Objects that survive multiple collections move to Gen 2.<br>
Characteristics:<br>
Long-lived objects<br>
Collected less frequently<br>
More expensive GC operation<br>
Examples<br>
Application-wide objects<br>
Caches<br>
Static data<br>

Generation Flow
Object created<br>
     ↓<br>
Generation 0<br>
     ↓<br>
Survives GC<br>
     ↓<br>
Generation 1<br>
     ↓<br>
Survives again<br>
     ↓<br>
Generation 2<br>

Benefits of Generational GC<br>
Improves performance<br>
Most objects die quickly<br>
Reduces expensive full heap scans<br>
Because most objects are short-lived, Gen 0 collections are very fast.<br>

Important GC Points<br>
GC only manages heap memory<br>
Stack memory is automatically cleared when methods exit<br>
GC runs automatically<br>
Developers usually don't control GC<br>

Short Interview Definition<br>
Garbage Collection is an automatic memory management process that frees heap memory by removing objects that are no longer referenced.<br>

Key Interview Summary<br>
GC → manages heap memory<br>
Unreferenced objects → eligible for GC<br>
Generations → Gen0, Gen1, Gen2<br>
Most collections happen in Gen0<br>
Gen2 collections are expensive
