---
layout: post
title: Effective Java
---

With the release of the 3rd Edition of "Effective Java" a few months back, I decided to re-read it and compile a few notes for future reference. These notes only represent points that are specifically useful to my development as a Java programmer.

### Chapter 2 : Creating and Destroying Objects
#### Item 1 : Consider static factory methods instead of constructors
* Consider static factory methods instead of constructors.

#### Item 2 : Consider a builder when faced with many constructor parameters
* Consider a builder when faced with many constructor parameters.

#### Item 3 : Enforce the singleton property with a private constructor or an enum type

#### Item 4 : Enforce noninstantiability with a private constructor

#### Item 5 : Prefer dependency injection to hardwiring resources

#### Item 6 : Avoid creating unnecessary objects
* Creating a Pattern instance is expensive because it requires compiling the regular expression into a finite state machine.
* As is often the case with lazy initialization, it would complicate the implementation with no measurable performance improvement (Item 67).

#### Item 7 : Eliminate obsolete object references
* Generally speaking, maintaining your own object pools clutters your code, increases memory footprint, and harms performance.
* Whenever a class manages its own memory, the programmer should be alert for memory leaks
* A common source of memory leaks is listeners and other callbacks. If you implement an API where clients register callbacks but don’t deregister them explicitly, they will accumulate unless you take some action. One way to ensure that callbacks are garbage collected promptly is to store only weak references to them, for instance, by storing them only as keys in a WeakHashMap.

#### Item 8 : Avoid finalizers and cleaners
* The Java 9 replacement for finalizers is cleaners. Cleaners are less dangerous than finalizers, but still unpredictable, slow, and generally unnecessary.
* The promptness with which finalizers and cleaners are executed is primarily a function of the garbage collection algorithm, which varies widely across implementations.
* Providing a finalizer for a class can arbitrarily delay reclamation of its instances.
* Not only does the specification provide no guarantee that finalizers or cleaners will run promptly; it provides no guarantee that they’ll run at all.
* There is a severe performance penalty for using finalizers and cleaners.
* Finalizers inhibit efficient garbage collection.
* Finalizers have a serious security problem: they open your class up to finalizer attacks.
* So what should you do instead of writing a finalizer or cleaner for a class whose objects encapsulate resources that require termination, such as files or threads? Just have your class implement AutoCloseable, and require its clients to invoke the close method on each instance when it is no longer needed, typically using try-with-resources to ensure termination even in the face of exceptions (Item 9).
* So what, if anything, are cleaners and finalizers good for? They have perhaps two legitimate uses. One is to act as a safety net in case the owner of a resource neglects to call its close method. While there’s no guarantee that the cleaner or finalizer will run promptly (or at all), it is better to free the resource late than never if the client fails to do so. If you’re considering writing such a safety-net finalizer, think long and hard about whether the protection is worth the cost. Some Java library classes, such as FileInputStream, FileOutputStream, ThreadPoolExecutor, and java.sql.Connection, have finalizers that serve as safety nets. A second legitimate use of cleaners concerns objects with native peers. A native peer is a native (non-Java) object to which a normal object delegates via native methods. Because a native peer is not a normal object, the garbage collector doesn’t know about it and can’t reclaim it when its Java peer is reclaimed. A cleaner or finalizer may be an appropriate vehicle for this task, assuming the performance is acceptable and the native peer holds no critical resources. If the performance is unacceptable or the native peer holds resources that must be reclaimed promptly, the class should have a close method, as described earlier.
* In summary, don’t use cleaners, or in releases prior to Java 9, finalizers, except as a safety net or to terminate noncritical native resources. Even then, beware the indeterminacy and performance consequences.

#### Item 9 : Prefer try-with-resources to try-finally
* Always use try-with-resources in preference to try-finally when working with resources that must be closed. The resulting code is shorter and clearer, and the exceptions that it generates are more useful. The try-with-resources statement makes it easy to write correct code using resources that must be closed, which was practically impossible using try-finally.

### Chapter 3 : Methods Common to All Objects
#### Item 10 : Obey the general contract when overriding equals

