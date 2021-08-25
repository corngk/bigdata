# Scala
## Install Scala on Windows 10 WDSL 2 Ubuntu 20.4

## Check Java
Open the terminal and check if java is installed

    java -version

    openjdk version "1.8.0_292"
    OpenJDK Runtime Environment (build 1.8.0_292-8u292-b10-0ubuntu1~20.04-b10)
    OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)

Update all the package

    sudo apt-get update

Install Java JDK if not installed

    sudo apt-get install default-JDK

Install Scala

    sudo apt-get install scala

    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following additional packages will be installed:
    libhawtjni-runtime-java libjansi-java libjansi-native-java libjline2-java scala-library scala-parser-combinators scala-xml
    Suggested packages:
    scala-doc
    The following NEW packages will be installed:
    libhawtjni-runtime-java libjansi-java libjansi-native-java libjline2-java scala scala-library scala-parser-combinators scala-xml
    0 upgraded, 8 newly installed, 0 to remove and 118 not upgraded.
    Need to get 25.0 MB of archives.
    After this operation, 28.7 MB of additional disk space will be used.
    Do you want to continue? [Y/n] Y

Check installation and run Scala to start Scala REPL

    scala

    Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_292).
    Type in expressions for evaluation. Or try :help.

    scala>


## Some Examples
Open the terminal and type scala to run Scala REPL.

### Declare Basic Literals
    scala> var big= 1.2345
    big: Double = 1.2345

    scala> var little = 1.234F
    little: Float = 1.234

    scala> var a = 'A'
    a: Char = A

    scala> var hello = "Hello"
    hello: String = Hello

### Print a string in the console
    scala> println("""Welcome to Scala""")
    Welcome to Scala

### Declare the Boolean literal and use different types of arithmatic operators
    scala> var bool=true
    bool: Boolean = true

    scala> var fool=false
    fool: Boolean = false

    scala> 1.2+2.3
    res2: Double = 3.5

    scala> 1.2*2.3
    res3: Double = 2.76

    scala> 1.2/0.2
    res4: Double = 5.999999999999999

    scala> 3-1
    res5: Int = 2

    scala> 2L*3L
    res6: Long = 6

    scala> 11/4
    res7: Int = 2

    scala> 11.0/4/0
    res8: Double = Infinity

    scala> 11.0/4.0
    res9: Double = 2.75

### Logical Operators
    scala> 1 & 2
    res11: Int = 0

    scala> 2+3 == 5
    res12: Boolean = true

    scala> 5 > 3
    res13: Boolean = true

    scala> 5 < 3
    res14: Boolean = false

    scala> 2 + 2.5 == 4.5
    res15: Boolean = true

    scala> 1 + 3 < 10
    res16: Boolean = true

    scala> 10 - 5 == 5
    res17: Boolean = true

### Type Inference Functions Anonymous Function and Scale
Open the terminal and type **scala** to start the Scala REPL.

    Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_292).
    Type in expressions for evaluation. Or try :help.

    scala> object InferenceTest1 extends App {
        | val x = 1 + 2 * 3
        | val y = x.toString()
        | def succ(x:Int) = x + 1;
        | }
    defined object InferenceTest1

    scala> def sumInts(a:Int, b:Int): Int = {
        | if( a> b ) 0
        | else
        | a + sumInts(a+1, b)
        | }
    sumInts: (a: Int, b: Int)Int

    scala> (y:Int) => y*y;
    res1: Int => Int = <function1>

    scala> class ScalaClass1(a:Int, b:Int) {
        | var x: Int = a;
        | var y: Int = b;
        | def addition(c:Int, d:Int) {
        | x=x+1;
        | y=y+1;
        | println("X output::"+x);
        | println("Y output::"+y);
        | }
        | }
    defined class ScalaClass1

### Collections
Open the terminal and type **scala** to start the Scala REPL.

    Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_292).
    Type in expressions for evaluation. Or try :help.

    scala> var x = List(1, 2, 3, 4)
    x: List[Int] = List(1, 2, 3, 4)

    scala> var x = Set(1, 3, 3, 4, 5, 5)
    x: scala.collection.immutable.Set[Int] = Set(1, 3, 4, 5)

    scala> val x = Map("One" -> 1, "Two" -> 2, "Three" - > 3)
    <console>:1: error: ')' expected but integer literal found.
    val x = Map("One" -> 1, "Two" -> 2, "Three" - > 3)
                                                    ^

    scala> val x = Map("One" -> 1, "Two" -> 2, "Three" -> 3)
    x: scala.collection.immutable.Map[String,Int] = Map(One -> 1, Two -> 2, Three -> 3)

    scala> val x = (10, "Scala")
    x: (Int, String) = (10,Scala)

    scala> object Test {
     | def main(args: Array[String]) {
     | val it = Iterator("a", "b", "c", "d")
     | while(it.hasNext) {
     | println(it.next())
     | }
     | }
     | }
    defined object Test


### Operations on List
Open the terminal and type **scala** to start the Scala REPL.

    Welcome to Scala 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_292).
    Type in expressions for evaluation. Or try :help.

    scala> object Test {
        | def main(args: Array[String]) {
        | val processing = "MapReduce"::("Hive"::("Spark"::Nil))
        | val query = Nil
        | println("Head of processing:"+processing.head)
        | println("Some list of processing:"+processing.tail)
        | println("Check if processing is empty:"+processing.isEmpty)
        | println("Check if query is empty:"+query.isEmpty)
        | }
        | }
    defined object Test

