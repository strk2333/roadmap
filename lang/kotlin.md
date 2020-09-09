# Kotlin

## Basic Syntax

\##### 包规范放在文件顶部

\##### 方法

​    **fun** sum(a: Int, b: Int): Int? { **return** a + b}

  **fun** sum(a: Int, b: Int) = a + b

  **fun** printSum(a: Int, b: Int): Unit { print(a + b)} == **fun** printSum(a: Int, b: Int) { print(a + b)}

\##### 局部变量

 Assign-once read-only local variable

   val a: Int = 1

   val b = 1   // 'Int' type is inferred

   val c: Int

   c = 1

 mutable variable

   var x = 5

   x += 1

\##### 注释

   //

   /*  */

\##### String模板

​    **fun** main(args: Array<String>) { **if** (args.size == 0) **return** print("First argument: ${args[0]}")}

  val s = "abc"val str = "$s.length is ${s.length}"

  val price = """${'$'}9.99"""

\##### Control Flow

   if

​     (str is String)

​       **fun** max(a: Int, b: Int) = **if** (a > b) a **else** b

  when

   when (x) {

​      1 -> print("x == 1")

​      2 -> print("x == 2")

​     0, 1 -> print("x == 0 or x == 1") 

​     parseInt(s) -> print("s encodes x")

​     in 1..10 -> print("x is in the range")

​      in validNumbers -> print("x is valid")

​      !in 10..20 -> print("x is outside the range")

​     is String -> x.startsWith("prefix")

​     x.isOdd() -> print("x is odd")

​      x.isEven() -> print("x is even")

​     else -> { // Note the block

​        print("x is neither 1 nor 2")

​      }

   }

  for

   for (item in collection | i in array.indices | (index, value) in array.withIndex()) {

​      print(item)

​     print(array[i])

​     println("the element at $index is $value")

   }

   

   for ((k, v) in map) {

​      print("$k -> $v")

   }

  while

   while (x > 0) {

​      x--

   }

   do {

​      val y = retrieveData()

   } while (y != null) // y is visible here!

\##### Range

   i in 1..10

   i in 10 downTo 1 step 2

   rangeTo()

   downTo()

   reversed()

   step()

\### Idoims

\##### DTOs数据类

   data class Customer(val name: String,val email: String)

   

\##### others

   val map = mapOf("a" to 1, "b" to 2, "c" to 3)

  懒加载

   val p: String by lazy {

   }

  扩展函数

   fun String.spcaceToCamelCase() { ... }

   "Convert this to camelcase".spcaceToCamelCase()

  单例

   object Resource {

​      val name = "Name"

   }

   如果为空否则...的简写

   val files = File("Test").listFiles()

   println(files?.size)

   如果为空...否则...的简写

   val files = File("test").listFiles()

   println(files?.size ?: "empty")

   如果声明为空执行某操作

   val data = ...

   val email = data["email"] ?: throw IllegalStateException("Email

   is missing!")

   如果不为空执行某操作

   val date = ...

   data?.let{

​      ...//如果不为空执行该语句块

   }

   返回 when 判断

   fun transform(color: String): Int {

​      return when(color) {

​        "Red" -> 0

​        "Green" -> 1

​        "Blue" -> 2

​        else -> throw IllegalArgumentException("Invalid color pa

ram value")

​      }

   }

   // fun transform(color: String): Int = when (color) {

   try-catch 表达式

   fun test() {

​      val result = try {

​        count()

​      }catch (e: ArithmeticException) {

​        throw IllegaStateException(e)

​      }

​      //处理 result

   }

   if 表达式

   fun foo(param: Int){

​      val result = if (param == 1) {

​        "one"

​      } else if (param == 2) {

​        "two"

​      } else {

​        "three"

​      }

   }

   方法使用生成器模式返回 Unit

   fun arrOfMinusOnes(size: Int): IntArray{

​      return IntArray(size).apply{ fill(-1) }

   }

   只有一个表达式的函数

   fun theAnswer() = 42

   与下面的语句是等效的

   fun theAnswer(): Int {

​      return 42

   }

   利用 with 调用一个对象实例的多个方法

class Turtle {

   fun penDown()

   fun penUp()

   fun turn(degrees: Double)

   fun forward(pixels: Double)

}

val myTurtle = Turtle()

with(myTurtle) { //draw a 100 pix square

   penDown()

   for(i in 1..4) {

​      forward(100.0)

​      turn(90.0)

   }

   penUp()

}

   Java 7’s try with resources

   val stream = Files.newInputStream(Paths.get("/some/file.txt"))

   stream.buffered().reader().use { reader ->

​      println(reader.readText())

   }

------

Type cast class label