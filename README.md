
# Functions

## Functions

* Think of functions as something that take input and throws output
* Functions are based on `traits`: `Function1`, `Function2`, …​, `Function22`

## Functions the hard way

Given: An anonymous instantiation


```scala
val f1:Function1[Int, Int] = new Function1[Int, Int] {
   def apply(x:Int) = x + 1
}
```


    Intitializing Scala interpreter ...



    Spark Web UI available at http://32c6e00c8fb3:4044
    SparkContext available as 'sc' (version = 2.4.3, master = local[*], app id = local-1562180768731)
    SparkSession available as 'spark'
    





    f1: Int => Int = <function1>
    




```scala
println(f1.apply(4)) //5
```

    5
    

But since, the name of the method is `apply`…​.


```scala
println(f1(4)) //5
```

    5
    

## Different Signatures of Function (The long way)

Here are some functions declared the long way

We explicitly declare the types on the left hand side Function1, Function0

We are instantiating the traits


```scala
val f1:Function1[Int, Int] = new Function1[Int, Int] {
     def apply(x:Int) = x + 1
}
```




    f1: Int => Int = <function1>
    




```scala
val f0:Function0[Int] = new Function0[Int] {
     def apply() = 1
}
```




    f0: () => Int = <function0>
    




```scala
val f2:Function2[Int, String, String] = new Function2[Int, String, String] {
     def apply(x:Int, y:String) = y + x
}
```




    f2: (Int, String) => String = <function2>
    



## Signatures of Function (The medium way)

* Here are some functions declared the medium way
* We explicitly declare the types on the left hand side as:
* `Int ⇒ Int` to represent `Function1[Int, Int]`
* `() ⇒ Int` to represent `Function0[Int]`
* `(Int, String) ⇒ String` to represent `Function2[Int, String, String]`

Of course these functions are still anonymous instantiating of traits


```scala
val f1:(Int => Int) = new Function1[Int, Int] {
     def apply(x:Int) = x + 1
}
```




    f1: Int => Int = <function1>
    




```scala
val f0:() => Int = new Function0[Int] {
     def apply() = 1
}
```




    f0: () => Int = <function0>
    




```scala
val f2:(Int, String) => String = new Function2[Int, String, String] {
     def apply(x:Int, y:String) = y + x
}
```




    f2: (Int, String) => String = <function2>
    



## Trimming down the functions

Give the functions from the previous page, we can clean up the functions

The left hand side uses a short hand notation to declare the types as before

The right hand side uses a short hand notation to create the function!


```scala
val f1:Int => Int = (x:Int) => x + 1
```




    f1: Int => Int = <function1>
    




```scala
val f0:() => Int = () => 1
```




    f0: () => Int = <function0>
    




```scala
val f2:(Int, String) => String = (x:Int, y:String) => y + x
```




    f2: (Int, String) => String = <function2>
    



## Making it concise: Using type inference with functions

* Since there is heavy type inference in Scala, we can trim everything down
* The left hand side can be left with little or no type annotation since the right hand side is declaring most of the information


```scala
val f1 = (x:Int) => x + 1
```




    f1: Int => Int = <function1>
    




```scala
val f0 = () => 1
```




    f0: () => Int = <function0>
    




```scala
val f2 = (x:Int, y:String) => y + x
```




    f2: (Int, String) => String = <function2>
    



## Choosing the type inference, left hand side or right hand side

Using right hand side verbosity, and left hand side type inference


```scala
val f1 = (x:Int) => x + 1
```




    f1: Int => Int = <function1>
    




```scala
val f0 = () => 1
```




    f0: () => Int = <function0>
    




```scala
val f2 = (x:Int, y:String) => y + x
```




    f2: (Int, String) => String = <function2>
    



Using right hand side inferred, and left hand side verbosely declared


```scala
val f1:Int => Int = x => x + 1
```




    f1: Int => Int = <function1>
    




```scala
val f0:() => Int = () => 1
```




    f0: () => Int = <function0>
    




```scala
val f2:(Int, String) => String = (x,y) => y + x
```




    f2: (Int, String) => String = <function2>
    



## Returning more than one result

* Every function has one return value so how do we return multiple items?
* Use a Tuple!


```scala
val f3 = (x:String) => (x, x.size)  // Right hand side declaration
println(f3("Laser"))                // ("Laser", 5)
```

    (Laser,5)
    




    f3: String => (String, Int) = <function1>
    



## Trimming the result with `_`

* Given the function where the left hand side is declared with the type
* We can provide a shortcut using the `_` that represents one of the arguments
* Seasoned Scala developers will recognize this shortcut
* Be aware that this may be confusing for novices

Given:


```scala
val f5: Int => Int = x => x + 1
```




    f5: Int => Int = <function1>
    



Can be converted to:


```scala
val f5: Int => Int = _ + 1
```




    f5: Int => Int = <function1>
    



Works the same when invoking the function


```scala
f5(40)  //41
```




    res0: Int = 41
    



## Advanced Feature: Postfix Operators

* If the argument on the right most position is a `_`, it can be omitted
* This may require that an import feature may need to be turned on

Given:


```scala
val f5: Int => Int = x => x + 1
```




    f5: Int => Int = <function1>
    



Can be converted to:


```scala
val f5: Int => Int = _ + 1
```




    f5: Int => Int = <function1>
    



Since addition is commutative, it can be rearranged jj


```scala
val f5: Int => Int = 1 + _
```




    f5: Int => Int = <function1>
    



Since, the _ is at the right most position it can be dropped


```scala
val f5: Int => Int = 1+
```




    warning: there was one feature warning; re-run with -feature for details
    f5: Int => Int = <function1>
    



Works the same when invoking the function


```scala
f5(40)  //41
```




    res4: Int = 41
    



## Advanced Feature: Postfix Operators, Clearing Warnings

If you attempted to clear out the most _ on the right hand side you may have received this warning:

```
warning: postfix operator + should be enabled
by making the implicit value scala.language.postfixOps visible.
This can be achieved by adding the import clause 'import scala.language.postfixOps'
or by setting the compiler option -language:postfixOps.
See the Scaladoc for value scala.language.postfixOps for a discussion
why the feature should be explicitly enabled.
```

This means that to avoid the warning you can turn that feature on by an import statement


```scala
import scala.language.postfixOps
val f5: Int => Int = 1+
```




    import scala.language.postfixOps
    f5: Int => Int = <function1>
    



Works the same when invoking the function, and you get no warnings


```scala
f5(40)  //41
```




    res5: Int = 41
    



## Trimming the result with multiple _

* If a function has two arguments, that too can use the _ shortcut
* The only thing is that `_` can only be applied to one argument

Here is the before:


```scala
val sum: (Int, Int) => Int = (x, y) => x + y
```




    sum: (Int, Int) => Int = <function2>
    



Here is the after:


```scala
val sum: (Int, Int) => Int = _ + _
```




    sum: (Int, Int) => Int = <function2>
    



Note in the above:

* The first `_` represents the first argument, or `x` in the above example
* The second `_` represents the second argument, or `y` in the above example

## Functions Conclusion

* Functions are traits that we instantiate anonymously.
* The `apply` method in the function means that you don’t have to call `apply` explicitly.
* While you can only return one item, that one item can be a collection or a tuple.
* Shortcuts to the argument can be made with `_`
* The `_` represents each argument being applied to the function
