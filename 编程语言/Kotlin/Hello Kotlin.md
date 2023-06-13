# Hello Kotlin

‍

## Hello, World!

```kotlin
fun main(args: Array<String>) {
    println("Hello World!")	// Hello, World!
}
```

‍

‍

## 面向对象

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
