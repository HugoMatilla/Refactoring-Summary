This is my summary of the "Refactoring: Improving the Design of Existing Code" by Martin Fowler. I use it while learning and as quick reference. It is not intended to be an standalone substitution of the book so if you really want to learn the concepts here presented, buy and read the book and use this repository as a reference and guide.

If you are the publisher and think this repository should not be public, just write me an email at hugomatilla [at] gmail [dot] com and I will make it private.

Contributions: Issues, comments and pull requests are super welcome :smiley:

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

# 1. TABLE OF CONTENT
- [1. TABLE OF CONTENT](#1-table-of-content)
- [3. BAD SMELLS IN CODE](#3-bad-smells-in-code)
	- [1. Duplicated code](#1-duplicated-code)
	- [2. Long Method](#2-long-method)
	- [3. Large Classes](#3-large-classes)
	- [4. Long Parameter List](#4-long-parameter-list)
	- [5. Divergent Change](#5-divergent-change)
	- [6. Shotgun Surgery](#6-shotgun-surgery)
	- [7. Feature Envy](#7-feature-envy)
	- [8. Data Clumps](#8-data-clumps)
	- [9. Primitive Obsession](#9-primitive-obsession)
	- [10. Switch Statements](#10-switch-statements)
	- [11. Parallel Inheritance Hierarchies](#11-parallel-inheritance-hierarchies)
	- [12. Lazy Class](#12-lazy-class)
	- [13. Speculative Generality](#13-speculative-generality)
	- [14. Temporary Field](#14-temporary-field)
	- [15. Message Chain](#15-message-chain)
	- [16. Middle Man](#16-middle-man)
	- [17. Inappropriate Intimacy](#17-inappropriate-intimacy)
	- [18. Alternative Classes with Different Interfaces](#18-alternative-classes-with-different-interfaces)
	- [19. Incomplete Library Class](#19-incomplete-library-class)
	- [20. Data Class](#20-data-class)
	- [21. Refused Bequest](#21-refused-bequest)
	- [22. Comments](#22-comments)
- [6. COMPOSING METHODS](#6-composing-methods)
	- [1. Extract Method](#1-extract-method)
	- [2. Inline Method](#2-inline-method)
	- [3. Inline Temp](#3-inline-temp)
	- [4. Replace Temp with Query](#4-replace-temp-with-query)
	- [5. Introduce Explaining Variable](#5-introduce-explaining-variable)
	- [6. Split Temporary Variable](#6-split-temporary-variable)
	- [7. Remove Assignments to Parameters](#7-remove-assignments-to-parameters)
	- [8. Replace Method with Method Object](#8-replace-method-with-method-object)
	- [9. Substitute Algorithm](#9-substitute-algorithm)
- [7. Moving features between elements](#7-moving-features-between-elements)
	- [10. Move method](#10-move-method)
	- [11. Move field](#11-move-field)
	- [12. Extract Class](#12-extract-class)
	- [13. Inline Class](#13-inline-class)
	- [14. Hide Delegate](#14-hide-delegate)
	- [15. Remove Middle Man](#15-remove-middle-man)
	- [16. Introduce Foreign Method](#16-introduce-foreign-method)
	- [17. Introduce Local Extension](#17-introduce-local-extension)
- [8. ORGANIZING DATA](#8-organizing-data)
	- [18. Self Encapsulate Field](#18-self-encapsulate-field)
	- [19. Replace Data Value with Object](#19-replace-data-value-with-object)
	- [20. Change Value to Reference](#20-change-value-to-reference)
	- [21. Change Reference to Value](#21-change-reference-to-value)
	- [22. Replace Array with Object](#22-replace-array-with-object)
	- [23. Duplicate Observed Data](#23-duplicate-observed-data)
	- [24. Change Unidirectional Association to Bidirectional](#24-change-unidirectional-association-to-bidirectional)
	- [25. Change Bidirectional Association to Unidirectional](#25-change-bidirectional-association-to-unidirectional)
	- [26. Replace Magic Number with Symbolic Constant](#26-replace-magic-number-with-symbolic-constant)
	- [27. Encapsulate Field](#27-encapsulate-field)
	- [28. Encapsulate Collection](#28-encapsulate-collection)
	- [29. Remove Record with data class](#29-remove-record-with-data-class)
	- [30. Replace Type Code with Class](#30-replace-type-code-with-class)
	- [31. Replace Type Code with Subclasses](#31-replace-type-code-with-subclasses)
	- [32. Replace Type Code with State/Strategy](#32-replace-type-code-with-statestrategy)
	- [32. Replace Subclass with Fields](#32-replace-subclass-with-fields)
- [9. SIMPLIFYING CONDITIONAL EXPRESSIONS](#9-simplifying-conditional-expressions)
	- [33. Decompose Conditional](#33-decompose-conditional)
	- [34. Consolidate Conditional Expression](#34-consolidate-conditional-expression)
	- [35. Consolidate Duplicate Conditional Fragments](#35-consolidate-duplicate-conditional-fragments)
	- [36. Remove Control Flag](#36-remove-control-flag)
	- [37. Replace Nested Conditional with Guard Clauses](#37-replace-nested-conditional-with-guard-clauses)
	- [38. Replace Conditional with Polymorphism](#38-replace-conditional-with-polymorphism)
	- [39. Introduce Null Object](#39-introduce-null-object)
	- [40. Introduce Assertion](#40-introduce-assertion)
- [10. MAKING METHOD CALLS SIMPLER](#10-making-method-calls-simpler)
	- [41. Rename method](#41-rename-method)
	- [42. Add Parameter](#42-add-parameter)
	- [43. Remove Parameter](#43-remove-parameter)
	- [44. Separate Query from Modifier](#44-separate-query-from-modifier)
	- [45. Parameterize Method](#45-parameterize-method)
	- [46. Replace Parameter with Explicit Methods](#46-replace-parameter-with-explicit-methods)
	- [47. Preserve Whole Object](#47-preserve-whole-object)
	- [48. Replace Parameter with Method](#48-replace-parameter-with-method)
	- [49. Introduce Parameter Object](#49-introduce-parameter-object)
	- [50. Remove Setting Method](#50-remove-setting-method)
	- [51. Hide Method](#51-hide-method)
	- [52. Replace Constructor with Factory Method](#52-replace-constructor-with-factory-method)
	- [53. Encapsulate Downcast](#53-encapsulate-downcast)
	- [54. Replace Error Code with Exception](#54-replace-error-code-with-exception)
	- [55. Replace Exception with Test](#55-replace-exception-with-test)
- [11. DEALING WITH GENERALIZATION](#11-dealing-with-generalization)
	- [56. Pull up field](#56-pull-up-field)
	- [57. Pull Up Method](#57-pull-up-method)
	- [58. Pull Up Constructor Body](#58-pull-up-constructor-body)
	- [59. Push Down Method](#59-push-down-method)
	- [60. Push Down Field](#60-push-down-field)
	- [61. Extract Subclass](#61-extract-subclass)
	- [62. Extract Superclass](#62-extract-superclass)
	- [63. Extract Interface](#63-extract-interface)
	- [64. Collapse Hierarchy](#64-collapse-hierarchy)
	- [65. Form Template Method](#65-form-template-method)
	- [66. Replace Inheritance with Delegation](#66-replace-inheritance-with-delegation)
	- [67. Replace Delegation with Inheritance](#67-replace-delegation-with-inheritance)
- [12. BIG REFACTORINGS](#12-big-refactorings)
	- [68. Tease Apart Inheritance](#68-tease-apart-inheritance)
	- [69. Convert Procedural Design to Objects](#69-convert-procedural-design-to-objects)
	- [70. Separate Domain from Presentation](#70-separate-domain-from-presentation)
	- [71. Extract Hierarchy](#71-extract-hierarchy)

<!-- /TOC -->

# 3. BAD SMELLS IN CODE
### 1. Duplicated code
Same code structure in more than one place.
### 2. Long Method
The longer a procedure is the more difficult is to understand.
### 3. Large Classes
When a class is trying to do too much, duplicated code cannot be far behind.
### 4. Long Parameter List
The are hard to understand, inconsistent and difficult to use.
### 5. Divergent Change
When one class is commonly changed in different ways for different reasons.
### 6. Shotgun Surgery
When every time you make a kind of change, you have to make a lot of little changes to a lot of different classes.
### 7. Feature Envy
A method that seems more interested in a class other than the one it actually is in.
### 8. Data Clumps
Bunches of data(fields, parameters...) that hang around together.
### 9. Primitive Obsession
Using primitives types instead of small objects.
### 10. Switch Statements
The same switch statement scattered about a program in different places. Use polymorphism.
### 11. Parallel Inheritance Hierarchies
Every time you make a subclass of one class, you also have to make a subclass of another.
### 12. Lazy Class
A class that isn't doing enough to pay for itself should be eliminated.
### 13. Speculative Generality
All sorts of hooks and special cases to handle things that aren't required.
### 14. Temporary Field
An instance variable that is set only in certain circumstances.
### 15. Message Chain
When a client asks one object for another object, which the client then asks for yet another object...
### 16. Middle Man
When an object delegates much of its functionality.
### 17. Inappropriate Intimacy
When classes access to much to another classes.
### 18. Alternative Classes with Different Interfaces
Classes with methods that look to similar.
### 19. Incomplete Library Class
When we need extra features in libraries.
### 20. Data Class
Don't allow manipulation in Data Classes. Use encapsulation and immutability.
### 21. Refused Bequest
Subclasses that don't make uses of parents methods.
### 22. Comments
Not all comments but the ones that are there because the code is bad.

# 6. COMPOSING METHODS
## 1. Extract Method

You have a code fragment that can be grouped together.

```java

	void printOwing(double amount) {
		printBanner();
		//print details
		System.out.println ("name:" + _name);
		System.out.println ("amount" + amount);
	}
```

to

```java

	void printOwing(double amount) {
		printBanner();
		printDetails(amount);
	}

	void printDetails (double amount) {
		System.out.println ("name:" + _name);
	System.out.println ("amount" + amount);
	}
```

**Motivation**

* Increases the chances that other methods can use a method
* Allows the higher-level methods to read more like a series of comments

```java

	void printOwing(double previousAmount) {
		Enumeration e = _orders.elements();
		double outstanding = previousAmount * 1.2;
		printBanner();

		// calculate outstanding
		while (e.hasMoreElements()) {
			Order each = (Order) e.nextElement();
			outstanding += each.getAmount();
		}
		printDetails(outstanding);
	}
```

to

```java

	void printOwing(double previousAmount) {
		printBanner();
		double outstanding = getOutstanding(previousAmount * 1.2);
		printDetails(outstanding);
	}

	double getOutstanding(double initialValue) {
		double result = initialValue;
		Enumeration e = _orders.elements();

		while (e.hasMoreElements()) {
			Order each = (Order) e.nextElement();
			result += each.getAmount();
		}
		return result;
	}
```

## 2. Inline Method
A method's body is just as clear as its name.

```java

	int getRating() {
		return (moreThanFiveLateDeliveries()) ? 2 : 1;
	}

	boolean moreThanFiveLateDeliveries() {
		return _numberOfLateDeliveries > 5;
	}
```

to

```java

	int getRating() {
		return (_numberOfLateDeliveries > 5) ? 2 : 1;
	}
```
**Motivation**

* When Indirection is needless (simple delegation) becomes irritating.
* If group of methods are badly factored and grouping them makes it clearer


## 3. Inline Temp
You have a temp that is assigned to once with a simple expression, and the temp is getting in the way of other refactorings.

```java

	double basePrice = anOrder.basePrice();
	return (basePrice > 1000)
```

to

```java

	return (anOrder.basePrice() > 1000)
```
**Motivation**

* Use it with [4. Replace Temp with Query](#4-replace-temp-with-query)


## 4. Replace Temp with Query
You are using a temporary variable to hold the result of an expression.
```java

	double basePrice = _quantity * _itemPrice;
	if (basePrice > 1000)
		return basePrice * 0.95;
	else
		return basePrice * 0.98;
```
to

```java

	if (basePrice() > 1000)
		return basePrice() * 0.95;
	else
		return basePrice() * 0.98;
	...
	double basePrice() {
		return _quantity * _itemPrice;
	}
```

**Motivation**

* Replacing the temp with a query method, any method in the class can get at the information.
* Is a vital step before [1. Extract Method](#1-extract-method)

## 5. Introduce Explaining Variable
You have a complicated expression
```java

	if ( (platform.toUpperCase().indexOf("MAC") > -1) &&
		(browser.toUpperCase().indexOf("IE") > -1) &&
		wasInitialized() && resize > 0 )
	{
		// do something
	}
```
to

```java

	final boolean isMacOs = platform.toUpperCase().indexOf("MAC") >-1;
	final boolean isIEBrowser = browser.toUpperCase().indexOf("IE") >-1;
	final boolean wasResized = resize > 0;
	if (isMacOs && isIEBrowser && wasInitialized() && wasResized) {
		// do something
	}

```

**Motivation**

* When expressions are hard to read

## 6. Split Temporary Variable
You have a temporary variable assigned to more than once, but is not a loop variable nor a collecting temporary variable.

```java

	double temp = 2 * (_height + _width);
	System.out.println (temp);
	temp = _height * _width;
	System.out.println (temp);
```
to
```java

	final double perimeter = 2 * (_height + _width);
	System.out.println (perimeter);
	final double area = _height * _width;
	System.out.println (area);
```

**Motivation**

* Variables should not have more than one responsibility.
* Using a temp for two different things is very confusing for the reader.

## 7. Remove Assignments to Parameters
The code assign to a parameter
```java

	int discount (int inputVal, int quantity, int yearToDate) {
		if (inputVal > 50) inputVal -= 2;
```
to
```java

	int discount (int inputVal, int quantity, int yearToDate) {
		int result = inputVal;
		if (inputVal > 50) result -= 2;
```

**Motivation**

* You can change the internals of  object is passed but do not point to another object.
* Use only the parameter to represent what has been passed.

## 8. Replace Method with Method Object
You have a long method that uses local variables in such a way that you cannot apply Extract Method.

```java

	class Order...
		double price() {
			double primaryBasePrice;
			double secondaryBasePrice;
			double tertiaryBasePrice;
			// long computation;
			...
		}
```
to
```java

	class Order...
		double price(){
			return new PriceCalculator(this).compute()
		}
	}

	class PriceCalculato...
	compute(){
		double primaryBasePrice;
		double secondaryBasePrice;
		double tertiaryBasePrice;
		// long computation;
		return ...
	}

```

**Motivation**

* When a method has lot of local variables and applying decomposition is not possible

_This sample does not really needs this refactoring, but shows the way to do it._
```java

	Class Account
		int gamma (int inputVal, int quantity, int yearToDate) {
			int importantValue1 = (inputVal * quantity) + delta();
			int importantValue2 = (inputVal * yearToDate) + 100;
			if ((yearToDate - importantValue1) > 100)
			importantValue2 -= 20;
			int importantValue3 = importantValue2 * 7;
			// and so on.
			return importantValue3 - 2 * importantValue1;
		}
	}
```
to
```java

	class Gamma...
		private final Account _account;
		private int inputVal;
		private int quantity;
		private int yearToDate;
		private int importantValue1;
		private int importantValue2;
		private int importantValue3;

		Gamma (Account source, int inputValArg, int quantityArg, int yearToDateArg) {
			_account = source;
			inputVal = inputValArg;
			quantity = quantityArg;
			yearToDate = yearToDateArg;
		}

		int compute () {
			importantValue1 = (inputVal * quantity) + _account.delta();
			importantValue2 = (inputVal * yearToDate) + 100;
			if ((yearToDate - importantValue1) > 100)
			importantValue2 -= 20;
			int importantValue3 = importantValue2 * 7;
			// and so on.
			return importantValue3 - 2 * importantValue1;
		}

		int gamma (int inputVal, int quantity, int yearToDate) {
			return new Gamma(this, inputVal, quantity,yearToDate).compute();
		}
```

## 9. Substitute Algorithm
You want to replace an algorithm with one that is clearer.

```java

	String foundPerson(String[] people){
		for (int i = 0; i < people.length; i++) {
			if (people[i].equals ("Don")){
				return "Don";
			}
			if (people[i].equals ("John")){
				return "John";
			}
			if (people[i].equals ("Kent")){
				return "Kent";
			}
		}
		return "";
	}
```
to
```java

	String foundPerson(String[] people){
		List candidates = Arrays.asList(new String[] {"Don", "John","Kent"});
	for (int i = 0; i<people.length; i++)
		if (candidates.contains(people[i]))
			return people[i];
	return "";
	}
```

**Motivation**

* Break down something complex into simpler pieces.
* Makes easier apply changes to the algorithm.
* Substituting a large, complex algorithm is very difficult; making it simple can make the substitution tractable.


# 7. Moving features between elements
## 10. Move method
A method is, or will be, using or used by more features of another class than the class on which it is defined.  

_Create a new method with a similar body in the class it uses most. Either turn the old method into a simple delegation, or remove it altogether._  

```java

	class Class1 {
		aMethod()
	}

	class Class2 {	}
```
to


```java

	class Class1 {	}

	class Class2 {
		aMethod()
	}
```

**Motivation**

When classes do to much work or when collaborate too much and are highly coupled

## 11. Move field
A field is, or will be, used by another class more than the class on which it is defined.
_Create a new field in the target class, and change all its users._

```java

	class Class1 {
		aField
	}

	class Class2 {	}
```
to


```java

	class Class1 {	}

	class Class2 {
		aField
	}
```

**Motivation**

If a field is used mostly by outer classes and before [12. Extract Class](#12-extract-class)


## 12. Extract Class
You have one class doing work that should be done by two.
_Create a new class and move the relevant fields and methods from the old class into the new class._
```java

	class Person {
		name,
		officeAreaCode,
		officeNumber,
		getTelephoneNumber()
	}
```
to
```java

	class Person {
		name,
		getTelephoneNumber()
	}

	class TelephoneNumber {
		areaCode,
		number,
		getTelephoneNumber()
	}
```

**Motivation**

Classes grow.
_Split them when_:

* subsets of methods seem to be together
* subsets of data usually change together or depend on each other

## 13. Inline Class
A class isn't doing very much.
_Move all its features into another class and delete it._
```java

	class Person {
		name,
		getTelephoneNumber()
	}

	class TelephoneNumber {
		areaCode,
		number,
		getTelephoneNumber()
	}
```
to
```java

	class Person {
		name,
		officeAreaCode,
		officeNumber,
		getTelephoneNumber()
	}
```


**Motivation**

After refactoring normally there are a bunch of responsibilities moved out of the class, letting the class with little left.

## 14. Hide Delegate
A client is calling a delegate class of an object.  
_Create methods on the server to hide the delegate._
```java

	class ClientClass {
		//Dependencies
		Person person = new Person()
		Department department = new Department()
		person.doSomething()
		department.doSomething()
	}
```
to
```java

	class ClientClass {
		Person person = new Person()
		person.doSomething()
	}

	class Person{
		Department department = new Department()
		department.doSomething()
	}
```

_The solution_
```java

	class ClientClass{
		Server server = new Server()
		server.doSomething()
	}

	class Server{
		Delegate delegate = new Delegate()
		void doSomething(){
			delegate.doSomething()
		}
	}
	// The delegate is hidden to the client class.
	// Changes won't be propagated to the client they will only affect the server
	class Delegate{
		void doSomething(){...}
	}

```

**Motivation**

Key of encapsulation.
Classes should know as less as possible of other classes.

`> manager = john.getDepartment().getManager();`
```java



	class Person {
		Department _department;
		public Department getDepartment() {
			return _department;
		}
		public void setDepartment(Department arg) {
			_department = arg;
			}
		}

	class Department {
		private String _chargeCode;
		private Person _manager;
		public Department (Person manager) {
			_manager = manager;
		}
		public Person getManager() {
			return _manager;
		}
		...
```
to

`> manager = john.getManager();`
```java

	class Person {
		...
		public Person getManager() {
			return _department.getManager();
		}
	}
```

## 15. Remove Middle Man
A class is doing too much simple delegation.
_Get the client to call the delegate directly._
```java

	class ClientClass {
		Person person = new Person()
		person.doSomething()
	}

	class Person{
		Department department = new Department()
		department.doSomething()
	}
```
to

```java

	class ClientClass {
		//Dependencies
		Person person = new Person()
		Department department = new Department()
		person.doSomething()
		department.doSomething()
	}
```


**Motivation**

When the "Middle man" (the server) does to much is time for the client to call the delegate directly.

## 16. Introduce Foreign Method
A server class you are using needs an additional method, but you can't modify the class.  
_Create a method in the client class with an instance of the server class as its first argument._

```java

	Date newStart = new Date(previousEnd.getYear(),previousEnd.getMonth(),previousEnd.getDate()+1);
```
to
```java

	Date newStart = nextDay(previousEnd);

	private static Date nextDay(Date date){
		return new Date(date.getYear(),date.getMonth(),date.getDate()+1);
	}
```

**Motivation**

When there is a lack of a method in class that you use a lot and you can not change that class.

## 17. Introduce Local Extension
A server class you are using needs several additional methods, but you can't modify the class.
_Create a new class that contains these extra methods. Make this extension class a subclass or a wrapper of the original._
```java

	class ClientClass(){

		Date date = new Date()
		nextDate = nextDay(date);

		private static Date nextDay(Date date){
			return new Date(date.getYear(),date.getMonth(),date.getDate()+1);
		}
	}
```
to
```java

	class ClientClass() {
		MfDate date = new MfDate()
		nextDate = nextDate(date)
	}
	class MfDate() extends Date {
		...
		private static Date nextDay(Date date){
			return new Date(date.getYear(),date.getMonth(),date.getDate()+1);
		}
	}
```

**Motivation**

When plenty of [16. Introduce Foreign Method](#16-introduce-foreign-method) need to be added to a class.


# 8. ORGANIZING DATA
## 18. Self Encapsulate Field
You are accessing a field directly, but the coupling to the field is becoming awkward.   
_Create getting and setting methods for the field and use only those to access the field._  
```java

	private int _low, _high;
	boolean includes (int arg) {
		return arg >= _low && arg <= _high;
	}
```
to

```java

	private int _low, _high;
	boolean includes (int arg) {
		return arg >= getLow() && arg <= getHigh();
	}
	int getLow() {return _low;}
	int getHigh() {return _high;}
```

**Motivation**  
Allows a subclass to override how to get that information with a method and that it supports more flexibility in managing the data, such as lazy initialization.

## 19. Replace Data Value with Object
You have a data item that needs additional data or behavior.  
_Turn the data item into an object_

```java

	class Order...{
		private String _customer;
		public Order (String customer) {
			_customer = customer;
		}
	}
```
to
```java

	class Order...{
		public Order (String customer) {
			_customer = new Customer(customer);
		}
	}

	class Customer {
		public Customer (String name) {
			_name = name;
		}
	}
```
**Motivation**  
Simple data items  aren't so simple anymore.

## 20. Change Value to Reference
You have a class with many equal instances that you want to replace with a single object.  
_Turn the object into a reference object._
```java

	class Order...{
		public Order (String customer) {
			_customer = new Customer(customer);
		}
	}

	class Customer {
		public Customer (String name) {
			_name = name;
		}
	}
```
to
```java

	//Use Factory Method
	class Customer...
		static void loadCustomers() {
			new Customer ("Lemon Car Hire").store();
			new Customer ("Associated Coffee Machines").store();
			new Customer ("Bilston Gasworks").store();
		}
		private void store() {
			_instances.put(this.getName(), this);
		}
		public static Customer create (String name) {
			return (Customer) _instances.get(name);
		}
```
**Motivation**  
Reference objects are things like customer or account. Each object stands for one object in the real world, and you use the object identity to test whether they are equal.

## 21. Change Reference to Value
You have a reference object that is small, immutable, and awkward to manage.  
_Turn it into a value object_
```java

	new Currency("USD").equals(new Currency("USD")) // returns false
```
to
```java
	
	// Add `equals` and `hashCode` to the Currency class 
	class Currency{ 
		...
		public boolean equals(Object arg) {
	 		if (! (arg instanceof Currency)) return false;
	 		Currency other = (Currency) arg;
			return (_code.equals(other._code));
		}

		public int hashCode() {
 			return _code.hashCode();
 		}
 	}

 	// Now you can do
	new Currency("USD").equals(new Currency("USD")) // now returns true
```
**Motivation**  
Working with the reference object becomes awkward. The reference object is immutable and small. Used in distributed or concurrent systems.

## 22. Replace Array with Object
You have an array in which certain elements mean different things.  
_Replace the array with an object that has a field for each element_
```java

	String[] row = new String[3];
	row [0] = "Liverpool";
	row [1] = "15";
```
to
```java

	Performance row = new Performance();
	row.setName("Liverpool");
	row.setWins("15");
```
**Motivation**  
Arrays should be used only to contain a collection of similar objects in some order.

## 23. Duplicate Observed Data
You have domain data available only in a GUI control, and domain methods need access.  
_Copy the data to a domain object. Set up an observer to synchronize the two pieces of data_  
**Motivation**  
 To separate code that handles the user interface from code that handles the business logic.
## 24. Change Unidirectional Association to Bidirectional
You have two classes that need to use each other's features, but there is only a one-way link.  
_Add back pointers, and change modifiers to update both sets_
```java

	class Order...
		Customer getCustomer() {
			return _customer;
		}
		void setCustomer (Customer arg) {
			_customer = arg;
		}
		Customer _customer;
	}
```
to
```java

	class Order...
		Customer getCustomer() {
			return _customer;
		}
		void setCustomer (Customer arg) {
			if (_customer != null) _customer.friendOrders().remove(this);
			_customer = arg;
			if (_customer != null) _customer.friendOrders().add(this);
		}
		private Customer _customer;

		class Customer...
			void addOrder(Order arg) {
				arg.setCustomer(this);
			}
			private Set _orders = new HashSet();

			Set friendOrders() {
				/** should only be used by Order */
				return _orders;
			}
		}
	}

	// Many to Many
	class Order... //controlling methods
		void addCustomer (Customer arg) {
			arg.friendOrders().add(this);
			_customers.add(arg);
		}
		void removeCustomer (Customer arg) {
			arg.friendOrders().remove(this);
			_customers.remove(arg);
		}
	class Customer...
		void addOrder(Order arg) {
			arg.addCustomer(this);
		}
		void removeOrder(Order arg) {
			arg.removeCustomer(this);
		}
	}
```
**Motivation**  
When the object referenced needs access access to the object that refer to it.

## 25. Change Bidirectional Association to Unidirectional
You have a two-way association but one class no longer needs features from the other.  
_Drop the unneeded end of the association_  
**Motivation**  
If Bidirectional association is not needed, reduce complexity, handle zombie objects, eliminate interdependency
## 26. Replace Magic Number with Symbolic Constant
You have a literal number with a particular meaning.  
_Create a constant, name it after the meaning, and replace the number with it_
```java

	double potentialEnergy(double mass, double height) {
		return mass * 9.81 * height;
	}
```
to
```java

	double potentialEnergy(double mass, double height) {
		return mass * GRAVITATIONAL_CONSTANT * height;
	}
	static final double GRAVITATIONAL_CONSTANT = 9.81;
```
**Motivation**  
Avoid using Magic numbers.

## 27. Encapsulate Field
There is a public field.  
_Make it private and provide accessors_
```java

	public String _name
```
to
```java

	private String _name;
	public String getName() {return _name;}
	public void setName(String arg) {_name = arg;}
```
**Motivation**  
You should never make your data public.

## 28. Encapsulate Collection
A method returns a collection.  
_Make it return a read-only view and provide add/remove methods_
```java

	class Person {
		Person (String name){
			HashSet set new HashSet()
		}
		Set getCourses(){}
		void setCourses(:Set){}
	}
```
to
```java

	class Person {
		Person (String name){
			HashSet set new HashSet()
		}
		Unmodifiable Set getCourses(){}
		void addCourses(:Course){}
		void removeCourses(:Course){}
	}
```
**Motivation**   
* Encapsulated reduce the coupling of the owning class to its clients.
* The getter should not return the collection object itself.
* The getter should return something that prevents manipulation of the collection and hides unnecessary details.
* There should not be a setter for collection, only operations to add and remove elements.

## 29. Remove Record with data class
You need to interface with a record structure in a traditional programming environment.  
_Make a dumb data object for the record._

**Motivation**  
* Copying a legacy program
* Communicating a structured record with a traditional programming API or database.
## 30. Replace Type Code with Class
A class has a numeric type code that does not affect its behavior.  
_Replace the number with a new class._
```java

	class Person{
		O:Int;
		A:Int;
		B:Int;
		AB:Int;
		bloodGroup:Int;
	}
```
to
```java

	class Person{
		bloodGroup:BloodGroup;
	}

	class BloodGroup{
		O:BloodGroup;
		A:BloodGroup;
		B:BloodGroup;
		AB:BloodGroup;
	}
```
**Motivation**  
Statically type checking.

## 31. Replace Type Code with Subclasses
You have an immutable type code that affects the behavior of a class.  
_Replace the type code with subclasses._
```java

	class Employee...
		private int _type;

		static final int ENGINEER = 0;
		static final int SALESMAN = 1;
		static final int MANAGER = 2;

		Employee (int type) {
			_type = type;
		}
	}
```
to
```java

	abstract int getType();
	static Employee create(int type) {
		switch (type) {
			case ENGINEER:
				return new Engineer();
			case SALESMAN:
				return new Salesman();
			case MANAGER:
				return new Manager();
			default:
				throw new IllegalArgumentException("Incorrect type code value");
		}
	}
```
**Motivation**  

* Execute different code depending on the value of a type.
* When each type code object has unique features.
* Structure to implement [38. Replace Conditional with Polymorphism](#38-replace-conditional-with-polymorphism)
* Use of [30. Replace Type Code with Class](#30-replace-type-code-with-class)

## 32. Replace Type Code with State/Strategy
You have a type code that affects the behavior of a class, but you cannot use subclassing.  
_Replace the type code with a state object_
```java

	class Employee {
		private int _type;

		static final int ENGINEER = 0;
		static final int SALESMAN = 1;
		static final int MANAGER = 2;

		Employee (int type) {
			_type = type;
		}
		int payAmount() {
			switch (_type) {
				case ENGINEER:
					return _monthlySalary;
				case SALESMAN:
					return _monthlySalary + _commission;
				case MANAGER:
					return _monthlySalary + _bonus;
				default:
					throw new RuntimeException("Incorrect Employee");
				}
			}
		}
	}
```
to
```java

	class Employee...
		static final int ENGINEER = 0;
		static final int SALESMAN = 1;
		static final int MANAGER = 2;

		void setType(int arg) {
			_type = EmployeeType.newType(arg);
		}
		class EmployeeType...
			static EmployeeType newType(int code) {
				switch (code) {
					case ENGINEER:
						return new Engineer();
					case SALESMAN:
						return new Salesman();
					case MANAGER:
						return new Manager();
					default:
						throw new IllegalArgumentException("Incorrect Employee Code");
				}
			}
		}
		int payAmount() {
			switch (getType()) {
				case EmployeeType.ENGINEER:
					return _monthlySalary;
				case EmployeeType.SALESMAN:
					return _monthlySalary + _commission;
				case EmployeeType.MANAGER:
					return _monthlySalary + _bonus;
				default:
					throw new RuntimeException("Incorrect Employee");
			}
		}
	}
```
**Motivation**  

* Similar to [31. Replace Type Code with Subclasses](#31-replace-type-code-with-subclasses), but can be used if the type code changes during the life of the object or if another reason prevents subclassing.
* It uses either the state or strategy pattern

## 32. Replace Subclass with Fields
You have subclasses that vary only in methods that return constant data.  
_Change the methods to superclass fields and eliminate the subclasses_
```java

	abstract class Person {
		abstract boolean isMale();
		abstract char getCode();
		...
	}
	class Male extends Person {
			boolean isMale() {
			return true;
		}
		char getCode() {
			return 'M';
		}
	}
	class Female extends Person {
		boolean isMale() {
			return false;
		}
		char getCode() {
			return 'F';
		}
	}
```
to
```java

	class Person{
		protected Person (boolean isMale, char code) {
			_isMale = isMale;
			_code = code;
		}
		boolean isMale() {
			return _isMale;
		}
		static Person createMale(){
			return new Person(true, 'M');
		}
		static Person createFemale(){
			return new Person(false, 'F');
		}
	}
```
**Motivation**  

* Subclasses that consists only of constant methods is not doing enough to be worth existing.
* Remove such subclasses completely by putting fields in the superclass.
* Remove the extra complexity of the subclasses.

# 9. SIMPLIFYING CONDITIONAL EXPRESSIONS

## 33. Decompose Conditional
You have a complicated conditional (if-then-else) statement.     
_Extract methods from the condition, then part, and else parts_
```java

	if (date.before (SUMMER_START) || date.after(SUMMER_END))
		charge = quantity * _winterRate + _winterServiceCharge;
	else charge = quantity * _summerRate;
```
to
```java

	if (notSummer(date))
		charge = winterCharge(quantity);
	else charge = summerCharge (quantity);
```
**Motivation**  

* Highlight the condition and make it clearly what you are branching on.
* Highlight the reason for the branching

## 34. Consolidate Conditional Expression
You have a sequence of conditional tests with the same result.  
_Combine them into a single conditional expression and extract it_
```java

	double disabilityAmount() {
	if (_seniority < 2) return 0;
	if (_monthsDisabled > 12) return 0;
	if (_isPartTime) return 0;
	// compute the disability amount
```
to
```java

	double disabilityAmount() {
	if (isNotEligableForDisability()) return 0;
	// compute the disability amount
```
**Motivation**  

* Makes the check clearer by showing that you are really making a single check
* Sets you up for [1. Extract Method](#1-extract-method).
* Clarify your code (replaces a statement of what you are doing with why you are doing it.)

## 35. Consolidate Duplicate Conditional Fragments
The same fragment of code is in all branches of a conditional expression.  
_Move it outside of the expression._
```java

	if (isSpecialDeal()) {
		total = price * 0.95;
		send();
	}
	else {
		total = price * 0.98;
		send();
	}
```
to
```java

	if (isSpecialDeal()) {
		total = price * 0.95;
	}
	else {
		total = price * 0.98;
	}
	send();

```
**Motivation**  
Makes clearer what varies and what stays the same.

## 36. Remove Control Flag
You have a variable that is acting as a control flag for a series of boolean expressions.  
_Use a break or return instead_
```java

	void checkSecurity(String[] people) {
		boolean found = false;
			for (int i = 0; i < people.length; i++) {
				if (! found) {
					if (people[i].equals ("Don")){
						sendAlert();
						found = true;
					}
					if (people[i].equals ("John")){
						sendAlert();
						found = true;
					}
				}
		}
	}
```
to
```java

	void checkSecurity(String[] people) {
		for (int i = 0; i < people.length; i++) {
			if (people[i].equals ("Don")){
				sendAlert();
				break; // or return
			}
			if (people[i].equals ("John")){
				sendAlert();
				break; // or return
			}
		}
	}
```
**Motivation**  

* Control flag are used to determine when to stop looking, but modern languages enforce the use of `break` and `continue`
* Make the real purpose of the conditional much more clear.

## 37. Replace Nested Conditional with Guard Clauses
A method has conditional behavior that does not make clear the normal path of execution.  
_Use guard clauses for all the special cases_
```java

	double getPayAmount() {
		double result;
		if (_isDead) result = deadAmount();
		else {
			if (_isSeparated) result = separatedAmount();
			else {
				if (_isRetired) result = retiredAmount();
				else result = normalPayAmount();
			};
		}
		return result;
	};
```
to
```java

	double getPayAmount() {
		if (_isDead) return deadAmount();
		if (_isSeparated) return separatedAmount();
		if (_isRetired) return retiredAmount();
		return normalPayAmount();
	};
```
**Motivation**  

* If the condition is an unusual condition, check the condition and return if the condition is true.
* This kind of check is often called a **guard clause** [Beck].
* If you are using an if-then-else construct you are giving equal weight to the if leg and the else leg.
* This communicates to the reader that the legs are equally likely and important.
* Instead the **guard clause** says, _"This is rare, and if it happens, do something and get out."_

## 38. Replace Conditional with Polymorphism
You have a conditional that chooses different behavior depending on the type of an object.   
_Move each leg of the conditional to an overriding method in a subclass. Make the original method abstract_
```java

	class Employee {
		private int _type;

		static final int ENGINEER = 0;
		static final int SALESMAN = 1;
		static final int MANAGER = 2;

		Employee (int type) {
			_type = type;
		}
		int payAmount() {
			switch (_type) {
				case ENGINEER:
					return _monthlySalary;
				case SALESMAN:
					return _monthlySalary + _commission;
				case MANAGER:
					return _monthlySalary + _bonus;
				default:
					throw new RuntimeException("Incorrect Employee");
				}
			}
		}
	}
```
to
```java

	class Employee...
		static final int ENGINEER = 0;
		static final int SALESMAN = 1;
		static final int MANAGER = 2;

		void setType(int arg) {
			_type = EmployeeType.newType(arg);
		}
		int payAmount() {
			return _type.payAmount(this);
		}
	}

	class Engineer...
		int payAmount(Employee emp) {
			return emp.getMonthlySalary();
		}
	}
```
**Motivation**  

* Avoid writing an explicit conditional when you have objects whose behavior varies depending on their types.
* Switch statements should be less common in object oriented programs

## 39. Introduce Null Object
You have repeated checks for a null value.  
_Replace the null value with a null object_
```java

	if (customer == null) plan = BillingPlan.basic();
	else plan = customer.getPlan();
```
to
```java

	class Customer {}

	class NullCusomer extends Customer {}
```
**Motivation**

* The object, depending on its type, does the right thing. Null objects should also apply this rule.
* Use **Null Object Pattern** is the little brother of **Special Case Pattern**.

## 40. Introduce Assertion
A section of code assumes something about the state of the program.  
_Make the assumption explicit with an assertion_
```java

	double getExpenseLimit() {
		// should have either expense limit or a primary project
		return (_expenseLimit != NULL_EXPENSE) ?
			_expenseLimit:
			_primaryProject.getMemberExpenseLimit();
	}
```
to
```java

	double getExpenseLimit() {
		Assert.isTrue (_expenseLimit != NULL_EXPENSE || _primaryProject	!= null);
		return (_expenseLimit != NULL_EXPENSE) ?
			_expenseLimit:
			_primaryProject.getMemberExpenseLimit();
	}
```
**Motivation**  

* Assertions are conditional statements that are assumed to be always true.
* Assertion failures should always result in unchecked exceptions.
* Assertions usually are removed for production code.
* As communication  aids: they help the reader understand the assumptions the code is making.
* As debugging aids: assertions can help catch bugs closer to their origin.

# 10. MAKING METHOD CALLS SIMPLER
## 41. Rename method
The name of a method does not reveal its purpose.  
_Change the name of the method_
```java
	getinvcdtlmt()
```
to
```java
	getInvoiceableCreditLimit
```
**Motivation**   
Methods names must communicate their intention.

## 42. Add Parameter
A method needs more information from its caller.  
_Add a parameter for an object that can pass on this information_
```java
	getContact()
```
to
```java
	getContact(:Date)
```
**Motivation**    
After changed a method you require more information.
## 43. Remove Parameter
A parameter is no longer used by the method body.  
_Remove it_
```java
	getContact(:Date)
```
to
```java
	getContact()
```
**Motivation**    
A parameter is no more needed.

## 44. Separate Query from Modifier
You have a method that returns a value but also changes the state of an object.  
_Create two methods, one for the query and one for the modification_
```java
	getTotalOutStandingAndSetReadyForSummaries()
```
to
```java
	getTotalOutStanding()
	SetReadyForSummaries()
```
**Motivation**    
Signaling methods with side effects and those without.
## 45. Parameterize Method
Several methods do similar things but with different values contained in the method body.  
_Create one method that uses a parameter for the different values_
```java
	fivePercentRaise()
	tenPercentRaise()
```
to
```java
	raise(percentage)
```
**Motivation**    
Removes duplicate code and increases flexibility.
## 46. Replace Parameter with Explicit Methods
You have a method that runs different code depending on the values of an enumerated parameter.  
_Create a separate method for each value of the parameter_
```java

	void setValue (String name, int value) {
		if (name.equals("height")){}
			_height = value;
			return;
		}
		if (name.equals("width")){
			_width = value;
			return;
		}
		Assert.shouldNeverReachHere();
	}
```
to
```java

	void setHeight(int arg) {
		_height = arg;
	}
	void setWidth (int arg) {
		_width = arg;
	}
```
**Motivation**

* Avoid the conditional behavior
* Gain compile time checking
* Clearer Interface

## 47. Preserve Whole Object
You are getting several values from an object and passing these values as parameters in a method call.  
_Send the whole object instead_
```java

	int low = daysTempRange().getLow();
	int high = daysTempRange().getHigh();
	withinPlan = plan.withinRange(low, high);
```
to
```java

	withinPlan = plan.withinRange(daysTempRange());
```
**Motivation**

* Make parameters list robust to changes
* Make code more readeable
* Remove possible duplicate code already done in the object passed
* Negative: It creates a dependency between the parameter object and the called.

## 48. Replace Parameter with Method
An object invokes a method, then passes the result as a parameter for a method. The receiver can also invoke this method.  
_Remove the parameter and let the receiver invoke the method_
```java

	int basePrice = _quantity * _itemPrice;
	discountLevel = getDiscountLevel();
	double finalPrice = discountedPrice (basePrice, discountLevel);
```
to
```java

	int basePrice = _quantity * _itemPrice;
	double finalPrice = discountedPrice (basePrice);
```
**Motivation**

* If a method can get a value that is passed in as parameter by another means, it should.
* If the receiving method can make the same calculation (does not reference any of the parameters of the calling method)
* If you are calling a method on a different object that has a reference to the calling object.

## 49. Introduce Parameter Object
You have a group of parameters that naturally go together.  
_Replace them with an object_
```java

	class Customer{
		amountInvoicedIn (start : Date, end : Date)
		amountReceivedIn (start : Date, end : Date)
		amountOverdueIn (start : Date, end : Date)
	}
```
to
```java

	class Customer{
		amountInvoicedIn (: DateRange)
		amountReceivedIn (: DateRange)
		amountOverdueIn (: DateRange)
	}
```
**Motivation**

* Reduces the size of the parameter list
* Make the code more consistent

## 50. Remove Setting Method
A field should be set at creation time and never altered.  
_Remove any setting method for that field_
```java

	class Employee{
		setImmutableValue()
	}
```
to
```java

	class Employee{
		¯\_(ツ)_/¯
	}
```
**Motivation**   
Make your intention  clear: If you don't want that field to change once is created, then don't provide a setting method (and make the field final).

## 51. Hide Method
A method is not used by any other class.  
_Make the method private_
```java

	class Employee{
		public method()
	}
```
to
```java

	class Employee{
		private method()
	}
```
**Motivation**   
Whenever a method is not needed outside its class it should be hidden

## 52. Replace Constructor with Factory Method
You want to do more than simple construction when you create an object.  
_Replace the constructor with a factory method_
```java

	Employee (int type) {
		_type = type;
	}
```
to
```java

	static Employee create(int type) {
		return new Employee(type);
	}
```
**Motivation**   
Create an object depending on its subclasses  (types). Constructors can only return an instance of the object that is asked for so a Factory method is needed.

## 53. Encapsulate Downcast
A method returns an object that needs to be downcasted by its callers.  
_Move the downcast to within the method_

```java

	Object lastReading() {
		return readings.lastElement();
	}
```
to
```java

	Reading lastReading() {
		return (Reading) readings.lastElement();
	}
```
**Motivation**   
Provide as result type, the most specific type of the method signature.      
If the signature is to general, check the uses the clients do of that method and if coherent, provide a more specific one.

## 54. Replace Error Code with Exception
A method returns a special code to indicate an error.   
_Throw an exception instead_
```java

	int withdraw(int amount) {
		if (amount > _balance)
			return -1;
		else {
			_balance -= amount;
			return 0;
		}
	}
```
to
```java

	void withdraw(int amount) throws BalanceException {
		if (amount > _balance)
			throw new BalanceException();
		_balance -= amount;
	}
```
**Motivation**
When a program that spots an error can't figure out what to do about it. It needs to let its caller know, and the caller may pass the error up the chain.
## 55. Replace Exception with Test
You are throwing a checked exception on a condition the caller could have checked first.   
_Change the caller to make the test first_
```java

	double getValueForPeriod (int periodNumber) {
		try {
			return _values[periodNumber];
		} catch (ArrayIndexOutOfBoundsException e) {
			return 0;
		}
	}
```
to
```java

	double getValueForPeriod (int periodNumber) {
		if (periodNumber >= _values.length)
			return 0;
		return _values[periodNumber];
}
```
**Motivation**

* Do not overuse Exceptions they should be used for exceptional behavior (behavior that is unexpected).
* Do not use them as substitute for conditional tests.
* Check expected wrong conditions before before calling the operation.

# 11. DEALING WITH GENERALIZATION
## 56. Pull up field
Two subclasses have the same field.   
_Move the field to the superclass_   
```java

	class Salesman extends Employee{
		String name;
	}
	class Engineer extends Employee{
		String name;
	}
```
to
```java

	class Employee{
		String name;
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}

```
**Motivation**

* Removes the duplicate data declaration.
* Allows  to move  behavior from the subclasses to the superclass.

## 57. Pull Up Method
You have methods with identical results on subclasses.   
_Move them to the superclass_
```java

	class Salesman extends Employee{
		String getName();
	}
	class Engineer extends Employee{
		String getName();
	}
```
to
```java

	class Employee{
		String getName();
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}

```
**Motivation**

* Eliminates duplicated behavior.

## 58. Pull Up Constructor Body
You have constructors on subclasses with mostly identical bodies.  
_Create a superclass constructor; call this from the subclass methods_  
```java

	class Manager extends Employee...
		public Manager (String name, String id, int grade) {
		_name = name;
		_id = id;
		_grade = grade;
	}

```
to
```java

	public Manager (String name, String id, int grade) {
		super (name, id);
		_grade = grade;
	}
```
**Motivation**

* Constructors are not the same as methods.
* Eliminates duplicated behavior.

## 59. Push Down Method
Behavior on a superclass is relevant only for some of its subclasses.   
_Move it to those subclasses._   
```java

	class Employee{
		int getQuota();
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}
```
to
```java

	class Salesman extends Employee{
		int getQuota();
	}
	class Engineer extends Employee{}
```
**Motivation**
When a method makes only sense in the subclass.
## 60. Push Down Field
A field is used only by some subclasses.  
_Move the field to those subclasses_
```java

	class Employee{
		int quota;
	}
	class Salesman extends Employee{}
	class Engineer extends Employee{}
```
to
```java

	class Salesman extends Employee{
		int quota;
	}
	class Engineer extends Employee{}
```
**Motivation**
When a field makes only sense in the subclass.
## 61. Extract Subclass
A class has features that are used only in some instances.  
_Create a subclass for that subset of features_  
```java

	class JobItem	{
		getTotalPrices()
		getUnitPrice()
		getEmployee()
	}
```
to
```java

	JobItem	{
		getTotalPrices()
		getUnitPrice()
	}
	class class LabotItem extends JobItem	{
		getUnitPrice()
		getEmployee()
	}
```
**Motivation**
When a class has behavior used for some instances of the class and not for others.
## 62. Extract Superclass
You have two classes with similar features.   
_Create a superclass and move the common features to the superclass_  
```java

	class Department{
		getTotalAnnualCost()
		getName()
		getHeadCount
	}
	class Employee{
		getAnnualCost()
		getName()
		getId
	}

```
to
```java

	class Party{
		getAnnualCost()
		getName()
	}
	class Department {
		getAnnualCost()
		getHeadCount
	}
	class Employee {
		getAnnualCost()
		getId
	}

```
**Motivation**	 
When two classes that do similar things in the same way or similar things in different ways.
## 63. Extract Interface
Several clients use the same subset of a class's interface, or two classes have part of their interfaces in common.  
_Extract the subset into an interface_  
```java

	class Employee {
		getRate()
		hasSpecialSkill()
		getName()
		getDepartment()
	}
```
to
```java

	interface Billable	{
		getRate()
		hasSpecialSkill()
	}
	class Employee implements Billable	{
		getRate
		hasSpecialSkill()
		getName()
		getDepartment()
	}
```	
**Motivation**

* If only a particular subset of a class's responsibilities is used by a group of clients.
* If a class needs to work with any class that can handle certain requests.
* Whenever a class has distinct roles in different situations.

## 64. Collapse Hierarchy
A superclass and subclass are not very different.   
_Merge them together_
```java

	class Employee{	}
	class Salesman extends Employee{	}
```
to
```java

	class Employee{	}
```		
**Motivation**	 
When a subclass that isn't adding any value.
## 65. Form Template Method
You have two methods in subclasses that perform similar steps in the same order, yet the steps are different.
_Get the steps into methods with the same signature, so that the original methods become the same. Then you can pull them up_
```java

	class Site{}
	class ResidentialSite extends Site{
		getBillableAmount()
	}
	class LifelineSite extends Site{
		getBillableAmount()
	}
```
to
```java

	class Site{ 	
		getBillableAmount()
		getBaseAmount()
		getTaxAmount()
	}
	class ResidentialSite extends Site{
		getBaseAmount()
		getTaxAmount()
	}
	class LifelineSite extends Site{
		getBaseAmount()
		getTaxAmount()
	}
```
**Motivation**

* Use Inheritance with polymorphism  for eliminating duplicate behavior that is slightly different.
* When there are two similar methods in a subclass, bring them together in a superclass.
* [Template Method](https://www.wikiwand.com/en/Template_method_pattern)[[Gang of Four]](https://www.wikiwand.com/en/Design_Patterns)

## 66. Replace Inheritance with Delegation
A subclass uses only part of a superclasses interface or does not want to inherit data.  
_Create a field for the superclass, adjust methods to delegate to the superclass, and remove the subclassing_
```java

	class Vector{
		isEmpty()
	}

	class Stack extends Vector {}

```
to
```java

	class Vector {
		isEmpty()
	}

	class Stack {
		Vector vector
		isEmpty(){
			return vector.isEmpty()
		}
	}

```
**Motivation**

* Using delegation makes  clear that you are making only partial use of the delegated class.
* You control which aspects of the interface to take and which to ignore.

Mechanics
## 67. Replace Delegation with Inheritance
You're using delegation and are often writing many simple delegations for the entire interface.   
_Make the delegating class a subclass of the delegate_
```java

	class Person {
		getName()
	}

	class Employee {
		Person person
		getName(){
			return person.getName()
		}
	}
```
to
```java

	class Person{
		getName()
	}
	class Employee extends Person{}

```
**Motivation**	 

* If you are using all the methods of the delegate.
* If you aren't using all the methods of the class to which you are delegating, you shouldn't use it.
* Beware of is that in which the delegate is shared by more than one object and is mutable. Data sharing is a responsibility that cannot be transferred back to inheritance.

# 12. BIG REFACTORINGS
## 68. Tease Apart Inheritance
You have an inheritance hierarchy that is doing two jobs at once.   
_Create two hierarchies and use delegation to invoke one from the other_
```java

	class Deal{}
	class ActiveDeal extends Deal{}
	class PassiveDeal extends Deal{}
	class TabularActiveDeal extends ActiveDeal{}
	class TabularPassiveDeal extends PassiveDeal{}
```
to
```java

	class Deal{
		PresentationStyle presettationStyle;
	}
	class ActiveDeal extends Deal{}
	class PassiveDeal extends Deal{}

	class PresentationStyle{}
	class TabularPresentationStyle extends PresentationStyle{}
	class SinglePresentationStyle extends PresentationStyle{}

```
**Motivation**

* Tangled inheritance leads to code duplication.
* If every class at a certain level in the hierarchy has subclasses that begin with the same adjective, you probably are doing two jobs with one hierarchy.

**Mechanics Note**   
Identify the different jobs being done by the hierarchy. Create a two-dimensional(or x-dimensional) grid and label the axes with the different jobs.

| Deal         | Active Deal | Passive Deal |
|--------------|-------------|--------------|
| Tabular Deal |             |              |

## 69. Convert Procedural Design to Objects
You have code written in a procedural style.   
_Turn the data records into objects, break up the behavior, and move the behavior to the objects_
```java

	class OrderCalculator{
		determinePrice(Order)
		determineTaxes(Order)
	}
	class Order{}
	class OrderLine{}
```
to
```java

	class Order{
		getPrice()
		getTaxes()
	}
	class OrderLine{
		getPrice()
		getTaxes()
	}
```
**Motivation**
Use OOP (at least in JAVA)

## 70. Separate Domain from Presentation
You have GUI classes that contain domain logic.   
_Separate the domain logic into separate domain classes_
```java

	class OrderWindow{}
```
to
```java

	class OrderWindow{
		Order order;
}
```
**Motivation**

* Separates two complicated parts of the program into pieces that are easier to modify.
* Allows multiple presentations of the same business logic.
* Its worth is proved

## 71. Extract Hierarchy
You have a class that is doing too much work, at least in part through many conditional statements.   
_Create a hierarchy of classes in which each subclass represents a special case_
```java

	class BillingScheme{}
```
to
```java

	class BillingScheme{}
	class BusinessBillingScheme extends BillingScheme{}
	class ResidentialBillingScheme extends BillingScheme{}
	class DisabilityBillingScheme extends BillingScheme{}
```
**Motivation**   

* A class as implementing one idea become implementing two or three or ten.
* Keep the Single Responsibility Principle.
