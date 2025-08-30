What is OOP - paradigm based on objects containing attributes and methods
##### Benefits: 
- code reusability
- modularity
- more security through encapsulation
- real-world modelling
- scalability 

##### Four Pillars of OOP:
1. Encapsulation: grouping of similar data into classes
   - access modifiers:
     - protected - derived classes can access only
     - private - only accessible within class
     - public - everyone can access
     - default - accessible within same package
2. Inheritance: new classes acquire properties of base classes
   - forms a 'is-a' relationship
   - types:
	   - single
	   - multiple
	   - multilevel
	   - hierarchal 
	   - hybrid
3. Polymorphism: multiple forms of a single method
   - compile time: static polymorphism like method overloading operator overloading, happens via changing parameters
   - runtime: dynamic polymorphism like method overriding happens via inheritance and method is resolved at runtime based on object type
     - basically virtual methods are all those methods which are not static, final, private
     - *virtual methods* are those methods which belong to an instance and non-virtual methods are those which belong to the class which means, that those methods are same for every instance of the class
4. Abstraction: data hiding, hiding implementation details 
   - abstract classes - abstract methods, cannot be instantiated
   - abstract methods - no definition only declaration
   - interface - blueprint of class
     - Interfaces are a mechanism to achieve abstraction, as they define what a class should do without specifying how it should do it.

##### Advanced OOP Concepts:
1. Association: relationship between objects, eg: student enrolled in a course
2. Aggregation: weak relation, 'HAS-A', eg: department has a employee
3. Composition: strong, 'PART-OF', eg: room is a part of the house

###### Coupling and Cohesion:
1. Coupling: degree of interdependence, loose coupling good, tight bad
2. Cohesion: how closely related elements within module are, high cohesion is good, low is bad

###### Solid Principles:
1. Single Responsibility: each class should serve only one responsibility 
2. Open/Closed Principle: classes should be open for extension and closed for modification ie. use inheritance if you need to extend the functionality
3. Liskov's Principle: subclasses should be substitutable for base classes ie. continue their functionality without breaking it. eg: when we want to initialise List we can start an object of List with the class of ArrayList<> or LinkedList<>
4. Interface segregation principle: create specific interfaces and not generic ones
5. Dependency Inversion Principle: high level modules shouldn't depend on low level modules

###### Constructor:
1. Default
2. Parametrised
3. Copy constructor -> Student(Student s2){ this.name = s2.name; }
#### Trivia:
Constructor chaining: object of derived class calls base class constructor first then derived class constructor

All attributes and methods in interface are static, final and public

Memory efficient class should make all common attributes as static as static data gets memory assigned only once but non-static data gets new memory for every new object created

---
## Questions:
## BASIC CONCEPTS

### 1. What is Object-Oriented Programming?

- Programming paradigm based on objects
- Objects contain data (attributes) and methods (behavior)
- Models real-world entities
- Four main principles: Encapsulation, Inheritance, Polymorphism, Abstraction
- **Advantages over procedural:**
    - Code reusability
    - Modularity
    - Data security
    - Easy maintenance
    - Problem decomposition

### 2. What are the four pillars of OOP?

- **Encapsulation**: Data hiding, bundling data and methods
- **Inheritance**: Acquiring properties from parent class
- **Polymorphism**: One interface, multiple implementations
- **Abstraction**: Hiding complex implementation details

### 3. Difference between class and object?

- **Class**: Blueprint/template for objects
- **Object**: Instance of class, runtime entity
- Class is logical concept, object is physical
- Class has no memory allocation, objects have memory
- **Can create object without class?** No (in most OOP languages)

### 4. What is encapsulation?

- Bundling data and methods together
- Data hiding from external access
- Controlled access through getters/setters
- **Importance:**
    - Data security
    - Code maintainability
    - Implementation flexibility
    - Reduces coupling

### 5. Access modifiers?

- **Public**: Accessible everywhere
- **Private**: Within same class only
- **Protected**: Within class and subclasses
- **Package/Default**: Within same package
- **No modifier specified**: Default/package access (Java)

### 6. What is inheritance?

- Acquiring properties/methods from parent class
- IS-A relationship
- **Types:**
    - Single: One parent, one child
    - Multiple: Multiple parents, one child
    - Multilevel: Chain inheritance
    - Hierarchical: One parent, multiple children
    - Hybrid: Combination of above
- **Advantages:** Code reuse, extensibility
- **Disadvantages:** Tight coupling, complexity

