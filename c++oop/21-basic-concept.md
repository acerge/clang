## 1.1 Class members are private by default

```cpp
class Distance
{
    int iFeet; //private by default
    float fInches; //private by default
    ...
    public:
        void setFeet(int x)
        {
            iFeet=x;
        }
}
```

## 1.2 Member function

### 1.2.1 Overloaded Member Functions

### 1.2.3 Inline Member Functions

### 1.2.4 Constant Member Functions

```
void show(int=1);    //Default Values for Formal Arguments of Member Functions
int getFeet() const; //should not be able to change the value of member data
```

### 1.2.5 Mutable Data Members

```
/*Beginning of mutable.h*/
class A
{
    int x; //non-mutable data member
    mutable int y; //mutable data member
public:
    void abc() const //a constant member function
    {
        x++; //ERROR: cannot modify a non-constant data member in a constant member function
        y++; //OK: can modify a mutable data member in a constant member function
}
```

### 1.2.6 Friend non-member functions

_**A friend function is a non-member function that has special rights to access private data  
 members of any object of the class of whom it is a friend. **_

```
/*Beginning of friend.cpp*/
class A
{
    int x;
public:
    friend void abc(A&); //prototype of the friend function
};
void abc(A& AObj) //definition of the friend function
{
    AObj.x++; //accessing private members of the object
}
void main()
{
    A A1;
    abc(A1);
}
/*End of friend.cpp
```

### 1.2.7 static member

![](/assets/staticmember.png)