#### Item 11 : Always override hashCode when you override equals
* If two objects are unequal according to the equals(Object) method, it is not required that calling hashCode on each of the objects must produce distinct results. However, the programmer should be aware that producing distinct results for unequal objects may improve the performance of hash tables (collision chances are reduced).
* The key provision that is violated when you fail to override hashCode is the second one: equal objects must have equal hash codes. Two distinct instances may be logically equal according to a class’s equals method, but to Object’s hashCode method, they’re just two objects with nothing much in common. Therefore, Object’s hashCode method returns two seemingly random numbers instead of two equal numbers as required by the contract.
* Don’t provide a detailed specification for the value returned by hashCode, so clients can’t reasonably depend on it; this gives you the flexibility to change it. Many classes in the Java libraries, such as String and Integer, specify the exact value returned by their hashCode method as a function of the instance value. This is not a good idea but a mistake that we’re forced to live with: It impedes the ability to improve the hash function in future releases. If you leave the details unspecified and a flaw is found in the hash function or a better hash function is discovered, you can change it in a subsequent release.

#### Item 12 : Always override toString
* Override Object’s toString implementation in every instantiable class you write, unless a superclass has already done so. It makes classes much more pleasant to use and aids in debugging. The toString method should return a concise, useful description of the object, in an aesthetically pleasing format. (Dhruv : This holds true only for models or entity classes I feel, not for everything.)

#### Item 13 : Override clone judiciously
* The Cloneable interface was intended as a mixin interface (Item 20) for classes to advertise that they permit cloning. Unfortunately, it fails to serve this purpose. Its primary flaw is that it lacks a clone method, and Object’s clone method is protected. You cannot, without resorting to reflection (Item 65), invoke clone on an object merely because it implements Cloneable. Even a reflective invocation may fail, because there is no guarantee that the object has an accessible clone method.
* So what does Cloneable do, given that it contains no methods? It determines the behavior of Object’s protected clone implementation: if a class implements Cloneable, Object’s clone method returns a field-by-field copy of the object; otherwise it throws CloneNotSupportedException. This is a highly atypical use of interfaces and not one to be emulated. Normally, implementing an interface says something about what a class can do for its clients. In this case, it modifies the behavior of a protected method on a superclass.
* Though the specification doesn’t say it, in practice, a class implementing Cloneable is expected to provide a properly functioning public clone method. In order to achieve this, the class and all of its superclasses must obey a complex, unenforceable, thinly documented protocol. The resulting mechanism is fragile, dangerous, and extralinguistic: it creates objects without calling a constructor.
* If a final class has a clone method that does not invoke super.clone, there is no reason for the class to implement Cloneable, as it doesn’t rely on the behavior of Object’s clone implementation.
* Suppose you want to implement Cloneable in a class whose superclass provides a well-behaved clone method. First call super.clone. The object you get back will be a fully functional replica of the original. Any fields declared in your class will have values identical to those of the original. If every field contains a primitive value or a reference to an immutable object, the returned object may be exactly what you need, in which case no further processing is necessary. Note that immutable classes should never provide a clone method because it would merely encourage wasteful copying.
* If an object contains fields that refer to mutable objects, the simple clone implementation shown earlier can be disastrous.
* In effect, the clone method functions as a constructor; you must ensure that it does no harm to the original object and that it properly establishes invariants on the clone. In order for the clone method on Stack to work properly, it must copy the internals of the stack.
* Calling clone on an array returns an array whose runtime and compile-time types are identical to those of the array being cloned. This is the preferred idiom to duplicate an array.
* The Cloneable architecture is incompatible with normal use of final fields referring to mutable objects.
* A final approach to cloning complex mutable objects is to call super.clone, set all of the fields in the resulting object to their initial state, and then call higherlevel methods to regenerate the state of the original object. While this approach is clean, it is antithetical to the whole Cloneable architecture because it blindly overwrites the field-by-field object copy that forms the basis of the architecture.
* Like a constructor, a clone method must never invoke an overridable method on the clone under construction (Item 19). If clone invokes a method that is overridden in a subclass, this method will execute before the subclass has had a chance to fix its state in the clone, quite possibly leading to corruption in the clone and the original.
* Object’s clone method is declared to throw CloneNotSupportedException, but overriding methods need not. Public clone methods should omit the throws clause, as methods that don’t throw checked exceptions are easier to use (Item 71).
* To recap, all classes that implement Cloneable should override clone with a public method whose return type is the class itself. This method should first call super.clone, then fix any fields that need fixing. Typically, this means copying any mutable objects that comprise the internal "deep structure" of the object and replacing the clone’s references to these objects with references to their copies. While these internal copies can usually be made by calling clone recursively, this is not always the best approach. If the class contains only primitive fields or references to immutable objects, then it is likely the case that no fields need to be fixed. There are exceptions to this rule. For example, a field representing a serial number or other unique ID will need to be fixed even if it is primitive or immutable.
* A better approach to object copying is to provide a copy constructor or copy factory. A copy constructor is simply a constructor that takes a single argument whose type is the class containing the constructor.
* Given all the problems associated with Cloneable, **new interfaces should not extend it, and new extendable classes should not implement it**. While it’s less harmful for final classes to implement Cloneable, this should be viewed as a performance optimization, reserved for the rare cases where it is justified (Item 67). As a rule, copy functionality is best provided by constructors or factories. A notable exception to this rule is arrays, which are best copied with the clone method. (Dhruv : I think Arrays.copyOf is better. clone is straight up garbage)

