# Resource Acquisition Is Initialization (RAII)

title: Resource Acquisition Is Initialization (RAII)
date: 2015-10-07 18:00:45
tags: 
- C++
- RAII

---

**"Resource Acquisition Is Initialization"** or **RAII**, is a `C++` programming technique which binds the life cycle of a resource (allocated memory, open socket, open file, mutex, database connection - anything that exists in limited supply) to the lifetime of an object with automatic storage duration.

<!-- more -->

**RAII** guarantees that the resource is available to any function that may access the object (resource availability is a class invariant). *It also guarantees that all resources are released when their controlling objects go out of scope, in reverse order of acquisition.* Likewise, if resource acquisition fails (the constructor exits with an exception), all resources acquired by every fully-constructed member and base subobject are released in reverse order of initialization. 

**RAII** can be summarized as follows:
- encapsulate each resource into a class, where the constructor acquires the resource and establishes all class invariants or throws an exception if that cannot be done 
- the destructor releases the resource and never throws exceptions

always use the resource via an instance of a RAII-class that either

- has automatic storage duration
- is a non-static member of a class whose instance has automatic storage duration

Classes with `open()`/`close()`, `lock()`/`unlock()`, or `init()`/`copyFrom()`/`destroy()` member functions are typical examples of **non-RAII** classes:


``` C++
std::mutex m;
void bad() 
{
    m.lock(); // acquire the mutex
    f(); // if f() throws an exception, the mutex is never released
    if(!everything_ok()) return; // early return, the mutex is never released
    m.unlock(); // only if execution reaches this statement, the mutex is released
}
void good()
{
    std::lock_guard<std::mutex> lk(m); // RAII class: mutex acquisition is initialization
    f(); // if f() throws an exception, the mutex is released
    if(!everything_ok()) return; // early return, the mutex is released
} // if the function returns normally, the mutex is released
```

The C++ library classes or wrappers that manage their own resources follow RAII:
- `std::string`
- `std::vector`
- `std::thread`
- `std::unique_ptr`
- `std::shared_ptr`
- `std::lock_gurad`
- `std::unique_lock`
- `std::shared_lock`


Adapted from [articles on cppreference.com](http://en.cppreference.com/w/cpp/language/raii).

@(Learning Cards)[Marxico|C++]

