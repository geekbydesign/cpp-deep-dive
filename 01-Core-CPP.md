# Module 1 — Core C++

> **Purpose:** Complete interview-revision notes for Core C++.
>
> **Standard:** C++17
>
> **Scope:** Types, variables, scope, lifetime, storage duration, `const`, `static`, `constexpr`, `auto`, `decltype`, references, pointers, casting, functions, overloading, `inline`, `extern`, and namespaces.
>
> These notes cover the concepts completely within the scope of Module 1. Topics that belong primarily to later modules are introduced only where necessary.

---

# 1. Types

## 1.1 What is a type?

A type tells the compiler:

- What kind of value an object can represent
- How the value is interpreted
- Which operations are valid
- How the object participates in expressions
- What conversions are possible

Example:

```cpp
int age = 29;
double salary = 21.5;
char grade = 'A';
bool active = true;
```

The variables have different types:

```text
age     -> int
salary  -> double
grade   -> char
active  -> bool
```

---

## 1.2 Fundamental types

### Boolean

```cpp
bool isReady = true;
bool isError = false;
```

`bool` represents:

```text
true
false
```

In arithmetic contexts:

```cpp
true  -> 1
false -> 0
```

---

### Character types

```cpp
char ch = 'A';
```

C++ also has:

```cpp
signed char
unsigned char
wchar_t
char8_t
char16_t
char32_t
```

`char` is commonly used for character data.

---

### Integer types

```cpp
short s = 10;
int i = 100;
long l = 1000;
long long ll = 1000000;
```

Unsigned versions:

```cpp
unsigned int x = 100;
unsigned long y = 1000;
```

Use `long long` when a larger integer range is required.

---

### Floating-point types

```cpp
float f = 3.14f;
double d = 3.14;
long double ld = 3.14L;
```

Usually:

```text
float       -> lower precision
double      -> higher precision
long double -> implementation-dependent extended precision
```

For most application code, `double` is the normal choice.

---

## 1.3 Signed and unsigned

```cpp
int x = -10;
unsigned int y = 10;
```

Signed integer types can represent negative and positive values.

Unsigned integer types represent only non-negative values.

### Important trap

```cpp
unsigned int x = 0;

--x;
```

This does not produce `-1`.

Unsigned arithmetic wraps modulo the range of the type.

### Signed/unsigned comparison trap

```cpp
int a = -1;
unsigned int b = 1;

if (a < b)
{
    // May be surprising
}
```

The signed value may be converted to unsigned before comparison.

**Interview point:** Mixing signed and unsigned values can produce unexpected results.

---

## 1.4 `void`

`void` represents the absence of a value.

```cpp
void print()
{
}
```

A function returning nothing uses `void`.

It can also appear as:

```cpp
void* ptr;
```

A `void*` is a pointer to an object of unknown type.

```cpp
int x = 10;

void* p = &x;
```

Before dereferencing, it must be converted to the correct pointer type:

```cpp
int* ip = static_cast<int*>(p);

std::cout << *ip;
```

`void*` does not carry type information.

---

## 1.5 `std::nullptr_t`

The type of `nullptr` is `std::nullptr_t`.

```cpp
auto x = nullptr;
```

Conceptually:

```text
decltype(nullptr) -> std::nullptr_t
```

Example:

```cpp
void test(int*);
void test(std::nullptr_t);

test(nullptr);
```

`nullptr` is preferred over `NULL` and `0` for null pointer values.

---

## 1.6 Arrays

An array contains a fixed number of elements of the same type.

```cpp
int numbers[5] = {1, 2, 3, 4, 5};
```

Elements are contiguous.

```cpp
std::cout << numbers[0];
std::cout << numbers[4];
```

Indexing starts at `0`.

---

## 1.7 Pointers

A pointer is a type that stores the address of another object.

```cpp
int x = 10;
int* p = &x;
```

Pointers are covered in detail later in these notes.

---

## 1.8 References

A reference is an alias for an existing object.

```cpp
int x = 10;
int& ref = x;
```

References are covered in detail later.

---

## 1.9 `struct`

A `struct` is a user-defined type.

```cpp
struct Point
{
    int x;
    int y;
};

Point p{10, 20};
```

By default:

```text
struct members -> public
```

This differs from a class, where members are private by default.

---

## 1.10 `class`

```cpp
class Car
{
    int speed;
};
```

By default:

```text
class members -> private
```

Classes and OOP are covered deeply in Module 2.

---

## 1.11 `enum`

```cpp
enum Color
{
    Red,
    Green,
    Blue
};

Color c = Red;
```

Enumerator values are integral values.

By default:

```text
Red   -> 0
Green -> 1
Blue  -> 2
```

---

## 1.12 `enum class`

Modern C++ generally prefers scoped enums when appropriate.

```cpp
enum class Color
{
    Red,
    Green,
    Blue
};

Color c = Color::Red;
```

Unlike an unscoped enum:

```cpp
// std::cout << Red;   // Not valid for enum class
```

You use:

```cpp
Color::Red
```

### Advantages

- Scoped names
- Better type safety
- No implicit conversion to `int`

Example:

```cpp
enum class Status
{
    Success,
    Failure
};

Status s = Status::Success;

// int x = s; // ERROR
```

---

## 1.13 Type aliases

### `using`

```cpp
using Integer = int;

Integer x = 10;
```

For complicated types:

```cpp
using IntVector = std::vector<int>;

IntVector values;
```

### `typedef`

Older syntax:

```cpp
typedef int Integer;

Integer x = 10;
```

Modern C++ generally prefers `using`.

---

# 2. Variables

## 2.1 Variable declaration

```cpp
int age;
```

This tells the compiler that `age` is an `int`.

---

## 2.2 Initialization

```cpp
int age = 29;
```

Initialization gives an object its initial value.

---

## 2.3 Assignment

```cpp
int age = 29;

age = 30;
```

The first operation is initialization.

The second is assignment.

---

## 2.4 Initialization styles

### Copy initialization

```cpp
int x = 10;
```

### Direct initialization

