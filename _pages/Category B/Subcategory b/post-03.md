Kotlin은 간결하고 읽기 쉬운 코드 작성이 가능한 언어입니다. 기본 문법을 주요 주제별로 정리해드릴게요.

---

### ✏️ **1. 변수 선언**
```kotlin
val name: String = "Kotlin" // 읽기 전용(불변)
var age: Int = 25           // 변경 가능(가변)

// 자료형 생략 가능
val city = "Seoul"
var score = 90
```
- `val` : 값을 변경할 수 없는 **불변 변수**  
- `var` : 값을 변경할 수 있는 **가변 변수**

---

### 🔢 **2. 기본 자료형**
```kotlin
val intNum: Int = 10        // 정수
val doubleNum: Double = 3.14 // 실수
val isTrue: Boolean = true   // 불리언
val char: Char = 'A'         // 문자
val str: String = "Hello"    // 문자열
```

- Kotlin은 **기본 자료형도 객체**로 다룸 (ex: `Int`, `Double`)

---

### 🔄 **3. 조건문**
```kotlin
val score = 85

// if-else
if (score >= 90) {
    println("A 등급")
} else if (score >= 80) {
    println("B 등급")
} else {
    println("C 등급")
}

// if는 표현식으로 사용 가능
val result = if (score >= 80) "합격" else "불합격"
println(result)
```

- `if`는 값을 반환하는 표현식으로 사용 가능

---

### 🔁 **4. 반복문**
```kotlin
// for문 (범위 연산자 사용)
for (i in 1..5) {
    println(i) // 1, 2, 3, 4, 5
}

// 하행 반복
for (i in 5 downTo 1) {
    println(i) // 5, 4, 3, 2, 1
}

// while문
var count = 3
while (count > 0) {
    println("Count: $count")
    count--
}
```

- `1..5` : 1부터 5까지  
- `1 until 5` : 1부터 4까지 (끝 포함 안함)  
- `step` : 간격 지정 가능  

---

### 📦 **5. 함수 선언**
```kotlin
// 기본 함수
fun add(a: Int, b: Int): Int {
    return a + b
}

// 표현식 형태의 함수
fun multiply(a: Int, b: Int) = a * b

// 반환값 없는 함수
fun printMessage(msg: String): Unit {
    println(msg)
}
```

- `Unit`은 Java의 `void`와 같으며 생략 가능  

---

### 📝 **6. 클래스와 객체**
```kotlin
class Person(val name: String, var age: Int) {
    fun introduce() {
        println("안녕하세요, 저는 $name이고, 나이는 $age살입니다.")
    }
}

val person = Person("홍길동", 30)
person.introduce()
```

- **주 생성자**는 클래스 헤더에 선언  
- **부 생성자**는 `constructor` 키워드 사용  

---

### 📚 **7. 컬렉션 (List, Set, Map)**
```kotlin
// List (불변)
val fruits = listOf("사과", "바나나", "딸기")
println(fruits[0]) // 사과

// MutableList (가변)
val mutableFruits = mutableListOf("사과", "바나나")
mutableFruits.add("딸기")

// Set (중복 없음)
val numbers = setOf(1, 2, 3, 3)
println(numbers) // [1, 2, 3]

// Map (Key-Value)
val user = mapOf("name" to "홍길동", "age" to 30)
println(user["name"]) // 홍길동
```

---

### ⚡ **8. Null 안전성**
```kotlin
var name: String? = "Kotlin" // ?를 붙이면 null 허용
name = null

// 안전 호출 (Safe Call)
println(name?.length) // null이면 실행 안됨

// 엘비스 연산자 (?:)
val length = name?.length ?: 0
println(length) // name이 null이면 0 반환

// 강제 null 처리 (!!)
println(name!!.length) // null이면 예외 발생
```

---

### 🛠️ **9. 확장 함수**
```kotlin
fun String.greet() {
    println("안녕하세요, $this!")
}

"홍길동".greet() // 안녕하세요, 홍길동!
```

- 기존 클래스에 새로운 기능을 추가하는 방법

---

### 🕰️ **10. 코루틴 (비동기 처리)**
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    launch {
        delay(1000L)
        println("코루틴 실행")
    }
    println("메인 실행")
}
```

- 코루틴은 **비동기 작업**을 간결하게 처리하는 방법  
- `launch`는 새로운 작업을 시작하고, `async`는 값을 반환  

---

이 외에도 **람다, 데이터 클래스, sealed 클래스, 제네릭** 등 다양한 기능이 있습니다. 특정 부분을 더 자세히 설명해드릴 수도 있는데, 어느 부분에 좀 더 관심이 있으신가요? 😊
