‍

### 函数定义

```kotlin
// 函数定义使用关键字 fun，参数格式为：参数 : 类型
fun sum(a: Int, b: Int): Int {   // Int 参数，返回值 Int
    return a + b
}
```

‍

### Hello, World!

```kotlin
fun main(args: Array<String>) {
    println("Hello World!")	// Hello, World!
}
```

‍

‍

### 面向对象

```kotlin
class Greeter(val name: String) {

    fun greet() {
        println("Hello, $name")		// Hello, World!
    }

}

fun main(args: Array<String>) {
    Greeter("World!").greet()          // 创建一个对象不用 new 关键字
}
```

‍