```cpp
int x(10);
```

### List initialization

```cpp
int x{10};
```

Modern C++ generally favors `{}` when appropriate.

---

## 2.5 Narrowing prevention

```cpp
int x{10.5}; // ERROR
```

But:

```cpp
int x = 10.5; // Allowed; value becomes 10
```

Brace initialization helps catch unintended narrowing.

---

## 2.6 Uninitialized variables

Local fundamental variables are not automatically initialized.

```cpp
void test()
{
    int x;

    // x has an indeterminate value
}
```

Reading such a value can result in undefined behavior.

Prefer:

```cpp
int x{};
```

which initializes it to zero.

---

## 2.7 Zero initialization

```cpp
int x{};
double d{};
bool b{};
```

Results:

```text
x -> 0
d -> 0.0
b -> false
```

---

## 2.8 `const` variables

```cpp
const int maxSize = 100;
```

The object cannot be modified through that const-qualified object.

```cpp
// maxSize = 200; // ERROR
```

---

## 2.9 Variable declaration vs definition

A declaration introduces a name/type.

```cpp
extern int value;
```

A definition actually defines the object:

```cpp
int value = 10;
```

For most local variables:

```cpp
int x = 10;
```

is both declaration and definition.

---

# 3. Scope

Scope describes where a name can be accessed.

---

## 3.1 Local/block scope

```cpp
void test()
{
    int x = 10;

    {
        int y = 20;

        std::cout << x;
        std::cout << y;
    }

    std::cout << x;

    // std::cout << y; // ERROR
}
```

`y` exists only inside the inner block.

---

## 3.2 Function parameter scope

```cpp
void print(int value)
{
    std::cout << value;
}
```

`value` is accessible within the function body.

---

## 3.3 Global/namespace scope

```cpp
int globalValue = 100;

void test()
{
    std::cout << globalValue;
}
```

The variable is declared at namespace scope.

---

## 3.4 Class scope

```cpp
class Person
{
    int age;

public:
    void setAge(int value)
    {
        age = value;
    }
};
```

`age` belongs to class scope.

---

## 3.5 Shadowing

```cpp
int value = 10;

void test()
{
    int value = 20;

    std::cout << value; // 20
}
```

The local variable shadows the global variable.

Use the global scope operator:

```cpp
std::cout << ::value;
```

to access the global variable.

---

## 3.6 Scope resolution operator `::`

```cpp
namespace A
{
    int value = 10;
}

namespace B
{
    int value = 20;
}

std::cout << A::value;
std::cout << B::value;
```

The `::` operator selects a name from a namespace/class/global scope.

---

# 4. Lifetime

Lifetime is the period during which an object exists.

Example:

```cpp
void test()
{
    int x = 10;
}
```

Conceptually:

```text
enter declaration
       ↓
object x begins lifetime
       ↓
function continues
       ↓
leave scope
       ↓
object x lifetime ends
```

---

## 4.1 Constructor/destructor example

```cpp
class Test
{
public:
    Test()
    {
        std::cout << "Constructed\n";
    }

    ~Test()
    {
        std::cout << "Destroyed\n";
    }
};

void test()
{
    Test obj;
}
```

Output:

```text
Constructed
Destroyed
```

The destructor runs when the object's lifetime ends.

---

## 4.2 Scope vs lifetime

These are not the same concept.

```text
Scope
    Where can I use the NAME?

Lifetime
    How long does the OBJECT exist?
```

A name may go out of scope while an object can still be alive in some situations, such as when another object/reference/pointer manages or refers to the underlying object.

---

## 4.3 Temporary lifetime

Expressions can create temporary objects.

```cpp
std::string result = std::string("Hello");
```

The temporary `std::string("Hello")` has a limited lifetime.

Temporary lifetime rules become particularly important with references.

---

## 4.4 Lifetime extension

A temporary can have its lifetime extended when bound to a suitable local `const` reference.

```cpp
const std::string& ref = std::string("Hello");
```

The temporary's lifetime is extended to the lifetime of `ref`.

Do not assume that every reference binding extends a temporary's lifetime. Lifetime-extension rules have important exceptions.

---

# 5. Storage Duration

Storage duration describes how long storage associated with an object exists.

C++ has four storage-duration categories:

```text
Automatic
Static
Thread
Dynamic
```

---

## 5.1 Automatic storage duration

Typical local variables:

```cpp
void test()
{
    int x = 10;
}
```

`x` has automatic storage duration.

Its storage is automatically managed when execution enters/leaves the relevant scope.

---

## 5.2 Static storage duration

Objects with static storage duration exist for the duration of the program.

Examples include:

```cpp
int globalValue = 10;
```

and:

```cpp
void counter()
{
    static int count = 0;
}
```

---

## 5.3 Thread storage duration

```cpp
thread_local int value = 0;
```

Each thread gets its own instance.

The object exists for the lifetime of the thread.

---

## 5.4 Dynamic storage duration

Objects created using dynamic allocation have dynamic storage duration.

```cpp
int* p = new int(10);

delete p;
```

Modern C++ generally prefers RAII and smart pointers instead of direct `new`/`delete`.

---

## 5.5 Scope, lifetime, storage duration

Keep these separate:

```text
Scope
    Visibility of a name

Lifetime
    Existence of an object

Storage duration
    Duration of the object's storage
```

---

# 6. `const`

`const` means that an object is not modifiable through that const-qualified access.

---

## 6.1 Const object

```cpp
const int x = 10;

// x = 20; // ERROR
```

A const object must be initialized.

```cpp
const int x; // ERROR
```

---

## 6.2 Pointer to const

```cpp
int x = 10;

const int* p = &x;
```

You cannot modify `x` through `p`:

```cpp
// *p = 20; // ERROR
```

But the pointer itself can change:

```cpp
int y = 30;

p = &y; // OK
```

Equivalent syntax:

```cpp
int const* p = &x;
```

---

## 6.3 Const pointer

```cpp
int x = 10;
int y = 20;

int* const p = &x;
```

The pointer cannot be changed:

