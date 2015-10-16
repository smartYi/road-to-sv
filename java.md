+ Overlading vs. Overriding
	+ Overloading is a term used to describe when two methods have the same name but differ in the type or number of arguments
	+ Overriding occurs when a method shares the same name and function signature as another method in its super class
+ ArrayList
	+ ArrayList<String> myArr = new ArrayList<String>();
	+ myArr.add("one");
	+ System.out.println(myArr.get(0));
+ Vector
	+ Vector<String> myVect = new Vector<String>();
	+ myVect.add("one");
	+ System.out.println(myVect.get(0));
+ LinkedList
	+ LinkedList<String> myLinkedList = new LinkedList<String>();
	+ myLinkedList.addFirst("two");
	+ myLinkedList.addFirst("one");
	+ Iterator<String> iter = myLinkedList.iterator();
	+ while (iter.hashNext()) { System.out.println(iter.next()); }
+ HashMap
	+ HashMap<String, String> map = new HashMap<String, String>();
	+ map.put("one", "uno");
	+ map.put("two", "dos");
	+ System.out.println(map.get("one")); 
	
# Questions
	
> Private Constructor: In terms of inheritance, what is the effect of keeping

This has direct implications for inheritance, since a subclass calls its parent's constructor. The class A an be inherited, but only by its own or its parent's inner classes.
	
> Return from Finally: In java, does the `finally` block get executed if we insert a return statements inside the try block of a `try-catch-finally`?

Yes, it will get executed. The `finally` block gets executed when the `try` block exits.

There are some cases in which the `finally` block will not get executed, such as the following:

+ If the virtual machine exits during try/catch block execution
+ If the thread which is executing during the try/catch block gets killed

> Final, etc.: What is the difference between final, finally, and finalize?

+ final - control whether a variable, method, or class is "change-able"
+ finally - used in a try/catch block to ensure that a segment of code is always executed
+ finalize() - called by garbage collector once it determines that no more references exist.

> Generics vs. Templates: Explain the difference between templates in C++ and generics in Java?

The implementation of Java generics is rooted in an idea of "type erasure". This technique eliminates the parameterized types when source code is translated to the Java Virtual Machine (JVM) byte code.

The use of Java generics didn't really change much about our capabilities; it just made things a bit prettier. For this reason, Java generics are sometimes called "syntactic sugar"

In C++, templates are essentially a glorified macro set, with the compiler creating a new copy of the template code for each type.

> TreeMap, HashMap, LinkedHashMap: Explain the differences between these three. Provide an example of when each one would be best

+ `HashMap` offers O(1) lookup and insertion. It is implemented by an array of linked lists.
+ `TreeMap` offers O(log N) lookup and insertion. Keys are ordered. It is implemented by a Red-Black Tree.
+ `LinkedHashMap` offers O(1) lookup and insertion. Keys are ordered by their insertion order. It is implemented by doubly-linked buckets.

> Object Reflection: Explain what object reflection is in Java and why it is useful.

Provides a way to get relfective information about Java classes and objects, and perform operations such as:

+ Getting information about the methods and fields present inside the class at runtime.
+ Creating a new instance of a class
+ Getting and setting the object fields directly by getting field reference, regardless of what the access modifier is.

Three main reasons why Object Reflection is Useful:

1. It can help you observe or manipulate the runtime behavior of applications
2. It can help you debug or test programs, as you have direct access to methods, constructors, and fields.
3. You can call methods by name when you don't know the method in advance.