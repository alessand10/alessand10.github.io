---
title: Compile-Time IDs for Fast Type Verification
tags: [Optimization, C++]
style: outline
color: primary
description: Producing type IDs during compile time for render-loop-optimal type verification.
---

### Problem Background
While working on the [frame graph]({{site.baseurl}}/blog/the-g3d-frame-graph) for my 3D game engine, I came across an interesting design task that challenged me to verify
the type of an object as quickly as possible. 

My type ID system had the following requirements:
- Each class must be assigned a unique integer ID.
- It must be possible to retrieve the ID using the class name or header.
- This system must work quickly, IDs will be retrieved and compared against other IDs many times per frame.

<br/>

### Initial Design - Runtime IDs
In my initial approach, I defined a C++ macro and placed it in the body of all classes that required an ID. The macro
provided a simple way of defining and retrieving a static id member variable for each class.

``` c++
    #define CLASS_BODY \
        private: \
        inline static int m_id = -1; // The default id \
        public: \
        static int* getIDRef() {return &m_id};

    ...

    class A {
        CLASS_BODY
    }

    class B {
        CLASS_BODY
    }
```

Classes could then be registered with the engine, which assigned incrementing IDs:
``` c++
    engine.register(A::getIDRef()) // Assigns ID 0 to class A
    engine.register(B::getIDRef()) // Assigns ID 1 to class B
```

This method has the following <span style="color: red;">disadvantages</span>:
- It may be challenging to ensure that each class has the same ID for each invocation of the program. This is especially true if new classes are expected to be added over time.
- Registering many classes individually may be inconvenient.
- Forgetting to register classes will likely lead to undefined behaviour and runtime exceptions. 

<br/>

### Improved Design - Compile Time IDs

While considering improvements to the runtime ID system, I considered an alternative; generating IDs at compile time using a hash. 

This system takes the class name as an argument and generates a 32-bit hash (using the FNV-1a hashing function) that serves as the ID for that class:

```c++
    // FNV-1a compile-time hash
    constexpr std::uint32_t fnv1a(const char* str, std::size_t i = 0, std::uint32_t hash = 2166136261u) {
        return str[i] == '\0' 
            ? hash 
            : fnv1a(str, i + 1, (hash ^ static_cast<unsigned char>(str[i])) * 16777619u);
    }

    #define STRING_HASH(s) fnv1a(#s)

    std::uint32_t idClassA = STRING_HASH(A)
    std::uint32_t idClassB = STRING_HASH(B)
```

The [FNV-1a](http://www.isthe.com/chongo/tech/comp/fnv/index.html#history) hash is an excellent choice to generate compile time IDs as it is both fast and has excellent dispersion.

This method offers the following <span style="color: green;">improvements</span> over the runtime-assignment approach:
- IDs are known at compile time and can be used for C++ metaprogramming, if desired.
- IDs will not change across two invocations of the same program. 
- The type ID of any class can be retrieved without requiring the header.

<br/>

### Hash collisions

Although rare, it is important to keep in mind that hash collisions are a possibility when using this approach. Having two classes unintentionally result in the same ID can create nightmare bugs that are very difficult to find. To avoid issues with hash collisions, a good approach might be to check for uniqueness during initialization. This [write-up]("https://softwareengineering.stackexchange.com/questions/49550/which-hashing-algorithm-is-best-for-uniqueness-and-speed") provides interesting insights into collisions when using FNV-1a.


<br/>
<br/>
<br/>