```cpp
// p = &y; // ERROR
```

But the pointed object can be modified:

```cpp
*p = 50;
```

---

## 6.4 Const pointer to const

```cpp
const int* const p = &x;
```

Neither can be modified through `p`:

```cpp
// *p = 20; // ERROR
// p = &y;  // ERROR
```

### Easy rule

Read from right to left:

```cpp
const int* p;
```

`p` is a pointer to const int.

```cpp
int* const p;
```

`p` is a const pointer to int.

```cpp
const int* const p;
```

`p` is a const pointer to const int.

---

## 6.5 Const reference

```cpp
void print(const std::string& text)
{
    std::cout << text;
}
```

Benefits:

- No copy of the string
- Function cannot modify the argument through `text`

This is a very common C++ parameter pattern.

---

## 6.6 Top-level vs low-level const

```cpp
const int x = 10;
```

The object itself is const.

For pointers:

```cpp
const int* p;
```

The pointed-to object is const through `p`.

```cpp
int* const p = &x;
```

The pointer itself is const.

This distinction becomes important during type deduction and overload resolution.

---

## 6.7 `mutable`

A `mutable` data member can be modified even from a `const` member function.

```cpp
class Logger
{
    mutable int count = 0;

public:
    void log() const
    {
        ++count;
    }
};
```

`log()` is const, but `count` can change because it is `mutable`.

Typical use cases include caches, counters, and synchronization-related state.

---

## 6.8 `volatile`

`volatile` tells the compiler that accesses to the object have observable effects outside normal program flow and therefore should not be optimized away in the same manner as ordinary memory accesses.

```cpp
volatile int hardwareRegister;
```

`volatile` is relevant to certain low-level hardware/system programming situations.

**Important:** `volatile` does not make code thread-safe and does not replace `std::atomic`.

---

# 7. `static`

`static` has different meanings depending on where it is used.

---

## 7.1 Static local variable

```cpp
void counter()
{
    static int count = 0;

    ++count;

    std::cout << count << '\n';
}
```

```cpp
counter();
counter();
counter();
```

Output:

```text
1
2
3
```

The variable:

- Has local scope
- Has static storage duration
- Is initialized only once
- Retains its value between calls

---

## 7.2 Static initialization

A local static is initialized the first time execution reaches its declaration.

Since C++11, initialization of a function-local static is thread-safe.

```cpp
void getValue()
{
    static int value = expensiveCalculation();
}
```

The initialization itself is guaranteed to occur safely when multiple threads first reach it.

---

## 7.3 Static at namespace scope

```cpp
static int value = 10;
```

At namespace/file scope, `static` gives the object internal linkage.

The name is limited to the translation unit.

Modern C++ often uses an unnamed namespace instead:

```cpp
namespace
{
    int value = 10;
}
```

---

## 7.4 Static class member

```cpp
class Counter
{
public:
    static int count;
};

int Counter::count = 0;
```

There is one `count` shared by all `Counter` objects.

```cpp
Counter a;
Counter b;

++Counter::count;
```

Static class members are covered more deeply in Module 2.

---

## 7.5 Static function at namespace scope

```cpp
static void helper()
{
}
```

The function has internal linkage.

Only the current translation unit can use that name.

---

# 8. `constexpr`

`constexpr` indicates that an entity/function can participate in constant expressions when the required conditions are satisfied.

---

## 8.1 `constexpr` variable

```cpp
constexpr int size = 100;
```

This is a compile-time constant expression.

---

## 8.2 `const` vs `constexpr`

```cpp
const int a = 10;
constexpr int b = 10;
```

Both cannot be modified.

But `constexpr` is specifically intended for constant-expression evaluation.

Example:

```cpp
constexpr int size = 10;

int arr[size];
```

---

## 8.3 `constexpr` function

```cpp
constexpr int square(int x)
{
    return x * x;
}

constexpr int result = square(5);
```

The compiler can evaluate this at compile time.

---

## 8.4 `constexpr` function can run at runtime

```cpp
constexpr int square(int x)
{
    return x * x;
}

int x = 10;

int result = square(x);
```

The function is still valid; this invocation can be evaluated at runtime because `x` is not a constant expression.

---

## 8.5 `constexpr` and `consteval`

C++17 provides `constexpr`.

C++20 introduces `consteval`, which requires immediate compile-time evaluation.

Since this module is C++17-focused:

```text
constexpr -> compile-time capable
consteval  -> C++20, not part of C++17
```

---

# 9. `auto`

`auto` lets the compiler deduce the variable's type from its initializer.

```cpp
auto x = 10;      // int
auto d = 10.5;    // double
auto c = 'A';     // char
auto b = true;    // bool
```

`auto` is still statically typed.

```cpp
auto x = 10;

// x = "hello"; // ERROR
```

---

## 9.1 `auto` with const

```cpp
const int value = 10;

auto x = value;    // int
const auto y = value; // const int
```

By default, `auto` deduction does not preserve top-level `const`.

---

## 9.2 `auto` with references

```cpp
int value = 10;

int& ref = value;

auto a = ref;       // int
auto& b = ref;      // int&
```

`auto` normally deduces a value type.

Use `auto&` when you want a reference.

---

## 9.3 `auto` with pointers

```cpp
int x = 10;

int* p = &x;

auto q = p;       // int*
auto* r = p;      // int*
```

---

## 9.4 `auto` with `const`

```cpp
const int x = 10;

auto a = x;       // int
const auto b = x; // const int
```

For a reference:

```cpp
const int x = 10;

const auto& ref = x;
```

---

## 9.5 `auto` with arrays

```cpp
int arr[3] = {1, 2, 3};

auto a = arr;    // int*
auto& b = arr;   // int (&)[3]
```

This is an important deduction rule.

---

## 9.6 `auto` return type

```cpp
auto add(int a, int b)
{
    return a + b;
}
```

The return type is deduced from the return expression.

All return statements must deduce consistently.

---

# 10. `decltype`

`decltype` obtains the type associated with an expression according to special deduction rules.

---

