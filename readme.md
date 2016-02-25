This is my summary of the "Refactoring: Improving the Design of Existing Code" by Martin Fowler. I use it while learning and as quick reference. It is not intended to be an standalone substitution of the book so if you really want to learn the concepts here presented, buy and read the book and use this repository as a reference and guide.

If you are the publisher and think this repository should not be public, just write me an email at hugomatilla [at] gmail [dot]com and I will make it private.
# 3 Bad Smells in code
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

