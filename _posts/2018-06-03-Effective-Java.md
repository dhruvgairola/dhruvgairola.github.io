---
layout: post
title: Effective Java
---

With the release of the 3rd Edition of "Effective Java" a few months back, I decided to re-read it and compile a few notes for future reference. These notes are not very organized- they only represent points that are specifically useful to my development as a Java programmer.

### Creating and Destroying Objects
* Consider static factory methods instead of constructors.
* Consider a builder when faced with many constructor parameters.
* Creating a Pattern instance is expensive because it requires compiling the regular expression into a finite state machine.
* As is often the case with lazy initialization, it would complicate the implementation with no measurable performance improvement
* Generally speaking, however, maintaining your own object pools clutters your code, increases memory footprint, and harms performance
* Whenever a class manages its own memory, the programmer should be alert for memory leaks
* A common source of memory leaks is listeners and other callbacks. If you implement an API where clients register callbacks but don’t deregister them explicitly, they will accumulate unless you take some action. One way to ensure that callbacks are garbage collected promptly is to store only weak references to them, for instance, by storing them only as keys in a WeakHashMap.
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
* Always use try-with-resources in preference to try-finally when working with resources that must be closed. The resulting code is shorter and clearer, and the exceptions that it generates are more useful. The try-with- resources statement makes it easy to write correct code using resources that must be closed, which was practically impossible using try-finally.