## 10.1 `decltype(variable)`

```cpp
int x = 10;

decltype(x) y = 20;
```

`decltype(x)` is:

```text
int
```

---

## 10.2 `decltype` and references

```cpp
int x = 10;
int& ref = x;

decltype(ref) y = x;
```

`decltype(ref)` is:

```text
int&
```

---

## 10.3 `decltype((x))`

Important interview trap:

```cpp
int x = 10;

decltype(x) a = x;
decltype((x)) b = x;
```

Types:

```text
decltype(x)   -> int
decltype((x)) -> int&
```

Why?

`x` is an unparenthesized id-expression.

`(x)` is an lvalue expression.

---

## 10.4 Expression category rule

For general expressions:

```text
lvalue -> T&
xvalue -> T&&
prvalue -> T
```

Example:

```cpp
int x = 10;

decltype((x))        // int&
decltype(std::move(x)) // int&&
```

---

## 10.5 `auto` vs `decltype`

```cpp
int x = 10;
int& ref = x;

auto a = ref;          // int
decltype(ref) b = x;   // int&
```

Remember:

```text
auto     -> type deduction rules similar to template by-value deduction
decltype -> expression/category-sensitive rules
```

---

# 11. References

A reference is an alias for an existing object.

---

## 11.1 Lvalue reference

```cpp
int x = 10;

int& ref = x;
```

Changing the reference changes the original object:

```cpp
ref = 20;

std::cout << x; // 20
```

---

## 11.2 Reference must be initialized

```cpp
int& ref; // ERROR
```

A reference must be bound when it is declared.

---

## 11.3 Reference cannot be reseated

```cpp
int x = 10;
int y = 20;

int& ref = x;

ref = y;
```

This does not make `ref` refer to `y`.

It performs:

```text
x = y
```

So `x` becomes `20`.

---

## 11.4 References are aliases

```cpp
int x = 10;

int& a = x;
int& b = x;

a = 20;

std::cout << b; // 20
```

Both refer to the same object.

---

## 11.5 Pass by reference

```cpp
void increment(int& value)
{
    ++value;
}

int x = 10;

increment(x);

std::cout << x; // 11
```

No copy is made.

---

## 11.6 Pass by const reference

```cpp
void print(const std::string& value)
{
    std::cout << value;
}
```

Useful for efficiently passing large objects without modifying them.

---

## 11.7 Reference vs pointer

| Reference | Pointer |
|---|---|
| Alias | Stores address |
| Must be initialized | Can be null |
| Cannot normally be reseated | Can point elsewhere |
| Accessed directly | Dereferenced using `*` |
| Cannot be `nullptr` | Can be `nullptr` |
| Common for function parameters | Useful for optional/null relationships |

---

## 11.8 Reference to pointer

```cpp
int* p = nullptr;

int*& ref = p;

int x = 10;

ref = &x;

std::cout << *p;
```

`ref` is a reference to the pointer `p`.

---

## 11.9 Pointer to reference

A pointer cannot point to a reference directly.

```cpp
int x = 10;
int& ref = x;

// int&* p; // ERROR
```

References are aliases, not separate pointer-like objects that can be pointed to.

---

## 11.10 Rvalue references

C++ also has rvalue references:

```cpp
int&& ref = 10;
```

They are primarily used for:

- Move semantics
- Perfect forwarding
- Working with temporary/rvalue objects

Deep treatment belongs in Module 4, but the fundamental syntax is:

```cpp
T&&
```

---

# 12. Pointers

A pointer stores the address of an object.

---

## 12.1 Address-of operator

```cpp
int x = 10;

int* p = &x;
```

`&x` means:

```text
address of x
```

---

## 12.2 Dereference operator

```cpp
std::cout << *p;
```

`*p` accesses the object pointed to by `p`.

---

## 12.3 Modify through pointer

```cpp
int x = 10;

int* p = &x;

*p = 20;

std::cout << x; // 20
```

---

## 12.4 Null pointer

Use:

```cpp
int* p = nullptr;
```

Do not use:

```cpp
int* p = NULL;
```

or:

```cpp
int* p = 0;
```

`nullptr` is type-safe.

---

## 12.5 Dereferencing null is invalid

```cpp
int* p = nullptr;

// std::cout << *p; // Undefined behavior
```

Always ensure a pointer points to a valid object before dereferencing.

---

## 12.6 Pointer arithmetic

```cpp
int arr[] = {10, 20, 30};

int* p = arr;

std::cout << *p << '\n';       // 10
std::cout << *(p + 1) << '\n'; // 20
std::cout << *(p + 2) << '\n'; // 30
```

Pointer arithmetic is defined meaningfully within the same array object.

---

## 12.7 Pointer increment

```cpp
int arr[] = {10, 20, 30};

int* p = arr;

++p;

std::cout << *p; // 20
```

The pointer moves to the next element, taking the size of the pointed-to type into account.

---

## 12.8 Pointer to pointer

```cpp
int x = 10;

int* p = &x;
int** pp = &p;

std::cout << **pp; // 10
```

Conceptually:

```text
pp -> p -> x
```

---

## 12.9 Pointer to const

```cpp
const int* p = &x;
```

Cannot modify `x` through `p`.

---

## 12.10 Const pointer

```cpp
int* const p = &x;
```

Cannot make `p` point elsewhere.

---

## 12.11 Const pointer to const

```cpp
const int* const p = &x;
```

Neither can be modified through `p`.

---

## 12.12 Pointer to object

```cpp
class Car
{
public:
    void drive()
    {
        std::cout << "Driving\n";
    }
};

Car car;

Car* p = &car;

p->drive();
```

`->` is used to access members through a pointer.

Equivalent:

```cpp
(*p).drive();
```

---

## 12.13 Function pointer

Functions also have addresses.

```cpp
int add(int a, int b)
{
    return a + b;
}

int (*operation)(int, int) = add;

std::cout << operation(10, 20);
```

Function pointers are useful when selecting behavior dynamically.

---

## 12.14 Pointer to member function

A member function pointer has different syntax:

```cpp
class Calculator
{
public:
    int add(int a, int b)
    {
        return a + b;
    }
};

int (Calculator::*func)(int, int) = &Calculator::add;

Calculator c;

int result = (c.*func)(10, 20);
```

With a pointer to an object:

```cpp
Calculator* p = &c;

int result = (p->*func)(10, 20);
```

---

# 13. Casting

C++ provides four named casts:

```text
static_cast
dynamic_cast
const_cast
reinterpret_cast
```

There are also C-style casts.

Prefer named C++ casts because they make intent explicit.

---

# 13.1 Implicit conversion

The compiler can automatically convert compatible types.

```cpp
int x = 10;

double d = x;
```

`x` is converted to `double`.

---

# 13.2 `static_cast`

Used for well-defined compile-time conversions.

```cpp
double d = 10.5;

int x = static_cast<int>(d);
```

Result:

```text
10
```

Another example:

```cpp
int x = 10;

double d = static_cast<double>(x);
```

---

## 13.3 Avoid old C-style casts

Instead of:

```cpp
int x = (int)d;
```

prefer:

```cpp
int x = static_cast<int>(d);
```

The C++ cast clearly communicates the intended conversion.

---

# 13.4 `dynamic_cast`

Used for runtime-checked casts in polymorphic class hierarchies.

```cpp
class Base
{
public:
    virtual ~Base() = default;
};

class Derived : public Base
{
public:
    void show()
    {
        std::cout << "Derived\n";
    }
};

Base* base = new Derived;

Derived* derived = dynamic_cast<Derived*>(base);

if (derived)
{
    derived->show();
}

delete base;
```

If the cast fails for a pointer:

```cpp
dynamic_cast<Derived*>(base)
```

returns `nullptr`.

`dynamic_cast` requires a polymorphic base class for the usual runtime downcast use case.

Detailed polymorphism belongs in Module 2.

---

# 13.5 `const_cast`

Used to add/remove `const` or `volatile` qualification.

```cpp
const int x = 10;

int* p = const_cast<int*>(&x);
```

Important:

If the original object was actually defined as `const`, modifying it through the casted pointer results in undefined behavior.

```cpp
const int x = 10;

int* p = const_cast<int*>(&x);

// *p = 20; // Undefined behavior
```

`const_cast` should be used sparingly.

---

# 13.6 `reinterpret_cast`

Used for low-level reinterpretation.

```cpp
int x = 10;

char* p = reinterpret_cast<char*>(&x);
```

It does not perform a normal value conversion. It reinterprets the pointer/value representation according to the cast rules.

Common in:

- Low-level systems code
- Hardware interfaces
- Certain serialization/binary interfaces

It is dangerous if used incorrectly.

---

# 13.7 Cast summary

| Cast | Main purpose |
|---|---|
| `static_cast` | Normal explicit conversions |
| `dynamic_cast` | Runtime-checked polymorphic cast |
| `const_cast` | Add/remove const/volatile qualification |
| `reinterpret_cast` | Low-level reinterpretation |

---

# 14. Functions

A function packages reusable behavior.

```cpp
int add(int a, int b)
{
    return a + b;
}
```

Usage:

```cpp
int result = add(10, 20);
```

---

## 14.1 Function declaration

```cpp
int add(int, int);
```

This tells the compiler the function exists.

---

## 14.2 Function definition

```cpp
int add(int a, int b)
{
    return a + b;
}
```

This provides the implementation.

---

## 14.3 Declaration + definition

A definition also serves as a declaration.

```cpp
int add(int a, int b)
{
    return a + b;
}
```

---

## 14.4 Pass by value

```cpp
void modify(int x)
{
    x = 100;
}

int value = 10;

modify(value);

std::cout << value; // 10
```

A copy is passed.

---

## 14.5 Pass by pointer

```cpp
void modify(int* x)
{
    *x = 100;
}

int value = 10;

modify(&value);

std::cout << value; // 100
```

---

## 14.6 Pass by reference

```cpp
void modify(int& x)
{
    x = 100;
}

int value = 10;

modify(value);

std::cout << value; // 100
```

---

## 14.7 Pass by const reference

```cpp
void print(const std::string& value)
{
    std::cout << value;
}
```

Useful when you want:

- No unnecessary copy
- No modification

---

## 14.8 Return by value

```cpp
int createValue()
{
    return 42;
}
```

Modern C++ handles return-by-value efficiently through copy elision and move semantics where applicable.

---

## 14.9 Return by reference

```cpp
int& getValue(int& x)
{
    return x;
}

int value = 10;

getValue(value) = 20;

std::cout << value; // 20
```

Never return a reference to a local variable:

```cpp
int& bad()
{
    int x = 10;

    return x; // ERROR in terms of correctness: dangling reference
}
```

The local object is destroyed when the function returns.

---

## 14.10 Return by pointer

```cpp
int* getValue(int* p)
{
    return p;
}
```

Again, never return a pointer to a local variable:

```cpp
int* bad()
{
    int x = 10;

    return &x; // dangling pointer
}
```

---

## 14.11 Default arguments

```cpp
int power(int value, int exponent = 2)
{
    // implementation
    return 0;
}
```

Usage:

```cpp
power(5);
power(5, 3);
```

Default arguments are normally specified in the declaration.

---

## 14.12 Function overloading

```cpp
int add(int a, int b)
{
    return a + b;
}

double add(double a, double b)
{
    return a + b;
}
```

The compiler selects the appropriate overload.

---

## 14.13 Function pointer

```cpp
int add(int a, int b)
{
    return a + b;
}

int (*func)(int, int) = add;

int result = func(10, 20);
```

---

## 14.14 `constexpr` function

```cpp
constexpr int square(int x)
{
    return x * x;
}
```

Can be used in constant expressions when called with constant-expression arguments.

---

## 14.15 `noexcept` basic awareness

A function can specify that it does not throw exceptions:

```cpp
void process() noexcept
{
}
```

If an exception escapes a `noexcept` function, the program calls `std::terminate`.

