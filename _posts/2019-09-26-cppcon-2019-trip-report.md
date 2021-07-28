# Poster Session at Reception
Sunday, September 15 • 20:00 - 22:00

There were two posters that I really liked. The first was one from two software engineers at EPFL’s [blue brain](https://www.epfl.ch/research/domains/bluebrain/) project. It’s a 100+ person research project that simulates parts of the brain based on stimulating a real brain.

The second one that I thought was interesting was from snapchat. They covered a framework for building different filters for mobile platforms. They used Conan. Sometimes they use openBLAS or Eigen depending on what runs faster on what platform. There’s a filter [creator](https://www.snapchat.com/create).

# Talks Seen at CPPCon 2019

## C++20: C++ at 40
Monday, September 16 • 08:45 - 10:30  
Bjarne Stroustrup  
[CppCon 2019: Bjarne Stroustrup “C++20: C++ at 40”](https://www.youtube.com/watch?v=u_ij0YNkFUs)

Bjarne Stroustrup, the inventor of C++, recounts some history and highlights new features. Some of the new data structures are span, coroutines, modules.

He mentions jthread. That data structure is like `std::thread`, but cannot be detached. It joins in the destructor, which follows RAII. It will come in C++20 and it is still coming into [gsl](https://github.com/isocpp/CppCoreGuidelines/pull/1520).

## The Best Parts of C++
Monday, September 16 • 11:00 - 12:00  
Jason Turner

This was an interactive talk where the speaker brought up some code and transformed it into modern C++. It went from const and went into C++20’s ranges.

## Writing Safety Critical Automotive C++ Software for High Performance AI Hardware
Monday, September 16 • 14:00 - 15:00  
Michael Wong And Gordon Brown

MISRA C++ 2008 is based on the C++98. AUTOSAR targets C++14. AUTOSAR will be folded into MISRA and there will be a new MISRA in the coming years based on C++17. SG12 was formed to study undefined behavior. WG23 was formed to identify specific vulnerabilities.

## When C++ Style Guides Contradict
Monday, September 16 • 15:15 - 16:15  
Nicolai Josuttis

For initialization, [abseil](https://abseil.io/tips/88) recommends using assignment because it is simpler. Yet, MISRA C++ 2008 recommends braced initialization, because it doesn’t allow narrowing conversions. The speaker points out these contradictions, but it will take more work to resolve them all.

## Quickly Testing Legacy C++ Code with Approval Tests
Monday, September 16 • 16:45 - 17:45  
Clare Macrae

The speaker developed a library that integrates a test framework with popular diffing tools. When a difference exists, a GUI is brought up to compare the result with the expected output. The diffing tools are capable of comparing text or images.

The idea is that some legacy code can be wrapped in a process. Then, that process can output a file. The test then operates on that file.

## Committee Fireside Chat
Monday, September 16 • 20:30 - 22:00  

Members of the standard body for C++ discuss topics from the audience. Vittorio Romeo mentioned C++ epochs, which is very early and very exciting. It could mean that C++ uses const by default.

## Error Handling is Cancelling Operations
Tuesday, September 17 • 09:00 - 10:00  
Andrzej Krzemieński

The speaker goes through various error handling techniques. It included exceptions vs error codes. There was an interesting discussion at the end about handling out of memory exceptions.

## "Allegro" Means Both Fast and Happy. Coincidence?
Tuesday, September 17 • 10:30 - 12:00  
Andrei Alexandrescu  
[CppCon 2019: Andrei Alexandrescu “Speed Is Found In The Minds of People"](https://www.youtube.com/watch?v=FJJTYQYB1JQ)

The speaker goes beyond what is typically taught in Computer Science courses. He looks at different solutions for sorting at different input sizes. Sometimes, more instructions take less time to compute, because of branch prediction. Also, he talks about moving conditions to arithmetic. The STL only guarantees the interface, so every instruction is scrutinized to shave off extra computation.

There’s similar results on this [stackoverflow page](https://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-processing-an-unsorted-array) where a user compares running an if statement on a sorted vs unsorted array.

## 10 Techniques to Understand Existing Code
Tuesday, September 17 • 14:00 - 15:00  
Jonathan Boccara

I liked the topic, since code is more often read than written. He ran some statistics on variables usages and locality to better visualize topics and data flow. He also recommended reading "How to Read a Book: The Classic Guide to Intelligent Reading" by Mortimer J. Adler.

## C++ Code Smells
Tuesday, September 17 • 16:45 - 17:45  
Jason Turner

This was an interactive talk with a lot of developers fixing code live. The speaker said that seeing an explicit `std::move` is a code smell. I tend to agree, since I’ve seen it used wrong more than I’ve seen it used right.

## Applied WebAssembly: Compiling and Running C++ in Your Web Browser
Wednesday, September 18 • 10:30 - 12:00  
Ben Smith  
[CppCon 2019: Ben Smith “Applied WebAssembly: Compiling and Running C++ in Your Web Browser”](https://www.youtube.com/watch?v=5N4b-rU-OAA&t=493s)

The speaker goes through turning C++ into webassembly. There were some cool demos at the end. One of them did ray tracing. Threads are not currently supported in webassembly.

Compiler explorer runs programs on its server. Maybe in the future, compiler explorer programs will run in the client's browser using webassembly.

## Destructor Case Studies: Best Practices for Safe and Efficient Teardown
Thursday, September 19 • 09:00 - 10:00  
Pete Isensee

This talk covered a fundamental half of RAII. He went over the rule of 5, popularized by Scott Meyers. The general guideline to avoid defining destructors seems good. Also, he had a good chart on the order of destruction.

## Better Code: Relationships
Thursday, September 19 • 10:30 - 12:00  
Sean Parent  
[CppCon 2019: Sean Parent “Better Code: Relationships”](https://www.youtube.com/watch?v=ejF6qqohp3M)

The meaning of data and the relationships between them are important in programming. Sean Parent visualizes operations as relationships to understand code design deeper.

He classified the `std::move operator` as unsafe, because the object that is moved from becomes valid but unspecified. He argues that those two terms contradict each other.

He implements the Russian coat check algorithm. The algorithm to remove a coat, returns the coat from a data structure and marks the unfilled spaces. If more than half of the data structure’s entries are marked empty, then it compacts the data structure. The vector performs faster than unordered_map, because of the fewer allocations.

## The C++ ABI for Dummies
Thursday, September 19 • 14:00 - 15:00  
Louis Dionne

The topic choice is good. He explains that adding declarations is generally OK for ABI compatibility, but deleting or modifying declarations is generally not OK. It’s possible to get undefined behavior by breaking the one definition rule with incompatible ABIs.

One tool that library writers can use is inline namespaces. Inline namespaces allow the caller to avoid needing to write the version namespace explicitly. Then, the linker will use the full namespace. If there is a namespace discrepancy with the header and the compiled code, the linker will fail.

## Safe Software for Autonomous Mobility With Modern C++
Thursday, September 19 • 15:15 - 15:45  
Andreas Pasternak

The speaker goes through the steps for certifying parts of the ROS2 middleware. Some new data structure for using dynamic memory from a pool had to be made. Also, a small string data structure were made to put the data on the stack. They achieved 100% unit test coverage for ASIL certification.

## Behind the Scenes of a C++ Build System
Thursday, September 19 • 15:50 - 16:20  
Jussi Pakkanen

Meson build contends to be a simple, efficient build system. It’s written in python and the build configuration file is decidedly not Turing complete. Some developers wanted him to implement it in C or perl, but he prefers python. One of the challenges with building the system is integrating with all the other tools. For example, some operating systems may not have a good interface for starting processes. Therefore, quoting is done around the arguments. Having the right level of quotes requires tedious trial and error attempts for different systems. Similarly, C++20 modules is [difficult to implement](https://nibblestew.blogspot.com/2019/08/building-c-modules-take-n1.html) in any build system, because compiler vendors do things differently.

## Path Tracing Three Ways: A Study of C++ Style
Thursday, September 19 • 16:45 - 17:45  
Matt Godbolt

The speaker implements ray tracing three different ways
* Object oriented
* Functional
* Data oriented

This presentation was an exercise. It illustrated that C++ is a multiparadigm language. The object oriented and functional style were testable. The functional style used the ranges '\|’ operator. The data oriented style was faster, but was harder to test. There was a strange performance issue where he had to compute two calculations and consolidate an if statement for performance. It was related to branch prediction.

## Lightning Talks
Thursday, September 19 • 20:30 - 22:00

I took 5 minutes to present "Detecting Programs That Rely on Undefined Behavior", which included a few results from an [undefined behavior study](https://github.com/geoffviola/undefined_behavior_study).

## Naming is Hard: Let's Do Better
Friday, September 20 • 09:00 - 10:00  
Kate Gregory

Arguably the hardest thing in programming is naming things. One of the tips is to match certain words together. For example, begin and end are a good pair. Get implies little computation and a const method.

I’m still thinking about a few recommendations. She pushes for not naming a tuple variable as element1_element2_element3. Without named tuples like python, it can be hard to know where to index the variable. Also, she recommends against using plurals for data structures. Her argument for both of these is to search easier and communicate abstractions that won’t change.

##  De-fragmenting C++: Making Exceptions and RTTI More Affordable and Usable (“Simplifying C++” #6 of N)
Friday, September 20 • 16:15 - 18:00  
Herb Sutter  
[CppCon 2019: Herb Sutter “De-fragmenting C++: Making Exceptions and RTTI More Affordable and Usable”](https://www.youtube.com/watch?v=ARYP83yNAWk)

Herb Sutter goes through static exceptions and static reflection, which should come in C++23. Both of these should be lightweight alternatives to what are commonly excluded. Static exceptions will have consistent checks to avoid dynamic allocation. Static reflection will allow for fast runtime checks on downcasting.

# Tool Time
Tuesday, September 17 • 20:30 - 22:00

KDAB showed off a free CPU performance visualizer called [hotspot](https://github.com/KDAB/hotspot). They also have a memory analyzer called [heaptrack](https://github.com/KDAB/heaptrack).

# ISO SG14 Working Meeting
Wednesday, September 18 • All Day

First discussion was on optimized debug workflows. One idea is to optimize away most STL operations so that user code can be debugged more intently. The [paper](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1832r0.html) proposes changes to GCC and MSVC flags.

Someone presented on the `std::math::vector` and `std::math::Matrix` class proposal. It was basically this talk: [C++Now 2019: Bob Steagall “Linear Algebra for the Standard C++ Library”](https://www.youtube.com/watch?v=CslK9tu9ssA). It’s based on the 40 year old standard Basic Linear Algebra Subprograms (BLAS). The C++ proposal was already approved in SG14, but it needs some work to incorporate C++20 concepts. Concepts are easy to add, but almost impossible to remove so it will require extra care. Tensors were excluded [because they would take more time and may not be the right generalization](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1385r0.html#considered-but-excluded). The [strawman implementation](https://github.com/BobSteagall/wg21/tree/master/linear_algebra/code) may be worth a try, but the proposal is just the interface.

There was an early paper on declaring memory order and alignment. That way all the declarations could be put together. It would have a similar syntax to private, protected, or public. There is one point that it is not clear whether that means it can be declared partially or multiple times.

There’s a [proposal](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p0829r4.html) to mark more C++ features as freestanding. This feature list will encourage compiler writers to implement more basic C++ features. Then, kernel, microcontroller, and GPU programmers can use that subset. Basically, they want to allow all the abstraction, but not runtime allocation.

# Great Talks Already Seen Through the Bay Area Users Group
## Polymorphism != Virtual: Easy, Flexible Runtime Polymorphism Without Inheritance
Friday, September 20 • 10:30 - 11:30  
John Bandela

This talk starts pretty basic with overloading and inheritance. Then, it gets crazy with different dynamic polymorphism.

## Algorithm Intuition
Monday, September 16 • 14:00 - 16:15  
Conor Hoekstra

This talk builds on [CppCon 2018: Jonathan Boccara “105 STL Algorithms in Less Than an Hour”](https://www.youtube.com/watch?v=2olsGf6JIkU). The speaker goes through common transformations to make code simpler using algorithms from STL.

# Talks to See
## The C++20 Synchronization Library
Monday, September 16 • 11:00 - 12:00  
Bryce Adelstein Lelbach
## The Business Value of a Good API
Tuesday, September 17 • 14:00 - 15:00  
Bob Steagall
## Back to Basics: Object-Oriented Programming
Tuesday, September 17 • 16:45 - 17:45  
Jon Kalb
## There Are No Zero-cost Abstractions
Tuesday, September 17 • 16:45 - 17:45  
Chandler Carruth
## Killing Uninitialized Memory: Protecting the OS Without Destroying Performance
Wednesday, September 18 • 16:45 - 17:45  
Joe Bialek And Shayne Hiet-Block
## The Dawn of a New Error
Thursday, September 19 • 14:00 - 15:00  
Phil Nash
## The Truth of a Procedure
Thursday, September 19 • 14:00 - 15:00  
Lisa Lippincott

Based on her [cppcast interview](https://cppcast.com/lisa-lippincott-cppcon/), there are some details to relate functions to proofs.

## A Critical Look at the Coding Standards Landscape
Friday, September 20 • 10:30 - 11:30  
Michael Price
## Deprecating volatile
Friday, September 20 • 10:30 - 11:30  
JF Bastien
## Objects vs Values: Value Oriented Programming in an Object Oriented World
Friday, September 20 • 13:30 - 14:30  
Tony Van Eerd
## What is C++
Friday, September 20 • 14:45 - 15:45  
Chandler Carruth And Titus Winters