### 7. What is polymorphism?

- One interface, multiple forms
- **Types:**
    - **Compile-time**: Method overloading, operator overloading
    - **Runtime**: Method overriding, dynamic binding
- **Examples:**
    - Compile-time: `add(int, int)` vs `add(float, float)`
    - Runtime: Different `draw()` methods for Circle, Rectangle

### 8. What is abstraction?

- Hiding implementation complexity
- Showing only essential features
- **Vs Encapsulation:**
    - Abstraction: What to hide (complexity)
    - Encapsulation: How to hide (access control)
- Achieved through abstract classes, interfaces

## INHERITANCE & POLYMORPHISM

### 9. Method overloading vs overriding?

- **Overloading:**
    - Same method name, different parameters
    - Compile-time polymorphism
    - Can have different return types
    - Within same class
- **Overriding:**
    - Same signature, different implementation
    - Runtime polymorphism
    - Must have same return type (or covariant)
    - Requires inheritance
- **Override static method?** No, static methods are hidden, not overridden

### 10. Abstract class vs interface?

- **Abstract Class:**
    - Can have concrete methods
    - Can have constructors
    - Can have instance variables
    - Single inheritance
    - 0-100% abstraction
- **Interface:**
    - All methods abstract (traditionally)
    - No constructors
    - Only constants (public static final)
    - Multiple inheritance
    - 100% abstraction
- **When to use:**
    - Abstract class: Common base with some implementation
    - Interface: Contract definition, multiple inheritance needed

### 11. Override private methods?

- **No**, private methods not inherited
- Cannot be overridden because not visible to subclass
- **Protected methods:** Yes, can be overridden

### 12. Multiple inheritance in Java?

- **Classes:** No, Java doesn't support multiple class inheritance
- **Interfaces:** Yes, class can implement multiple interfaces
- **Reason:** Diamond problem - ambiguity in method calls
- **Java 8+:** Default methods in interfaces can cause diamond problem

### 13. Super keyword?

- References parent class
- **Uses:**
    - Call parent constructor: `super()`
    - Call parent method: `super.methodName()`
    - Access parent variable: `super.variableName`
- **this() vs super():**
    - this(): Call constructor of same class
    - super(): Call parent class constructor
    - Both must be first statement

### 14. Instantiate abstract class?

- **No**, abstract classes cannot be instantiated
- Can create reference variables
- **Interface:** No, cannot instantiate interfaces either
- Can create anonymous implementations

### 15. Dynamic method dispatch?

- Runtime method resolution
- JVM determines actual method to call based on object type
- Uses virtual method table (vtable)
- Enables runtime polymorphism

## CONSTRUCTORS & DESTRUCTORS

### 16. Constructor types?

- **Default**: No parameters, provided by compiler if none exists
- **Parameterized**: Takes parameters
- **Copy**: Creates object from another object
- **Cannot be inherited**: But can be chained using super()

### 17. Constructor chaining?

- Calling one constructor from another
- **Same class**: this()
- **Parent class**: super()
- If no explicit super(), default parent constructor called
- **Must be first statement** in constructor

### 18. Private constructors?

- **Yes**, constructors can be private
- **Use cases:**
    - Singleton pattern
    - Utility classes
    - Factory pattern
- **Object creation:** Through public static methods

### 19. Copy constructor?

- Creates new object as copy of existing object
- **C++:** Supported natively
- **Java:** Not directly supported, clone() method used
- **Deep vs shallow copy** consideration

### 20. Destructor/finalizer?

- **C++:** Destructor called when object destroyed
- **Java:** finalize() method, called by garbage collector
- **C#:** Destructor syntax, actually finalizer
- **Cannot force garbage collection** reliably

## RELATIONSHIPS & ASSOCIATIONS

### 21. Composition vs aggregation?

- **Composition:**
    - Strong "part-of" relationship
    - Child cannot exist without parent
    - Example: House-Room
    - Stronger coupling
- **Aggregation:**
    - Weak "has-a" relationship
    - Child can exist independently
    - Example: Department-Employee
    - Weaker coupling

### 22. Association?

- Relationship between objects
- **Types:**
    - One-to-one
    - One-to-many
    - Many-to-many
- Can be bidirectional or unidirectional
- Weakest form of relationship

### 23. IS-A vs HAS-A?

- **IS-A**: Inheritance relationship
    - Example: Car IS-A Vehicle
    - Uses extends/implements
