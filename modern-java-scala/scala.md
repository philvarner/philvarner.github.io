# Scala Cheatsheet

val - immutable
var - 
def - methods

val and var can be used to define anonymous functions

symbols! 'zed (replaces uses of enums and public static finals)

def someMethod[ATypeParameter](x: ATypeParameter):List[ATypeParameter] = List(x, x, x)




Smart String

```scala
/// margins for multi-line strings
"""Foo
   @Bar
   @Baz""".stripMargin('@') // pipe by default
   
// create a regex from a string   
println("""(\s*"\s+"\s+)""".r.findAllIn(""" "  " """).toList)
// List( "  " )

"Format this %s string".format("awesome")
"Format this %1$s string".format("awesome")
```

Interpolators
```scala
//s interpolator
val count = 5
s"${count + 1} things"

//f interpolator
val stringVar = "owl"
val cost = 12.25
f"An $stringVar%s costs $$$cost%1.2f"
// res11: String = An owl costs $12.25
```

Pattern matching

def matchingMethod(n: Int) = n match {
   case 1 => "matches"
   case _ => "nope"
}

_ almost always means "fill it in with whatever"
=> "fat arrow" -- pointing to a block of executable code