#### Item 14 : Consider implementing Comparable
* For example, consider the BigDecimal class, whose compareTo method is inconsistent with equals. If you create an empty HashSet instance and then add new BigDecimal("1.0") and new BigDecimal("1.00"), the set will contain two elements because the two BigDecimal instances added to the set are unequal when compared using the equals method. If, however, you perform the same procedure using a TreeSet instead of a HashSet, the set will contain only one element because the two BigDecimal instances are equal when compared using the compareTo method. (Dhruv : TreeSet is a sorted set. It is generally faster to add elements to the HashSet and then convert the collection to a TreeSet for a duplicate-free sorted traversal. PriorityQueue and TreeSet are similar because both contain sorted elements, but obviously PriorityQueue conforms to the Queue interface and can contain duplicate elements while TreeSet conforms to the Set interface and does not have duplicate elements.)
* In Java 7, static compare methods were added to all of Java’s boxed primitive classes. **Use of the relational operators < and > in compareTo methods is verbose and error-prone and no longer recommended.** Instead, use the static compare methods in the boxed primitive classes or the comparator construction methods in the Comparator interface.
* In Java 8, the Comparator interface was outfitted with a set of comparator construction methods, which enable fluent construction of comparators. e.g., Comparator.comparingInt
* The Comparator class has a full complement of construction methods.
* Occasionally you may see compareTo or compare methods that rely on the fact that the difference between two values is negative if the first value is less than the second, zero if the two values are equal, and positive if the first value is greater. Here is an example:

```
// BROKEN difference-based comparator - violates transitivity!
static Comparator<Object> hashCodeOrder = new Comparator<>() {
public int compare(Object o1, Object o2) {
    return o1.hashCode() - o2.hashCode();
}
};
```

Do not use this technique. It is fraught with danger from integer overflow and IEEE 754 floating point arithmetic artifacts [JLS 15.20.1, 15.21.1]. Furthermore, the resulting methods are unlikely to be significantly faster than those written using the techniques described in this item. Use either a static compare method:

```
// Comparator based on static compare method
static Comparator<Object> hashCodeOrder = new Comparator<>() {
public int compare(Object o1, Object o2) {
    return Integer.compare(o1.hashCode(), o2.hashCode());
}
};
```

or a comparator construction method:

```
// Comparator based on Comparator construction method
static Comparator<Object> hashCodeOrder = Comparator.comparingInt(o -> o.hashCode());
```

### Chapter 4 : Classes and Interfaces
#### Item 15: Minimize the accessibility of classes and members