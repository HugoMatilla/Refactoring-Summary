This is my summary of the "Refactoring: Improving the Design of Existing Code" by Martin Fowler. I use it while learning and as quick reference. It is not intended to be an standalone substitution of the book so if you really want to learn the concepts here presented, buy and read the book and use this repository as a reference and guide.

If you are the publisher and think this repository should not be public, just write me an email at hugomatilla [at] gmail [dot] com and I will make it private.
#3 Bad Smells in code
### Duplicated code
Same code structure in more than one place.
### Long Method
The longer a procedure is the more difficult is to understand.
### Large Classes
When a class is trying to do too much, duplicated code cannot be far behind.
### Long Parameter List
The are hard to understand, inconsistent and difficult to use.
### Divergent Change
When one class is commonly changed in different ways for different reasons.
### Shotgun Surgery
When every time you make a kind of change, you have to make a lot of little changes to a lot of different classes.
### Feature Envy
A method that seems more interested in a class other than the one it actually is in.
### Data Clumps
Bunches of data(fields, parameters...) that hang around together.
### Primitive Obsession
Using primitives types instead of small objects.
### Switch Statements
The same switch statement scattered about a program in different places. Use polymorphism.
### Parallel Inheritance Hierarchies
Every time you make a subclass of one class, you also have to make a subclass of another.
### Lazy Class
A class that isn't doing enough to pay for itself should be eliminated.
### Speculative Generality
All sorts of hooks and special cases to handle things that aren't required.
### Temporary Field
An instance variable that is set only in certain circumstances.
### Message Chain
When a client asks one object for another object, which the client then asks for yet another object...
### Middle Man
When an object delegates much of its functionality.
### Inappropriate Intimacy
When classes access to much to another classes.
### Alternative Classes with Different Interfaces 
Classes with methods that look to similar.
### Incomplete Library Class
When we need extra features in libraries.
### Data Class
Don't allow manipulation in Data Classes. Use encapsulation and immutability.
### Refused Bequest
Subclasses that don't make uses of parents methods.
### Comments
Not all comments but the ones that are there because the code is bad.

#6 Composing methods
##1 Extract Method

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

**Mechanics**

* Create a new method, and name it after the intention of the method
* Copy the extracted code into the new method
* Scan for references to any variables local in scope
* If temporary variables are only in the extracted code set them as temporary variables.
* If temporary variables are modified, return its modified value. If this is not possible try
	* [6 Split Temporary Variable]()
	* [4 Replace Temp with Query]()
* If variables are read, pass them in the constructor
* Replace
* Compile and test.

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

##2 Inline Method
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


**Mechanics**

* Check that the method is not polymorphic.
* Find all calls to the method.
* Replace each call with the method body.
* Compile and test.
* Remove the method definition

##3 Inline Temp
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

* Use it with [4 Replace Temp with Query]()

**Mechanics**

* Declare the temp as final if it isn't already, and compile.
* Find all references to the temp and replace them with the right-hand side of the assignment.
* Compile and test after each change.
* Remove the declaration

##4 Replace Temp with Query
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
* Is a vital step before [1 Extract Method]()

**Mechanics**

* Look for a temporary variable that is assigned to once. For more than [6 Split Temporary Variable]()
* Declare the temp as final.
* Compile.
* Extract the right-hand side of the assignment into a method.
	* Initially mark the method as private
	* Ensure there are no side effects (does not modify any object) or use [Separate Query from Modifier]()
* Compile and test
* [3 Inline Temp]() on the temp.

##5 Introduce Explaining Variable
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

**Mechanics**

* Declare a final temporary variable, and set it to the result of part of the complex expression.
* Replace the result part of the expression with the value of the temp.
* Compile and test.
* Repeat for other parts of the expression.

If using [1 Extract Method]() cost same effort use it, it will make available this methods out from the method scope. 
##6 Split Temporary Variable
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

**Mechanics**

* Change the name of a temp at its declaration and its first assignment. (No if  is of the  `i = i + x`)
* Declare the new temp as final.
* Change all references of the temp up to its second assignment.
* Declare the temp at its second assignment.
* Compile and test.
* Repeat in stages, each

##7 Remove Assignments to Parameters
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

**Mechanics**

* Create a temporary variable for the parameter.
* Replace all references to the parameter, made after the assignment, to the temporary variable.
* Change the assignment to assign to the temporary variable.
* Compile and test.

##8 Replace Method with Method Object
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

**Mechanics**

* Create a new class, name it after the method.
*  Give the new class a final field for the object that hosted the original method (the source object) and a field for each temporary variable and each parameter in the method.
* Give the new class a constructor that takes the source object and each parameter.
* Give the new class a method named "compute."
* Copy the body of the original method into compute. Use the source object field for any
invocations of methods on the original object.
* Compile.
* Replace the old method with one that creates the new object and calls compute.

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

##9 Substitute Algorithm
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

**Mechanics**

* Prepare your alternative algorithm.
* Run the new algorithm against your tests until it passes.

#7 Moving features between elements

##10 Move method
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

**Mechanics**

* Examine all features used by the source method that are defined ont he source class. Consider whether they also should be moved
* Check the sub and super-classes for other declarations
*  Declare the method in the target class
* Copy the code to the target. Adapt it to work.
* Compile
* Determine how to reference the target from the source
* Turn the source method to a delegating one
* Compile and test
* Decide to remove or keep as delegate the source method
* If removed, replace all references to the target method
* Compile and test