- **HAS-A**: Composition/aggregation
    - Example: Car HAS-A Engine
    - Uses instance variables

### 24. Tight vs loose coupling?

- **Tight coupling:**
    - High dependency between classes
    - Changes in one affect others
    - Difficult to maintain
- **Loose coupling:**
    - Low dependency
    - Independent modules
    - Easy to maintain and test
- **Reduce coupling:** Interfaces, dependency injection, abstract classes

### 25. Cohesion?

- How closely related elements in module are
- **High cohesion:** Elements work for single purpose (good)
- **Low cohesion:** Elements serve multiple purposes (bad)
- **Example high cohesion:** Calculator class with only math operations

## ADVANCED CONCEPTS

### 26. SOLID principles?

- **S - Single Responsibility:** One reason to change
- **O - Open/Closed:** Open for extension, closed for modification
- **L - Liskov Substitution:** Subclasses should be substitutable
- **I - Interface Segregation:** Many specific interfaces better than one general
- **D - Dependency Inversion:** Depend on abstractions, not concretions

### 27. Singleton pattern?

- Ensures only one instance of class
- **Implementation:**
    - Private constructor
    - Static instance variable
    - Public static method to get instance
- **Thread-safe:** Use synchronization, double-checked locking, or enum

### 28. Factory pattern?

- Creates objects without specifying exact class
- **Benefits:**
    - Loose coupling
    - Code reusability
    - Easy to extend
- **vs Direct creation:** More flexible, centralizes object creation logic

### 29. Observer pattern?

- One-to-many dependency between objects
- When one changes, all dependents notified
- **Example:** Newsletter subscription, MVC architecture
- **Components:** Subject, Observer, ConcreteSubject, ConcreteObserver

### 30. Dependency injection?

- Providing dependencies from external source
- **Types:** Constructor, setter, interface injection
- **Benefits:**
    - Loose coupling
    - Easy testing
    - Better maintainability

## MEMORY & PERFORMANCE

### 31. Object storage in memory?

- **Objects:** Stored in heap
- **References:** Stored in stack (local) or heap (instance variables)
- **Stack:** Fast access, automatic cleanup
- **Heap:** Slower access, garbage collection needed

### 32. Garbage collection?

- Automatic memory management
- Reclaims memory of unreachable objects
- **Algorithms:** Mark and sweep, generational, reference counting
- **Cannot force reliably:** System.gc() is suggestion, not guarantee

### 33. Object out of scope?

- Reference becomes unreachable
- Eligible for garbage collection
- Memory not immediately freed
- **Language specific behavior** varies

### 34. Memory leaks in OOP?

- Objects not eligible for garbage collection despite being unused
- **Causes:**
    - Static collections holding references
    - Event listeners not removed
    - Circular references (in some languages)
- **Prevention:** Proper resource management, weak references

## LANGUAGE-SPECIFIC

### 35. Java multiple inheritance?

- **Not supported for classes** due to diamond problem
- **Diamond problem:** Ambiguity when inheriting same method from multiple parents
- **Solution:** Single class inheritance, multiple interface implementation

### 36. Final keyword in Java?

- **Class:** Cannot be extended
- **Method:** Cannot be overridden
- **Variable:** Cannot be reassigned (constant)
- **Override final method?** No, compilation error

### 37. Static keyword?

- Belongs to class, not instance
- Shared among all objects
- **Override static methods?** No, they are hidden (method hiding)
- **Method hiding:** Subclass method hides parent static method

### 38. Inner classes in Java?

- **Types:**
    - Member inner class
    - Static nested class
    - Local inner class
    - Anonymous inner class
- Can access outer class members (non-static inner classes)

### 39. Autoboxing/unboxing?

- **Autoboxing:** Automatic conversion primitive to wrapper
- **Unboxing:** Automatic conversion wrapper to primitive
- **Wrapper classes:** Integer, Double, Boolean, etc.

### 40. Virtual function (C++)?

- Enables runtime polymorphism
- Resolved at runtime using vtable
- **Pure virtual:** Virtual function with no implementation (= 0)
- Makes class abstract

### 41. Virtual destructor (C++)?

- Ensures proper cleanup in inheritance hierarchy
- Called when deleting object through base pointer
- **Without virtual destructor:** Only base destructor called, memory leak

### 42. Function overloading vs overriding (C++)?

