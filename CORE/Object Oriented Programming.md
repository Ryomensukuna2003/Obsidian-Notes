Grouping of certain data types to form a single derived data type is known as **structs**

Let say we have knight in chess, we should have its position `(Int, int)`, captured `(Boolean)`, and color `(Boolean)`

As array group elements that are of same types, a struct groups elements of different data types

Main issue was that we cannot define a function in struct and can only reference them and to solve this programmers introduced **Objects**, also known as **Object Oriented Programming**

Objects are instances of class (Animal with 4 legs is class and Dogs, Cats and Tigers are Instances of that class)

Class is a template for Objects (Like a blueprint)

There are 4 main principles of Object Oriented Programming:

---

### 1. **Abstraction**

Data abstraction refers to providing only essential information about the data to the outside world, hiding the background details or implementation. 

- **Abstraction using Classes**  
    A Class can decide which data member will be visible to the outside world and which is not using access specifiers.
    
- **Abstraction in Header files**  
    Consider the `pow()` method present in `math.h` header file. Whenever we need to calculate the power of a number, we call the function `pow()` present in the `math.h` header file and pass the numbers as arguments without knowing the underlying algorithm.
    
- **Abstraction using Access Specifiers**
    
    - Members declared as **public** in a class can be accessed from anywhere in the program.
        
    - Members declared as **private** in a class can be accessed only from within the class. They are not allowed to be accessed from any part of the code outside the class.
        

---

### 2. **Encapsulation** (Hiding data Members)

Encapsulation involves combining similar data and functions into a single unit called a class. Encapsulation also helps to maintain the control the access of data.

**Class Access Modifiers**

- **Private:** Can only use class member or function listed as private within the class they belong to. Trying to access or view private data outside of their respective class results in an error.
    
- **Protected:** Protected members or functions are limited in their use as well. In addition to using protected members or functions within their class, we can also access protected information from within inherited classes.
    
- **Public:** We can use public members and functions anywhere else in our code.
    

---

### 3. **Inheritance**

**Types Of Inheritance:**

1. Single inheritance
2. Multilevel inheritance
3. Multiple inheritance
4. Hierarchical inheritance
5. Hybrid inheritance
    

- **1. Single Inheritance**  
    A class is allowed to inherit from only one class. i.e. one subclass is inherited by one base class only.  
    _Example: A basic employee system where `Manager` inherits properties from `Employee` like salary and emp ID._
    
- **2. Multiple Inheritance**  
    One **subclass** is inherited from more than one **base class**.  
    _Example: A smartphone that inherits features from both `Camera` and `Phone` classes._
    
- **3. Multilevel Inheritance**  
    A derived class is created from another derived class.  
    _Example: `Organism` → `Animal` → `Bird`._
    
- **4. Hierarchical Inheritance**  
    More than one subclass is inherited from a single base class.  
    _Example: `Vehicle` as a base class and `Car`, `Bike`, `Truck` as subclasses._
    
- **5. Hybrid (Virtual) Inheritance**  
    Hybrid Inheritance is implemented by combining more than one type of inheritance. For example: Combining Hierarchical inheritance and Multiple Inheritance.  
    _Example: A `HybridCar` inheriting properties from both `ElectricVehicle` and `FuelVehicle`, both of which inherit from `Vehicle`._

---

### 4. **Polymorphism**

Polymorphism is the ability of a message to be displayed in more than one form.  
e.g., A man at the same time is a father, a husband, and an employee.

---

## Types of Polymorphism

- **Compile-time Polymorphism**
- **Runtime Polymorphism**

#### 1. **Compile-Time Polymorphism**

This type of polymorphism is achieved by **function overloading** or **operator overloading**.

##### A. **Function Overloading**

When there are multiple functions with the same name but different parameters, then the functions are said to be **overloaded**.  
Functions can be overloaded by:

- **Changing the number of arguments**
- **Changing the type of arguments**

_Example:_

```cpp
void greet() {
    cout << "Hello, user!" << endl;
}

void greet(string name) {
    cout << "Hello, " << name << "!" << endl;
}

greet();         // Output: Hello, user!
greet("Shiv");   // Output: Hello, Shiv!
```

> The compiler chooses the function based on the number and type of arguments at **compile time**.

---

##### B. **Operator Overloading**

C++ has the ability to provide operators with a special meaning for a data type — this is called **operator overloading**.

_Example:_

```cpp
class Point {
public:
    int x, y;
    Point(int a, int b) : x(a), y(b) {}
    Point operator + (const Point& p) {
        return Point(x + p.x, y + p.y);
    }
};

Point p1(2, 3), p2(4, 5);
Point p3 = p1 + p2;  // Adds coordinates of p1 and p2
```

> The `+` operator is overloaded to add two `Point` objects like normal integers.

---

#### 2. **Runtime Polymorphism**

In contrast to compile-time polymorphism, the compiler binds the function call to the object **at runtime**, using **virtual functions** and **base class pointers**.

_Example:_

```cpp
class Animal {
public:
    virtual void sound() {
        cout << "Animal sound" << endl;
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Bark" << endl;
    }
};

Animal* a;
Dog d;
a = &d;
a->sound();  // Output: Bark
```

Even though the pointer is of type `Animal`, the `Dog`'s version of `sound()` is called at **runtime** due to the `virtual` keyword.