##11 Move field 
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

If a field is used mostly by outer classes and before [12 Extract Class]() 

**Mechanics**

* If the field is public, use [X Encapsulate Field]()
* Compile and test
* Create a field in the target (with accessors)
* Compile and test
* Determine how to reference it from the source
* Remove it on the source
* Replace all references on the source with the ones on the target
* Compile and test

##12 Extract Class
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

**Mechanics**

* Decide how to split the responsibilities of the class.
* Create a new class to express the split-off responsibilities.
* Make a link from the old to the new class.
* Use [11 Move Field]() on each field you wish to move.
* Compile and test.
* Use [10 Move Method]() to move methods over from old to new. Start with lower-level methods.
* Compile and test.
* Review and reduce the interfaces of each class.
* Decide whether to expose the new class. If you do expose the class, decide whether to expose it as a reference object or as an immutable value object.

##13 Inline Class
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

**Mechanics**

* Declare the public protocol of the source class onto the absorbing class. Delegate all these methods to the source class.
	* _If a separate interface makes sense for the source class methods, use Extract Interface before inlining._
* Change all references from the source class to the absorbing class.
* Compile and test.
* Use [10 Move Method]() and [11 Move Field]() to move features from the source class to the absorbing class.
* Hold a short, simple funeral service.

##14 Hide Delegate
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

**Mechanics**

* For each method on the delegate, create a simple delegating method on the server.
* Adjust the client to call the server.
	* If the client is not in the same package as the server, consider changing the delegate method's access to package visibility.
* Compile and test
* If no client needs to access the delegate anymore, remove the server's accessor for the delegate.
* Compile and test.

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

##15 Remove Middle Man
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

**Mechanics**

* Create an accessor for the delegate.
* For each client use of a delegate method, remove the method from the server and replace the call in the client to call method on the delegate.
* Compile and test

##16 Introduce Foreign Method
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

**Mechanics**

* Create a method in the client class that does what you need.
	* The method should not access any of the features of the client class. If it needs a value, send it in as a parameter.
* Make an instance of the server class the first parameter.
* Comment the method as "foreign method; should be in server."

##17 Introduce Local Extension
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

When plenty of [foreign methods]() need to be added to a class.

**Mechanics**

* Create an extension class either as a subclass or a wrapper of the original.
* Add converting constructors to the extension.
	* A constructor takes the original as an argument. 
	* The subclass version calls an appropriate superclass constructor 
	* The wrapper version sets the delegate field to the argument.
* Add new features to the extension.
* Replace the original with the extension where needed.
* Move any foreign methods defined for this class onto the extension.

# Organizing Data

##18 Self Encapsulate Field 
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

##19 Replace Data Value with Object 
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

##20 Change Value to Reference
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

##21 Change Reference to Value
You have a reference object that is small, immutable, and awkward to manage.  
_Turn it into a value object_ 
```java

	new Currency("USD").equals(new Currency("USD")) // returns false
```
to
```java
	
	new Currency("USD").equals(new Currency("USD")) // now returns true
```
**Motivation**  
Working with the reference object becomes awkward. The reference object is immutable and small. Used in distributed or concurrent systems.

##22 Replace Array with Object
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

##23 Duplicate Observed Data
You have domain data available only in a GUI control, and domain methods need access.  
_Copy the data to a domain object. Set up an observer to synchronize the two pieces of data_  
**Motivation**  
 To separate code that handles the user interface from code that handles the business logic.
##24 Change Unidirectional Association to Bidirectional
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

##25 Change Bidirectional Association to Unidirectional
You have a two-way association but one class no longer needs features from the other.  
_Drop the unneeded end of the association_  
**Motivation**  
If Bidirectional association is not needed, reduce complexity, handle zombie objects, eliminate interdependency 
##26 Replace Magic Number with Symbolic Constant
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

##27 Encapsulate Field
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

##28 Encapsulate Collection
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

##29 Remove Record with data class
You need to interface with a record structure in a traditional programming environment.  
_Make a dumb data object for the record._

**Motivation**  
* Copying a legacy program
* Communicating a structured record with a traditional programming API or database.
##30 Replace Type Code with Class
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

##31 Replace Type Code with Subclasses
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
* Structure to implement [XX Replace Conditional with Polymorphism]()
* Use of [30 Replace Type Code with Class]() when there is a conditional statement

##32 Replace Type Code with State/Strategy
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

* Similar to [X Replace Type Code with Subclasses](), but can be used if the type code changes during the life of the object or if another reason prevents subclassing. 
* It uses either the state or strategy pattern

##32 Replace Subclass with Fields
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

#9 Simplifying Conditional Expressions

##33 Decompose Conditional
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

##34 Consolidate Conditional Expression
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
* Sets you up for [X Extract Method]().
* Clarify your code (replaces a statement of what you are doing with why you are doing it.)

##35 Consolidate Duplicate Conditional Fragments
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

##36 Remove Control Flag
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

##37 Replace Nested Conditional with Guard Clauses
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

##38 Replace Conditional with Polymorphism
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

##39 Introduce Null Object
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

##40 Introduce Assertion
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

##X 
```java
```
to
```java
```
**Motivation** 