Exception safety is covered in Module 10.

---

# 15. Function Overloading

Function overloading allows multiple functions with the same name but different parameter lists.

---

## 15.1 Valid overloads

```cpp
void print(int x)
{
}

void print(double x)
{
}

void print(const std::string& x)
{
}
```

---

## 15.2 Return type alone cannot overload

Invalid:

```cpp
int getValue();
double getValue(); // ERROR
```

The compiler cannot distinguish calls based only on return type.

---

## 15.3 Parameter count

```cpp
void print(int x)
{
}

void print(int x, int y)
{
}
```

Valid overloads.

---

## 15.4 Parameter type

```cpp
void print(int x)
{
}

void print(double x)
{
}
```

Valid.

---

## 15.5 `const` and member-function overloading

A member function can be overloaded based on its cv-qualification.

```cpp
class Test
{
public:
    void print()
    {
        std::cout << "non-const\n";
    }

    void print() const
    {
        std::cout << "const\n";
    }
};
```

The appropriate overload is selected based on whether the object is const.

This becomes important with classes and OOP.

---

## 15.6 Ambiguous overload

```cpp
void print(int);
void print(double);

print(1.5);
```

This chooses `double`.

But overloads can become ambiguous when multiple conversions are equally viable.

```cpp
void test(long);
void test(double);

// test(10); // Depending on candidates/conversions, can become ambiguous
```

Understanding overload resolution is more important than memorizing every ranking rule initially.

---

# 16. `inline`

`inline` is frequently misunderstood.

```cpp
inline int square(int x)
{
    return x * x;
}
```

It does **not** mean:

> "The compiler must replace the function call with the function body."

The compiler decides whether to inline as an optimization.

---

## 16.1 Main language purpose

An inline function can have the same definition in multiple translation units, subject to the One Definition Rule.

This is why small functions can safely be defined in headers.

```cpp
// math.h

inline int square(int x)
{
    return x * x;
}
```

Multiple source files can include the header.

---

## 16.2 Functions defined inside a class

A function defined inside a class definition is implicitly inline.

```cpp
class Calculator
{
public:
    int add(int a, int b)
    {
        return a + b;
    }
};
```

---

## 16.3 `inline` variable

C++17 introduced inline variables.

```cpp
inline int globalValue = 10;
```

An inline variable can be defined in a header and included by multiple translation units while satisfying the relevant ODR requirements.

---

# 17. `extern`

`extern` commonly declares an entity that is defined elsewhere.

---

## 17.1 Global variable

File 1:

```cpp
int globalValue = 100;
```

File 2:

```cpp
extern int globalValue;

void test()
{
    std::cout << globalValue;
}
```

The second file does not define another `globalValue`.

It declares that the definition exists elsewhere.

---

## 17.2 Declaration vs definition

```cpp
extern int value; // declaration
```

versus:

```cpp
int value = 10;   // definition
```

---

## 17.3 Functions

Functions normally have external linkage unless their linkage is changed.

Header:

```cpp
int add(int a, int b);
```

Source:

```cpp
int add(int a, int b)
{
    return a + b;
}
```

The declaration allows other translation units to call the function.

---

## 17.4 `extern "C"`

C++ supports language linkage specifications.

```cpp
extern "C"
{
    void cFunction();
}
```

This is commonly used when interoperating with C code.

The exact ABI/name-mangling details are implementation-dependent and belong more deeply in Module 9.

---

# 18. Namespaces

Namespaces organize names and prevent collisions.

---

## 18.1 Basic namespace

```cpp
namespace Math
{
    int add(int a, int b)
    {
        return a + b;
    }
}
```

Use:

```cpp
int result = Math::add(10, 20);
```

---

## 18.2 Multiple namespaces can contain same names

```cpp
namespace A
{
    void print()
    {
        std::cout << "A\n";
    }
}

namespace B
{
    void print()
    {
        std::cout << "B\n";
    }
}
```

Use:

```cpp
A::print();
B::print();
```

---

## 18.3 Nested namespaces

```cpp
namespace Company
{
    namespace Project
    {
        int version = 1;
    }
}
```

C++17 allows:

```cpp
namespace Company::Project
{
    int version = 1;
}
```

---

## 18.4 Namespace alias

```cpp
namespace VeryLongNamespaceName
{
    int value = 10;
}

namespace V = VeryLongNamespaceName;

std::cout << V::value;
```

---

## 18.5 `using` declaration

```cpp
namespace A
{
    void print();
    void process();
}

using A::print;

print();
```

Only the selected name is introduced.

---

## 18.6 `using namespace`

```cpp
using namespace std;
```

This makes names available without the `std::` qualification.

For example:

```cpp
cout << "Hello";
```

However, avoid `using namespace std;` in header files because it can introduce unwanted name collisions.

Prefer:

```cpp
std::cout
std::string
std::vector
```

---

## 18.7 Anonymous namespace

```cpp
namespace
{
    int helperValue = 10;

    void helper()
    {
    }
}
```

Names in an unnamed namespace have internal linkage within the translation unit.

This is commonly used instead of namespace-scope `static` for internal implementation details.

---

## 18.8 Namespace and scope

A namespace introduces a scope.

```cpp
namespace A
{
    int x = 10;
}
```

Access:

```cpp
A::x
```

---

# 19. Linkage

Linkage determines whether a name refers to the same entity across scopes/translation units.

The important categories for interviews are:

```text
No linkage
Internal linkage
External linkage
```

---

## 19.1 No linkage

A local variable usually has no linkage.

```cpp
void test()
{
    int x = 10;
}
```

The name `x` is local to that scope.

---

## 19.2 Internal linkage

The name is available only within the current translation unit.

Example:

```cpp
static int value = 10;
```

or:

```cpp
namespace
{
    int value = 10;
}
```

---

## 19.3 External linkage

The name can refer to an entity across translation units.

```cpp
int globalValue = 10;
```

Another source file can declare:

```cpp
extern int globalValue;
```

---

# 20. Translation Units

A source file after preprocessing is effectively a translation unit.