- **Same as method overloading/overriding**
- **Operator overloading:** Redefining operators for user-defined types
- C++ supports operator overloading

### 43. Override vs new (C#)?

- **Override:** True polymorphism, overrides virtual method
- **New:** Method hiding, hides base method
- **Method hiding:** Base method still accessible through base reference

### 44. Properties (C#)?

- Provide controlled access to fields
- **Get/Set accessors**
- **vs Fields:** Properties can have logic, validation
- Syntactic sugar for getter/setter methods

## PRACTICAL/CODING

### 45. Class hierarchy design?

```
Vehicle (abstract)
├── Car
│   └── SportsCar
├── Motorcycle
└── Truck
```

- Use inheritance for IS-A relationships
- Include abstract methods in base class
- Override methods in derived classes

### 46. Singleton implementation?

java

```java
public class Singleton {
    private static volatile Singleton instance;
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### 47. Polymorphism example?

- Shape base class with draw() method
- Circle, Rectangle inherit and override draw()
- Runtime polymorphism through Shape reference

### 48. Interface implementation?

- Define contract with method signatures
- Classes implement interface methods
- Multiple classes can implement same interface differently

### 49. Overloading vs overriding demo?

- **Overloading:** Multiple add() methods with different parameters
- **Overriding:** Parent display() method overridden in child

### 50. Library management system?

- **Classes:** Book, Member, Library, Transaction
- **Relationships:** Library has Books and Members
- **Inheritance:** Different types of Books (TextBook, Novel)
- **Polymorphism:** Different display methods for book types

## TRICKY/CONCEPTUAL

### 51. Is Java purely object-oriented?

- **No**, has primitive types (int, char, boolean)
- Static methods don't belong to objects
- Procedural programming possible with static methods
- **C# similar issues**

### 52. Multiple inheritance through interfaces?

- **Yes**, but can lead to diamond problem with default methods
- **Java 8+:** Default methods in interfaces
- Resolution rules determine which method is called

### 53. Multiple inheritance complexity?

- **Diamond problem:** Ambiguous method calls
- **Solutions:**
    - Virtual inheritance (C++)
    - Interface-only multiple inheritance (Java, C#)
    - Method resolution order (Python)

### 54. Liskov Substitution Principle?

- Subtypes must be substitutable for base types
- **Violation example:** Rectangle-Square problem
- Square overriding Rectangle's setWidth/setHeight breaks expectations

### 55. Method resolution in multiple inheritance?

- **C++:** Uses scope resolution, virtual inheritance
- **Python:** Method Resolution Order (MRO), C3 linearization
- **Java:** Interfaces use resolution rules for conflicts

## SCENARIO-BASED

### 56. Composition over inheritance?

- **When:**
    - HAS-A relationship more natural than IS-A
    - Need multiple inheritance behavior
    - Want to avoid fragile base class problem
- **Example:** Car has Engine vs Car extends Vehicle

### 57. Add features without modifying existing code?

- **Open/Closed Principle**
- **Strategy Pattern:** Different algorithms
- **Plugin architecture**
- **Interface-based design**

### 58. Create different object types based on input?

- **Factory Pattern**
- **Abstract Factory Pattern**
- **Builder Pattern** for complex objects
- Switch based on input type

### 59. Single database connection instance?

- **Singleton Pattern**
- **Connection pooling** for multiple connections
- **Thread safety** considerations

### 60. Notification system design?

- **Observer Pattern:** Notify all subscribers
- **Strategy Pattern:** Different notification methods
- **Factory Pattern:** Create appropriate notifier
- **Command Pattern:** Encapsulate notification requests

## RAPID-FIRE

### 61. Can abstract class have constructor?

- **Yes**

### 62. Can interface have constructor?

- **No** (in Java/C#)

### 63. Can we override main method?

- **No**, main is static

### 64. What is method signature?

- Method name + parameter list

### 65. Can constructor return value?

- **No**, constructors don't have return type

### 66. Is constructor inherited?

- **No**, but can be chained

### 67. Can static methods be overridden?

- **No**, they can be hidden

### 68. Early vs late binding?

- **Early binding:** Compile-time resolution (overloading)
- **Late binding:** Runtime resolution (overriding)

### 69. Private virtual methods?

- **C++:** Yes, but unusual
- **Java:** No virtual keyword
- **C#:** No, private methods can't be virtual

### 70. Covariant return type?

- Subclass can return more specific type than parent method
- **Example:** Parent returns Animal, child returns Dog