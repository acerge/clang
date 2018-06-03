# C++基础

## 1.1 Header File Naming Conventions

![](/assets/import.png)

### 1.2 C++11 String Initialization

As you might expect by now, C++11 enables list-initialization for C-style strings and

string objects:

```cpp
char first_date[] = {"Le Chapon Dodu"};
char second_date[] {"The Elegant Plate"};
string third_date = {"The Bread Bowl"};
string fourth_date {"Hank's Fine Eats"};
```

```cpp
struct inflatable goose; // keyword struct required in C
inflatable vincent; // keyword struct not required in C++
```

```cpp
enum spectrum {red, orange, yellow, green, blue, violet, indigo, ultraviolet};
//It establishes red, orange, yellow, and so on, as symbolic constants for the integer
//values 0–7.These constants are called enumerators.
band = spectrum(3);
//You can set enumerator values explicitly by using the assignment operator:
enum bits{one = 1, two = 2, four = 4, eight = 8};
```

### 1.3 const and pointer

```cpp
//1.Pointer to const value:A pointer to a const value is a (non-const) pointer that points to a constant value.
//1.1
int value = 5;
int *ptr = &value;
*ptr = 6; // change value to 6

//1.2
const int value = 5; // value is const
int *ptr = &value; // compile error: cannot convert const int* to int*
*ptr = 6; // change value to 6

//1.3
const int value = 5;
const int *ptr = &value; // this is okay, ptr is a non-const pointer that is pointing to a "const int"
*ptr = 6; // not allowed, we can't change a const value

//1.4
int value = 5; // value is not constant
const int *ptr = &value; // this is still okay

//1.5
int value = 5;
const int *ptr = &value; // ptr points to a "const int"
value = 6; // the value is non-const when accessed through a non-const identifier

//1.6
int value = 5;
const int *ptr = &value; // ptr points to a "const int"
*ptr = 6; // ptr treats its value as const, so changing the value through ptr is not legal

//1.7
int value1 = 5;
const int *ptr = &value1; // ptr points to a const int

int value2 = 6;
ptr = &value2; // okay, ptr now points at some other const int

//2.Const pointers: A const pointer is a pointer whose value can not be changed after initialization
//2.1
int value = 5;
int *const ptr = &value;

//2.2
int value1 = 5;
int value2 = 6;

int * const ptr = &value1; // okay, the const pointer is initialized to the address of value1
ptr = &value2; // not okay, once initialized, a const pointer can not be changed.

//2.3
int value = 5;
int *const ptr = &value; // ptr will always point to value
*ptr = 6; // allowed, since ptr points to a non-const int

//2.4
int value = 5;
const int *const ptr = &value;

//3.To summarize, you only need to remember 4 rules, and they are pretty logical:

/*
A non-const pointer can be redirected to point to other addresses.
A const pointer always points to the same address, and this address can not be changed. 

A pointer to a non-const value can change the value it is pointing to. These can not point to a const value.
A pointer to a const value treats the value as const (even if it is not), and thus can not change the value 
it is pointing to. */
```

### 1.4 C++ only feature about function

inline function

reference variable

```cpp
int rats;
int & rodents = rats; // makes rodents an alias for rats
rodents++;

int rats = 101;
int & rodents = rats; // rodents a reference
int * prats = &rats; // prats a pointer

void swapr(int &a, int &b) // use references
{
    int temp;
    temp = a; // use a, b for values of variables
    a = b;
    b = temp;
}
void swapp(int *p, int *q) // use pointers
{
    int temp;
    temp = *p; // use *p, *q for values of variables
    *p = *q;
    *q = temp;
}
int wallet1 = 300;
int wallet2 = 350;
swapr(wallet1, wallet2); // pass variables
swapp(&wallet1, &wallet2); // pass addresses of variables

```

reference example

```cpp
//strc_ref.cpp -- using structure references
#include <iostream>
#include <string>
struct free_throws
{
    std::string name;
    int made;
    int attempts;
    float percent;
};
void display(const free_throws &ft);
void set_pc(free_throws &ft);
free_throws &accumulate(free_throws &target, const free_throws &source);
const free_throws &accumulate2(free_throws &target, const free_throws &source);
int main()
{
    // partial initializations – remaining members set to 0
    free_throws one = {"Ifelsa Branch", 13, 14};
    free_throws two = {"Andor Knott", 10, 16};
    free_throws three = {"Minnie Max", 7, 9};
    free_throws four = {"Whily Looper", 5, 9};
    free_throws five = {"Long Long", 6, 14};
    free_throws team = {"Throwgoods", 0, 0};
    // no initialization
    free_throws dup;
    set_pc(one);
    display(one);
    accumulate(team, one);
    display(team);
    // use return value as argument
    display(accumulate(team, two));
    accumulate(accumulate(team, three), four);
    display(team);
    // use return value in assignment
    dup = accumulate(team, five);
    std::cout << "Displaying team:\n";
    display(team);
    std::cout << "Displaying dup after assignment:\n";
    display(dup);
    set_pc(four);
    // ill-advised assignment: 
    accumulate(dup, five) = four;
    std::cout << "Displaying dup after ill-advised assignment:\n";
    display(dup);
    // accumulate2(dup, five) = four; not allowed
    return 0;
}
void display(const free_throws &ft)
{
    using std::cout;
    cout << "Name: " << ft.name << '\n';
    cout << " Made: " << ft.made << '\t';
    cout << "Attempts: " << ft.attempts << '\t';
    cout << "Percent: " << ft.percent << '\n';
}
void set_pc(free_throws &ft)
{
    if (ft.attempts != 0)
        ft.percent = 100.0f * float(ft.made) / float(ft.attempts);
    else
        ft.percent = 0;
}
free_throws &accumulate(free_throws &target, const free_throws &source)
{
    target.attempts += source.attempts;
    target.made += source.made;
    set_pc(target);
    return target;
}

const free_throws &accumulate2(free_throws &target, const free_throws &source)
{
    target.attempts += source.attempts;
    target.made += source.made;
    set_pc(target);
    return target;
}
```



