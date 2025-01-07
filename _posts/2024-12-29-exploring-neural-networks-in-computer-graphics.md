---
title: Compile-Time IDs for Fast Type Verification
tags: [Optimization, C++]
style: outline
color: primary
description: Producing type IDs during compile time for render-loop-optimal type verification.
---

### Motivation
While working on the frame graph for my 3D game engine, I came across a problem that I could solve by assigning type IDs to each class.

My type ID system had the following requirements:
- Each class must be assigned a unique ID.
- It must be possible to retrieve the ID using the class name.
- This system must work as quickly as possible, IDs will be retrieved (via class name) and compared against other IDs many times per frame.

### Initial Design - Runtime IDs
My initial approach involved using a C++ macro that would be placed in the body of any class which needed to be part of the ID system:

``` c++
    #define CLASS_BODY \
        private: \
        inline static int m_id = -1; // The default id \
        public: \
        static int* getIDRef();

    ...

    class A {
        CLASS_BODY
    }

    class B {
        CLASS_BODY
    }
```

Classes can then be registered with the engine, which assigns incrementing IDs:
``` c++
    engine.register(A::getIDRef()) // Assigns ID 0 to class A
    engine.register(B::getIDRef()) // Assigns ID 1 to class B
```

This method has the following <span style="color: red;">disadvantages</span>:
- Special care must be taken if each class must have the same ID for each invocation of the program. This is especially important if new classes are to be introduced to this system in the future.
- Registering many classes individually may be inconvenient.
- Forgetting to register classes will likely lead to undefined behaviour and runtime exceptions. 


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


This 








