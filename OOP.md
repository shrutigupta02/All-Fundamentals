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