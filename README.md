# E2-benchmarks
Benchmarks of E2 functions/logic

Each test (except some), are run 2-3 times. Each print out is from one of 1.000.000 runs.

Name: Global vars vs Local vars:
    Global (func1): 1.1187495012491e-06
    Local (func2): 3.5490606999374e-06
    Faster: Global
    Difference: 3.1723x faster

-- From ton of tests, sometimes local is even faster, but most of the time global is ~3 times faster.
```
Func1 = function(){
    A = 0
}
Func2 = function(){
    local A = 0
}
```
-------------------------------------------------------------------------------

Name: for(table) vs foreach(table):
    for(Table)(func1): 8.7233780999195e-06
    foreach(Table)(func2): 9.8770803049119e-06
    Faster: for(Table)
    Difference: 1.1322x faster

```
Func1 = function(){
    const Table = GlobalVar
    
    for(I=1, Table:count()){
        A = Table[I, number]
    }
}
Func2 = function(){
    const Table = GlobalVar
    
    foreach(I:number, V:number = Table){
        A = V
    }
}
```
-------------------------------------------------------------------------------

Name: for(array) vs foreach(array):
    for(Array)(func1): 9.4104822004447e-06
    foreach(Array)(func2): 1.0234021101252e-05
    Faster: for(Array)
    Difference: 1.0875x faster

```
Func1 = function(){
    const Array = GlobalVar
    
    for(I=1, Array:count()){
        A = Array[I, number]
    }
}
Func2 = function(){
    const Array = GlobalVar
    
    foreach(I:number, V:number = Array){
        A = V
    }
}
```
-------------------------------------------------------------------------------

Name: Caching table value vs not caching table value (2 usages):
    Caching(func1): 3.1641313986402e-06
    Not caching(func2): 3.0177291972941e-06
    Faster: Not caching
    Difference: 1.0485x faster (To be considered equal, and difference is to be considered a fluctuation)

```
Func1 = function(){
    const Table = GlobalVar
    
    const Num = Table[2, number]
    const A = Num
    const B = Num
}
Func2 = function(){
    const Table = GlobalVar
    
    const A = Table[2, number]
    const B = Table[2, number]
}
```
-------------------------------------------------------------------------------

Name: Caching table value vs not caching table value (4 usages):
    Caching(func1): 3.3019128994383e-06
    Not caching(func2): 3.9023352007534e-06
    Faster: Caching
    Difference: 1.1818x faster

```
Func1 = function(){
    const Table = GlobalVar
    
    const Num = Table[2, number]
    const A = Num
    const B = Num
    const C = Num
    const D = Num
}
Func2 = function(){
    const Table = GlobalVar
    
    const A = Table[2, number]
    const B = Table[2, number]
    const C = Table[2, number]
    const D = Table[2, number]
}
```
-------------------------------------------------------------------------------

Name: Multiplication vs Division:
    Multiplication(func1): 1.6986207035443e-06
    Division(func2): 2.3158794003539e-06
    Faster: Multiplication
    Difference: 1.3633x faster

-- That's actually suprised me, as from benchmarks of GLua that I saw, it was discovered that Multiplication being faster then Division was a myth,
-- But I guess E2 likes keeping up to myths :)
```
Func1 = function(){
    A = 8*0.5
}
Func2 = function(){
    A = 8/2
}
```
-------------------------------------------------------------------------------

Name: Logical AND vs Bitwise AND:
    Logical AND(func1): 2.7615460006491e-06
    Bitwise AND(func2): 1.4855218013945e-06
    Faster: Bitwise AND
    Difference: 1.8589x faster 

-- Ha-ha, I knew it :). (To those that always told me to use Logical AND - :P)
```
Func1 = function(){
    A = 2
    if( A & 1 ){
        B = 0
    }
}
Func2 = function(){
    A = 2
    if( A && 1 ){
        B = 0
    }
}
```
-------------------------------------------------------------------------------

Name: Logical OR vs Bitwise OR:
    Logical OR(func1): 3.2567436998688e-06
    Bitwise OR(func2): 2.3873028004273e-06
    Faster: Bitwise OR
    Difference: 1.3641x faster 

```
Func1 = function(){
    const A = 1
    if( A | 1 ){
        const B = 0
    }
}
Func2 = function(){
    const A = 1
    if( A || 1 ){
        const B = 0
    }
}
```
-------------------------------------------------------------------------------

Name: if(A) vs if(A==1):
    if(A)(func1): 3.1880695985856e-06
    if(A==1)(func2): 1.9789566011314e-06
    Faster: if(A==1)
    Difference: 1.6109x faster 

-- And that's a shame :)
```
Func1 = function(){
    A = 1
    if(A){
        B = 0
    }
}
Func2 = function(){
    A = 1
    if( A==1 ){
        B = 0
    }
}
```
-------------------------------------------------------------------------------

Name: v:setX() vs use math to set pitch:
    v:setX()(func1): 4.3080405023502e-06
    use math to set pitch(func2): 6.5721699985588e-06
    Faster: v:setX()
    Difference: 1.5255x faster 

```
Func1 = function(){
    local A = vec(50)
    A:setX(100)
}
Func2 = function(){
    local A = vec(50)
    A = A*vec(0,1,1)+vec(100,0,0)
}
```
-------------------------------------------------------------------------------

Name: for(I=1, 10) vs for(_=1, 10):
    for(I=1, 10)(func1): 5.7966269985372e-06
    for(_=1, 10)(func2): 4.1644744037403e-06
    Faster: for(_=1, 10)
    Difference: 1.3919x faster 

```
Func1 = function(){
    for(I=1, 10){
        const A = 0
    }
}
Func2 = function(){
    for(_=1, 10){
        const A = 0
    }
}
```
-------------------------------------------------------------------------------

Name: foreach(I:number, V:number = Array) vs foreach(_:number, V:number = Array):
    foreach(I:number, V:number = Array)(func1): 7.6134745981981e-06
    foreach(_:number, V:number = Array)(func2): 7.3428557003244e-06
    Faster: foreach(_:number, V:number = Array)
    Difference: 1.0368x faster (To be considered equal, and difference is to be considered a fluctuation)

```
 Func1 = function(){
    foreach(I:number, V:number = GlobalVar){
        const A = 0
    }
}
Func2 = function(){
    foreach(_:number, V:number = GlobalVar){
        const A = 0
    }
}
```
-------------------------------------------------------------------------------

Name: a:average() vs manual average number:
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

Name: Checking for value with lookup table vs Checking for value with a:indexOf (best case):
    Checking for value with lookup table(func1): 9.4220373995668e-06
    Checking for value with a:indexOf (best case)(func2): 4.2026332019668e-06
    Faster: Checking for value with a:indexOf (best case)
    Difference: 2.2419x faster

```
Func1 = function(){
    if( invert(GlobalVar):exists(1)!=0 ){
        const B = 0
    }
}
Func2 = function(){
    if( GlobalVar:indexOf(1)!=0 ){
        const B = 0
    }
}
GlobalVar = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```
-------------------------------------------------------------------------------

Name: Checking for value with lookup table vs Checking for value with a:indexOf (worst case):
    Checking for value with lookup table(func1): 9.5196129990763e-06
    Checking for value with a:indexOf (worst case)(func2): 4.0209813018155e-06
    Faster: Checking for value with a:indexOf (worst case)
    Difference: 2.3674x faster 

```
Func1 = function(){
    if( invert(GlobalVar):exists(10)!=0 ){
        const B = 0
    }
}
Func2 = function(){
    if( GlobalVar:indexOf(10)!=0 ){
        const B = 0
    }
}
GlobalVar = array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```