# E2-benchmarks
Benchmarks of performance of different E2's functions/logic/operators/methods/etc

> [!IMPORTANT]
> Tested on April 5, 2024 Workshop version (gmod x86-64 branch)
> 
> Each test (except some that needed more precision), are run 2.4m times. Each print out is an average of 800.000 code runs. (I've selected the most medium, no bias)
> 
> Obv the exact times of execution can differ VASTLY given different quota limits/hardware that it's run on/tick rate.
> 
> Don't take everything as 100% truth, as I may make mistakes, as well as anyone else, that's why I also added what functions I've used, for you to spot an error just in case.

> [!TIP]
> To find/jump between different benchmarks, use navigation button at the top left/right of this window.
![image](https://github.com/deltamolfar/E2-benchmarks/assets/72973198/72b05da7-5c9e-4872-b6c3-252ba05c98fa)
> If you want to run your benchmark (and potentially push them here), make sure to keep your tests to the tips that are on the top of E2 code.

### Global vs Local:
    Global(func1): 1.6932721248668e-06
    Local(func2): 1.5644746249666e-06
    Faster: Local
    Difference: 1.3029x faster

-- Keep in mind that this is in the testbencher, the Funcs are actually 5-deep nested, so that's why it may cause such difference.
```
Func1 = function(){
    G = 0
}
Func2 = function(){
    local L = 0
}
```
-------------------------------------------------------------------------------

### for(Table) vs foreach(Table):
    for(Table)(func1): 8.325243750096e-06
    foreach(Table)(func2): 8.9153580008929e-06
    Faster: for(Table)
    Difference: 1.0709x faster (To be considered equal, and difference is to be considered a fluctuation)
```
Func1 = function(){
    for(I=1, GlobalVar:count()){
        const A = GlobalVar[I, number]
    }
}
Func2 = function(){
    foreach(I:number, V:number = GlobalVar){
        const A = V
    }
}
GlobalVar = table(1=1, 2=2, 3=3, 4=4, 5=5, 6=6, 7=7, 8=8, 9=9, 10=10)
```
-------------------------------------------------------------------------------

### for(Array) vs foreach(Array):
    for(Array)(func1): 8.3095231255197e-06
    foreach(Array)(func2): 8.5949169993387e-06
    Faster: for(Array)
    Difference: 1.03825x faster (To be considered equal, and difference is to be considered a fluctuation)
```
Func1 = function(){
    for(I=1, GlobalVar:count()){
        const A = GlobalVar[I, number]
    }
}
Func2 = function(){
    foreach(I:number, V:number = GlobalVar){
        const A = V
    }
}
GlobalVar = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```
-------------------------------------------------------------------------------

### getting table value vs getting array value :
    getting table value(func1): 7.0829811247791e-06
    getting array value(func2): 7.0205573742464e-06
    Faster: getting array value
    Difference: 1.0279x faster (To be considered equal, and difference is to be considered a fluctuation)
```
Func1 = function(){
    for(_=1, 10){
        GlobalVarTable[5, number]
    }
}
Func2 = function(){
    for(_=1, 10){
        GlobalVarArray[5, number]
    }
}
GlobalVarArray = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
GlobalVarTable = table(1=1, 2=2, 3=3, 4=4, 5=5, 6=6, 7=7, 8=8, 9=9, 10=10)
```
-------------------------------------------------------------------------------

### appending table value vs appending array value :
    appending table value(func1): 1.7944658625383e-05
    appending array value(func2): 2.8403766375704e-05
    Faster: appending table value
    Difference: 1.58285x faster 
```
Func1 = function(){
    for(_=1, 10){
        GlobalVarTable:pushNumber(1024)
    }
}
Func2 = function(){
    for(_=1, 10){
        GlobalVarArray:pushNumber(1024)
    }
}
GlobalVarArray = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
GlobalVarTable = table(1=1, 2=2, 3=3, 4=4, 5=5, 6=6, 7=7, 8=8, 9=9, 10=10)
```
-------------------------------------------------------------------------------

### Table var caching(2 usages) vs No table var caching(2 usages):
    Table var caching(2 usages)(func1): 2.9246951252776e-06
    No table var caching(2 usages)(func2): 2.7979359994083e-06
    Faster: No table var caching(2 usages)
    Difference: 1.0452x faster (To be considered equal, and difference is to be considered a fluctuation)
```
Func1 = function(){
    const Num = GlobalVarTable[2, number]
    Num
    Num
}
Func2 = function(){
    GlobalVarTable[2, number]
    GlobalVarTable[2, number]
}
GlobalVarTable = table(1=1, 2=2, 3=3, 4=4, 5=5, 6=6, 7=7, 8=8, 9=9, 10=10)
```
-------------------------------------------------------------------------------

### Table var caching(4 usages) vs No table var caching(4 usages):
    Table var caching(4 usages)(func1): 2.3711792502468e-06
    No table var caching(4 usages)(func2): 2.5267207489082e-06
    Faster: Table var caching(4 usages)
    Difference: 1.19365x faster 
```
Func1 = function(){
    const Num = GlobalVarTable[2, number]
    Num
    Num
    Num
    Num
}
Func2 = function(){
    GlobalVarTable[2, number]
    GlobalVarTable[2, number]
    GlobalVarTable[2, number]
    GlobalVarTable[2, number]
}
GlobalVarTable = table(1=1, 2=2, 3=3, 4=4, 5=5, 6=6, 7=7, 8=8, 9=9, 10=10)
```
-------------------------------------------------------------------------------

### Multiplication vs Division:
    Multiplication(func1): 2.1088746375203e-05
    Division(func2): 2.1191265126743e-05
    Faster: Multiplication
    Difference: 1.0195x faster (To be considered equal, and difference is to be considered a fluctuation)
-- Heard a ton of arguments about that. I hope now that's settled :).
```
Func1 = function(){
    for(_=1, 50){
        3*0.5
    }
}
Func2 = function(){
    for(_=1, 50){
        3/0.5
    }
}
```
-------------------------------------------------------------------------------

### Logical AND vs Bitwise AND :
    Logical AND(func1): 9.3450682506909e-06
    Bitwise AND(func2): 1.2188277500979e-05
    Faster: Logical AND
    Difference: 1.3046x faster 

-- And I've always used Bitwise ;c

```
Func1 = function(){
    for(_=1, 25){
        5 & 5
    }
}
Func2 = function(){
    for(_=1, 25){
        5 && 5
    }
}
```
-------------------------------------------------------------------------------

### Logical OR vs Bitwise OR:
    Logical OR(func1): 3.0377830009661e-06
    Bitwise OR(func2): 5.0107538763245e-06
    Faster: Logical OR
    Difference: 1.36075x faster 

```
Func1 = function(){
    for(_=1, 25){
        5 | 5
    }
}
Func2 = function(){
    for(_=1, 25){
        5 || 5
    }
}
```
-------------------------------------------------------------------------------

### if(A) vs if(A==1) :
    if(A)(func1): 3.118137437486e-05
    if(A==1)(func2): 3.0595491000365e-05
    Faster: if(A==1)
    Difference: 1.0344x faster (To be considered equal, and difference is to be considered a fluctuation)

```
Func1 = function(){
    const A = 1
    for(_=1, 25){
        if( A ){  }
    }
}
Func2 = function(){
    const A = 1
    for(_=1, 25){
        if( A==1 ){  }
    }
}
```
-------------------------------------------------------------------------------

### v:setX() vs use math to set vector's x:
    v:setX()(func1): 4.3080405023502e-06
    use math to set pitch(func2): 6.5721699985588e-06
    Faster: v:setX()
    Difference: 1.5255x faster 
### v:setX() vs use math:
    v:setX()(func1): 2.6215744249907e-05
    use math(func2): 5.9519711250114e-05
    Faster: v:setX()
    Difference: 2.2704x faster 

```
Func1 = function(){
    for(_=1, 10){
        vec(50):setX(100)
    }
}
Func2 = function(){
    for(_=1, 10){
        vec(50)*vec(0,1,1)+vec(100,0,0)
    }
}
```
-------------------------------------------------------------------------------

### for(I=1, 50) vs for(_=1, 50) :
    for(I=1, 50)(func1): 9.6537351255711e-06
    for(_=1, 50)(func2): 9.1208142506343e-06
    Faster: for(_=1, 10)
    Difference: 1.05895x faster (To be considered equal, and difference is to be considered a fluctuation)

```
Func1 = function(){
    for(I=1, 50){ }
}
Func2 = function(){
    for(_=1, 50){ }
}
```
-------------------------------------------------------------------------------

### foreach(I:number, V:number = Array) vs foreach(_:number, V:number = Array) :
    foreach(I:number, V:number = Array)(func1): 6.2692422495638e-06
    foreach(_:number, V:number = Array)(func2): 5.7181398756438e-06
    Faster: foreach(_:number, V:number = Array)
    Difference: 1.0987x faster (To be considered equal, and difference is to be considered a fluctuation)

```
Func1 = function(){
    foreach(I:number, V:number = GlobalVarArray){ }
}
Func2 = function(){
    foreach(_:number, V:number = GlobalVarArray){ }
}
GlobalVarArray = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```
-------------------------------------------------------------------------------

### a:average() vs manual average number:
    a:average()(func1): 1.6577344026664e-06
    manual average number(func2): 1.1374525199102e-05
    Faster: a:average()
    Difference: 6.8614x faster

-- Idk what I've expected
```
Func1 = function(){
    GlobalVar:average()
}
Func2 = function(){
    const Count = GlobalVar:count()
    local Sum = 0
    for(I=1, Count){
        Sum+=GlobalVar[I, number]
    }
    Sum/Count
}
```
-------------------------------------------------------------------------------

### Checking for variable using: lookup table vs a:indexOf - (best case):
    lookup table(func1): 1.8567504999089e-05
    a:indexOf(func2): 4.6639391243048e-06
    Faster: a:indexOf
    Difference: 3.988x faster 

```
Func1 = function(){
    for(_=1, 2){
        invert(GlobalVarArray):exists(1)
    }
}
Func2 = function(){
    for(_=1, 2){
        GlobalVarArray:indexOf(1)!=0
    }
}
GlobalVarArray = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```
-------------------------------------------------------------------------------

### Checking for variable using: lookup table vs a:indexOf - (worst case):
    lookup table(func1): 1.8696632376898e-05
    a:indexOf(func2): 4.903197749818e-06
    Faster: a:indexOf
    Difference: 3.8201x faster 

```
Func1 = function(){
    for(_=1, 2){
        invert(GlobalVarArray):exists(10)
    }
}
Func2 = function(){
    for(_=1, 2){
        GlobalVarArray:indexOf(10)!=0
    }
}
GlobalVarArray = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```
### Checking for variable using: Table:exists(n) vs Array:indexOf(n)!=0 - :
    Table:exists(n)(func1): 4.472356374481e-06
    Array:indexOf(n)!=0(func2): 5.1354679974429e-06
    Faster: Table:exists(n)
    Difference: 1.1519x faster 

-- As expected, the larger the array and table, the better the table:exists() then :indexOf()!=0. With <25 entries, the difference is close to 0   
```
Func1 = function(){
    for(_=1, 2){
        GlobalVarTable:exists(25)
    }
}
Func2 = function(){
    for(_=1, 2){
        GlobalVarArray:indexOf(25)!=0
    }
}
GlobalVarArray = array(50 entries)
GlobalVarTable = table(50 entries)
```
-------------------------------------------------------------------------------

### elseif chain vs switch case - best case (first case is true):
    elseif chain(func1): 9.2845717495857e-06
    switch case(func2): 1.0719321622723e-05
    Faster: elseif chain
    Difference: 1.1658x faster 

-- I love switchcases ;c
```
Func1 = function(){
    const A = 5
    
    for(_=1, 5){
        if( A==5 ){
        } elseif( A==4 ){
        } elseif( A==3 ){
        } elseif( A==2 ){
        } elseif( A==1 ){
        }
    }
}
Func2 = function(){
    const A = 5
    
    for(_=1, 5){
        switch( A ){
            case 5,
            break
            case 4,
            break
            case 3,
            break
            case 2,
            break
            case 1,
            break
        }
    }
}
```
-------------------------------------------------------------------------------

### elseif chain vs switch case - worst case (last case is true):
    elseif chain(func1): 1.0976936749835e-05
    switch case(func2): 1.007760012571e-05
    Faster: switch case
    Difference: 1.119x faster 

-- I love switchcases ;c
```
Func1 = function(){
    const A = 1
    
    for(_=1, 5){
        if( A==5 ){
        } elseif( A==4 ){
        } elseif( A==3 ){
        } elseif( A==2 ){
        } elseif( A==1 ){
        }
    }
}
Func2 = function(){
    const A = 1
    
    for(_=1, 5){
        switch( A ){
            case 5,
            break
            case 4,
            break
            case 3,
            break
            case 2,
            break
            case 1,
            break
        }
    }
}
```
-------------------------------------------------------------------------------

### x^(1/2) vs sqrt(x):
    x^(1/2)(func1): 1.6167051252592e-05
    sqrt(x)(func2): 1.9966790247245e-05
    Faster: x^(1/2)
    Difference: 1.2353x faster 

```
Func1 = function(){
    for(_=1, 25){
        128^(1/2)
    }
}
Func2 = function(){
    for(_=1, 25){
        sqrt(128)
    }
}
```
-------------------------------------------------------------------------------

### v:distance2(v) vs v:distance(v):
    v:distance2(v)(func1): 2.6137698248644e-05
    v:distance(v)(func2): 2.6477860375626e-05
    Faster: v:distance2(v)
    Difference: 1.013x faster (To be considered equal, and difference is to be considered a fluctuation)

```
Func1 = function(){
    for(_=1, 25){
       Vector1:distance2(Vector2)
    }
}
Func2 = function(){
    for(_=1, 25){
       Vector1:distance(Vector2)
    }


}
```
-------------------------------------------------------------------------------

### Ternary vs if else:
    Ternary operator(func1): 1.1917865250975e-05
    if else(func2): 3.2560750373318e-05
    Faster: Ternary operator
    Difference: 2.7318x faster 

-- It's even a bit faster (~3x) if the first condition is true (replace '0' with '1')
```
Func1 = function(){
    for(_=1, 25){
       0 ? "true" : "false"
    }
}
Func2 = function(){
    for(_=1, 25){
        if( 0 ){
            "true"
        } else{
            "false"
        }
    }
}
```
-------------------------------------------------------------------------------

### t:pushNumber(n) vs t[t:count()+1, number]=n:
    t:pushNumber(n)(func1): 8.371339249743e-06
    t[t:count()+1, number]=n(func2): 1.2604062499104e-05
    Faster: t:pushNumber(n)
    Difference: 1.5058x faster 

```
Func1 = function(){
    for(_=1, 10){
       GlobalVarTable:pushNumber(0)
    }
}
Func2 = function(){
    for(_=1, 10){
        GlobalVarTable2[GlobalVarTable2:count()+1, number] = 0
    }
}
```
-------------------------------------------------------------------------------

### t:exists(n) vs t[n, number]!=0:
    t:exists(n)(func1): 1.0172602498865e-05
    t[n, number]!=0(func2): 8.8596381252137e-06
    Faster: t[n, number]!=0
    Difference: 1.148x faster 
```
Func1 = function(){
    for(_=1, 10){
       GlobalVarTable:exists(5)
    }
}
Func2 = function(){
    for(_=1, 10){
        GlobalVarTable[5, number]!=0
    }
}
```
-------------------------------------------------------------------------------

### t:clear() vs t = table() - small amount (1):
    t:clear()(func1): 1.6646853251696e-05
    t = table()(func2): 2.2625943747707e-05
    Faster: t:clear()
    Difference: 1.35935x faster 
```
Func1 = function(){
    local T = table("hi" = ":)")
    for(_=1, 10){
        T:clear()# You CAN use that on const
    }
}
Func2 = function(){
    local T = table("hi" = ":)")
    for(_=1, 10){
        T = table()# You CAN'T use that on const
    }
}
```
-------------------------------------------------------------------------------

### t:clear() vs t = table() - big amount (10000):
    t:clear()(func1): 0.00030535707874442
    t = table()(func2): 0.00027059088250885
    Faster: t = table()
    Difference: 1.12845x faster 
```
Func1 = function(){
    local T = GlobalVarTable:clone()

    T:clear()# You CAN use that on const
}
Func2 = function(){
    local T = GlobalVarTable:clone()

    T = table()# You CAN'T use that on const
}
GlobalVarTable:count==10000
```
-------------------------------------------------------------------------------

### Assigning table variables: Directly vs At Index - big amount:
    Directly(func1): 3.6574496998078e-05
    At Index(func2): 6.338529037329e-05
    Faster: Directly
    Difference: 1.73305x faster 
```
Func1 = function(){
    for(_=1, 10){
        const T = table(
            "VAR_1" = "HELLO",
            "VAR_2" = "WORLD",
            "VAR_3" = ":D",
            4 = 1,
            5 = 2,
            6 = 3
        )
    }
}
Func2 = function(){
    for(_=1, 10){
        const T = table()
            T["VAR_1", string] = "HELLO"
            T["VAR_2", string] = "WORLD"
            T["VAR_3", string] = ":D"
            T[4, number]= 1
            T[5, number] = 2
            T[6, number] = 3
    }
}
```

