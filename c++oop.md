## 1、Class Constructor

```cpp
explicit Stonewt(double lbs); // no implicit conversions allowed

Stonewt myCat; // create a Stonewt object
myCat = 19.6; // not valid if Stonewt(double) is declared as explicit
mycat = Stonewt(19.6); // ok, an explicit conversion
mycat = (Stonewt) 19.6; // ok, old form for explicit typecast
```

## ![](/assets/class_constructor.png)

## 2、this Pointer

![](/assets/this_pointer.png)







## 