For example:

```text
main.cpp
math.cpp
math.h
```

`main.cpp` and `math.cpp` become separate translation units after preprocessing.

This matters for:

- `static`
- `extern`
- `inline`
- namespaces
- ODR
- linkage

Detailed ODR/ABI behavior belongs in Module 9, but the basic concept is important here.

---

# 21. Name Lookup

When the compiler sees a name:

```cpp
value
```

it needs to determine which declaration that name refers to.

C++ performs name lookup according to scope and language rules.

Example:

```cpp
int value = 10;

void test()
{
    int value = 20;

    std::cout << value;
}
```

The local declaration is found first.

---

## 21.1 Qualified lookup

```cpp
namespace A
{
    int value = 10;
}

std::cout << A::value;
```

The namespace qualification tells the compiler exactly where to look.

---

# 22. Argument-Dependent Lookup (Basic Awareness)

Argument-Dependent Lookup, or ADL, allows C++ to consider functions in namespaces associated with the argument types.

Example:

```cpp
namespace Math
{
    struct Number
    {
    };

    void process(Number)
    {
        std::cout << "Math::process\n";
    }
}

Math::Number n;

process(n);
```

Even though `process` is not explicitly written as:

```cpp
Math::process(n);
```

ADL can find it because `Number` belongs to namespace `Math`.

ADL becomes particularly important with:

- operator overloading
- templates
- generic code
- customization functions

A deeper treatment belongs in Module 9.

---

# 23. Important Type Qualifiers

## `const`

Prevents modification through the const-qualified access.

```cpp
const int x = 10;
```

---

## `volatile`

Used when an object may be changed by mechanisms outside normal program flow.

```cpp
volatile int registerValue;
```

Does not provide atomicity or thread synchronization.

---

## `mutable`

Allows a data member to be modified even through a const member function.

```cpp
class Counter
{
    mutable int count = 0;

public:
    void increment() const
    {
        ++count;
    }
};
```

---

# 24. Important Pointer/Reference Traps

## Null pointer

```cpp
int* p = nullptr;
```

Safe to compare:

```cpp
if (p != nullptr)
{
}
```

Do not dereference it.

---

## Dangling pointer

A pointer referring to an object whose lifetime has ended.

```cpp
int* bad()
{
    int x = 10;

    return &x;
}
```

The returned pointer is dangling.

---

## Dangling reference

```cpp
int& bad()
{
    int x = 10;

    return x;
}
```

The returned reference is dangling.

---

## Wild/uninitialized pointer

```cpp
int* p;
```

`p` contains an indeterminate value.

Do not dereference it.

Prefer:

```cpp
int* p = nullptr;
```

---

# 25. Important `const` Interview Examples

```cpp
int x = 10;
int y = 20;

const int* p1 = &x;
int* const p2 = &x;
const int* const p3 = &x;
```

### `p1`

```text
pointer can change
pointed value cannot be modified through p1
```

### `p2`

```text
pointer cannot change
pointed value can be modified through p2
```

### `p3`

```text
pointer cannot change
pointed value cannot be modified through p3
```

---

# 26. `auto` vs `decltype` — Interview Table

| Expression | Result |
|---|---|
| `auto x = value` | value type |
| `auto& x = value` | lvalue reference |
| `const auto& x = value` | const reference |
| `decltype(x)` | type according to decltype rules |
| `decltype((x))` | usually reference for lvalue |
| `decltype(std::move(x))` | rvalue reference |

Example:

```cpp
int x = 10;
int& ref = x;

auto a = ref;
decltype(ref) b = x;
decltype((x)) c = x;
```

Types:

```text
a -> int
b -> int&
c -> int&
```

---

# 27. Initialization and `const` / `constexpr`

```cpp
const int runtimeValue = getValue();

constexpr int compileTimeValue = 100;
```

A `const` object does not necessarily mean its value is known at compile time.

For example:

```cpp
int getValue();

const int x = getValue();
```

`x` cannot be modified, but its value is obtained at runtime.

A `constexpr` object must be initialized with a constant expression.

---

# 28. Static vs Const vs Constexpr

These keywords solve different problems.

### `const`

```cpp
const int x = 10;
```

Means the object cannot be modified through the const-qualified access.

### `constexpr`

```cpp
constexpr int x = 10;
```

Indicates compile-time constant-expression capability.

### `static`

```cpp
static int x = 10;
```

The meaning depends on context. For a local variable it gives static storage duration; at namespace scope it can give internal linkage.

Do not treat these as interchangeable concepts.

---

# 29. Function Parameter Design

For interview questions such as:

> "How would you pass this object to a function?"

Think about:

### Small object, no modification

```cpp
void process(int value);
```

Pass by value.

### Large object, no modification

```cpp
void process(const std::string& value);
```

Const reference.

### Need to modify caller's object

```cpp
void process(std::string& value);
```

Non-const reference.

### Optional object/address semantics

```cpp
void process(std::string* value);
```

A pointer can represent "no object":

```cpp
process(nullptr);
```

This is a design choice; it should not be used merely because references exist.

---

# 30. Core C++ Interview Distinctions

## Scope vs lifetime

```text
Scope
    Where a NAME is accessible.

Lifetime
    How long an OBJECT exists.
```

---

## Pointer vs reference

```text
Pointer
    Stores address
    Can be nullptr
    Can be reseated

Reference
    Alias
    Must be initialized
    Cannot normally be reseated
```

---

## `const` vs `constexpr`

```text
const
    Cannot modify through const-qualified access.

constexpr
    Compile-time constant-expression capability.
```

---

## `auto` vs `decltype`

```text
auto
    Deduce a type from an initializer.

decltype
    Determine type using expression-specific rules.
```

---

## `static` local vs global `static`

```text
static local
    Local scope
    Static storage duration
    Retains value between calls

static namespace variable
    Internal linkage
```

---

## `extern` vs `static`

```text
extern
    Refers to an entity that can be defined elsewhere.

static at namespace scope
    Internal linkage.
```

---

## `inline` vs compiler inlining

```text
inline
    Language/ODR property.

Inlining optimization
    Compiler decision.
```

They are not the same thing.

---

# 31. Common Interview Traps

### Trap 1

```cpp
int& ref;
```

Invalid because a reference must be initialized.

---

### Trap 2

```cpp
int x = 10;
int y = 20;

int& ref = x;

ref = y;
```

`ref` still refers to `x`.

Result:

```text
x = 20
y = 20
```

---

### Trap 3

```cpp
const int* p;
```

The pointer is not const.

The pointed-to integer is const through `p`.

---

### Trap 4

```cpp
int* const p = &x;
```

The pointer itself is const.

---

### Trap 5

```cpp
auto x = constInt;
```

Top-level const is generally dropped during `auto` deduction.

---

### Trap 6

```cpp
decltype(x)
```

is not always equivalent to:

```cpp
decltype((x))
```

For an ordinary lvalue variable:

```text
decltype(x)   -> T
decltype((x)) -> T&
```

---

### Trap 7

```cpp
const int x = getValue();
```

`const` does not mean compile-time constant.

---

### Trap 8

```cpp
inline
```

does not guarantee that the compiler will inline the function.

---

### Trap 9

```cpp
int* p = nullptr;
*p = 10;
```

Undefined behavior.

---

### Trap 10

```cpp
int* bad()
{
    int x = 10;
    return &x;
}
```

Returns a dangling pointer.

---

# 32. Compact Revision Sheet

## Types

```text
Fundamental:
bool, char, integer types, floating-point types, void

Derived:
arrays, pointers, references, functions

User-defined:
class, struct, enum, enum class

Aliases:
using, typedef
```

## Variables

```text
Declaration
Definition
Initialization
Assignment
const
Type deduction
```

## Scope

```text
Block
Function
Class
Namespace
Global
Shadowing
```

## Lifetime

```text
Object existence period
```

## Storage duration

```text
Automatic
Static
Thread
Dynamic
```

## `const`

```text
const object
pointer to const
const pointer
const pointer to const
const reference
mutable
volatile
```

## `static`

```text
local static
namespace static
static function
static class member
static storage duration
internal linkage
```

## `constexpr`

```text
compile-time constant expression
constexpr variable
constexpr function
```

## `auto`

```text
type deduction
auto&
const auto&
auto*
return type deduction
```

## `decltype`

```text
decltype(x)
decltype((x))
expression category effects
```

## References

```text
lvalue reference
const reference
reference to pointer
rvalue reference basics
```

## Pointers

```text
address
dereference
nullptr
pointer arithmetic
pointer to pointer
pointer to const
const pointer
function pointer
member-function pointer
```

## Casting

```text
implicit conversion
static_cast
dynamic_cast
const_cast
reinterpret_cast
C-style cast
```

## Functions

```text
declaration
definition
parameters
return values
pass by value
pass by pointer
pass by reference
default arguments
function pointer
constexpr
noexcept
```

## Overloading

```text
same name
different parameter list
return type alone cannot overload
const member overload
overload resolution
ambiguity
```

## `inline`

```text
ODR
header definitions
inline variables
not a guarantee of compiler inlining
```

## `extern`

```text
declaration
definition elsewhere
external linkage
extern "C"
```

## Namespaces

```text
namespace
nested namespace
namespace alias
using declaration
using namespace
anonymous namespace
namespace scope
basic ADL
```

---

# 33. Final Mental Model

When reviewing Core C++, think in this order:

```text
TYPE
  ↓
What kind of object is this?

VARIABLE
  ↓
What name refers to it?

SCOPE
  ↓
Where can that name be accessed?

STORAGE DURATION
  ↓
How long does its storage exist?

LIFETIME
  ↓
How long does the object itself exist?

CONST / STATIC / CONSTEXPR
  ↓
What restrictions or storage/evaluation properties apply?

AUTO / DECLTYPE
  ↓
How is the type determined?

REFERENCE / POINTER
  ↓
How is the object accessed indirectly?

CASTING
  ↓
How is one type converted/reinterpreted?

FUNCTION
  ↓
How is behavior organized?

OVERLOADING
  ↓
How does the compiler select among functions?

INLINE / EXTERN
  ↓
How do definitions and linkage work across translation units?

NAMESPACE
  ↓
How are names organized and collisions avoided?
```

---

# Module 1 — Must-Know Interview Questions

Before considering Module 1 complete, you should be able to answer these without looking at the notes:

1. What is the difference between scope and lifetime?
2. What are the four storage-duration categories?
3. What is the difference between declaration and definition?
4. What is the difference between `const` and `constexpr`?
5. Explain `const int*`, `int* const`, and `const int* const`.
6. Why can a reference not be reseated?
7. Pointer vs reference — when would you use each?
8. What is a dangling pointer?
9. What is a dangling reference?
10. Why is `nullptr` preferred over `NULL`?
11. What does `auto` deduce in different const/reference situations?
12. What is the difference between `auto` and `decltype`?
13. Why are `decltype(x)` and `decltype((x))` different?
14. What does `static` mean for a local variable?
15. What does `static` mean at namespace scope?
16. What does `static` mean for a class member?
17. What is internal linkage?
18. What is external linkage?
19. What does `extern` do?
20. What is an anonymous namespace?
21. What does `inline` actually mean?
22. Does `inline` guarantee compiler inlining?
23. What are the four C++ casts?
24. When would you use `static_cast`?
25. When is `dynamic_cast` useful?
26. Why is modifying an originally `const` object through `const_cast` dangerous?
27. What is `reinterpret_cast`?
28. Can functions be overloaded based only on return type?
29. What is function overloading?
30. What are default arguments?
31. What is a function pointer?
32. What is a member-function pointer?
33. What is `volatile`?
34. Does `volatile` make code thread-safe?
35. What is `mutable`?
36. What is the difference between `enum` and `enum class`?
37. What is a type alias?
38. What is a translation unit?
39. What is name lookup?
40. What is basic Argument-Dependent Lookup (ADL)?
