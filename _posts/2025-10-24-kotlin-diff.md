---
layout: post
title: "코틀린 언어를 공부하며: 주요 차이점과 코틀린의 장점"
date: 2025-10-24 13:21 +0900
description: 자바와 코틀린의 주요 차이점과 장점
category: Programming
tags: [Java, Kotlin, Programming]
---

## 개요

백엔드 개발자로 전향하면서 자바를 시작했다. 약 6~7개월 동안 같이 자바 공부하던 멤버와 함께 `Kotlin in Action` 책 스터디를 하면서 자바와 같은 JVM 기반 언어임에도 불구하고 많은 차이점이 있어서 한 편으로는 놀라웠고 재밌었다.
코틀린 공부를 하며 느낀 코틀린의 주요 차이점과 코틀린이 제공하는 장점들을 살펴보며 해당 글을 작성했다.

## 주요 차이점

### 1. Null Safety (Null 안정성)

코틀린의 가장 큰 특징 중 하나는 타입 시스템에 null 안정성이 내장되어 있다는 점이다.

**Java**
자바에서는 해당 변수가 `null`일 경우 `NullPointerException` 예외가 발생할 수 있다.

```java
String name = null;
int length = name.length(); // NullPointerException 발생 가능
```

**Kotlin**
코틀린에서는 `Elvis` 연산자를 사용하여 예외 발생 없이 안전하게 호출이 가능하다.

```kotlin
var name: String? = null
val length = name?.length ?: 0 // 안전한 호출, Elvis 연산자
```

### 2. 데이터 클래스

단순히 데이터를 담는 클래스를 만들 때 코틀린은 훨씬 간결하다.

**Java**

```java
public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getter, setter, equals, hashCode, toString 등 필요
    public String getName() { return name; }
    public int getAge() { return age; }

    @Override
    public boolean equals(Object o) { /* ... */ }
    @Override
    public int hashCode() { /* ... */ }
    @Override
    public String toString() { /* ... */ }
}
```

- `Java` 언어로 사용할 경우 위와 같이 많은 코드를 작성하거나 어노테이션을 사용해야 한다.

**Kotlin**

```kotlin
data class User(val name: String, val age: Int)
// equals, hashCode, toString, copy 자동 생성
```

- `data class`에서는 어노테이션을 사용하지 않고 자동으로 생성되기 때문에 간략하게 작성이 가능하다.

### 3. 확장 함수

기존 클래스에 새로운 함수를 추가할 수 있다.

**Java**

```java
public class StringUtils {
    public static boolean isValidEmail(String str) {
        return str.contains("@");
    }
}

String email = "test@example.com";
boolean valid = StringUtils.isValidEmail(email);
```

**Kotlin**

```kotlin
fun String.isValidEmail(): Boolean {
    return this.contains("@")
}

val email = "test@example.com"
val valid = email.isValidEmail() // 더 자연스러운 호출
```

### 4. 변수 선언과 타입 추론

- 변수 선언 시 타입을 작성하지 않아도, 타입 추론이 가능하다.

**Java**

```java
String name = "John";
final int age = 25;
List<String> items = new ArrayList<>();
```

**Kotlin**

```kotlin
var name = "John"  // 타입 추론: String
val age = 25       // 불변, 타입 추론: Int
val items = mutableListOf<String>()
```

### 5. 기본 파라미터와 Named Arguments

**Java**

```java
public void createUser(String name, int age, String email) {
    // ...
}

// 오버로딩이 필요
public void createUser(String name, int age) {
    createUser(name, age, "");
}

createUser("John", 25, "john@example.com");
```

**Kotlin**

```kotlin
fun createUser(
    name: String,
    age: Int,
    email: String = ""
) {
    // ...
}

createUser("John", 25)  // 기본값 사용
createUser(name = "John", age = 25, email = "john@example.com")
```

### 6. 스마트 캐스팅

**Java**

- 자바에서는 `instanceof`로 형변환이 가능한지 체크한 후 명시적 캐스팅을 하여 형변환을 해야 한다.

```java
if (obj instanceof String) {
    String str = (String) obj;  // 명시적 캐스팅 필요
    System.out.println(str.length());
}
```

**Kotlin**

- 하지만 코틀린에서는 형변환이 가능한지 체크한 후에는 자동으로 캐스팅이 되어 명시적 캐스팅이 필요 없어 간단하다.

```kotlin
if (obj is String) {
    println(obj.length)  // 자동으로 캐스팅됨
}
```

### 7. When 표현식 (Switch 개선)

- 간단하게 설명하면 자바 `Switch`를 코틀린에서는 `When` 표현식을 사용하는 것이다.

**Java**

```java
switch (status) {
    case "SUCCESS":
        return "성공";
    case "ERROR":
        return "실패";
    default:
        return "알 수 없음";
}
```

**Kotlin:**

```kotlin
when (status) {
    "SUCCESS" -> "성공"
    "ERROR" -> "실패"
    else -> "알 수 없음"
}
```

---

## 마치며

자바에서 코틀린으로 전환하면서 가장 크게 느낀 점은 코드의 간결함과 안전성이었다. 특히 널 안정성은 런타임 에러를 크게 줄여주었고, 데이터 클래스와 확장 함수는 개발 생산성을 높여주었다.

처음에는 `다른 언어를 다시 배워야 하나. 언제 또 배우지`라는 생각이 있었지만, 이전에 사용했던 `TypeScript`와 `Java` 덕분인지 금방 배우고 적응하기 쉬웠다.

코틀린은 자바의 장점은 유지하면서도 현대적인 언어 기능을 제공하기 때문에, 안드로이드 개발뿐만 아니라 서버 사이드 개발에서도 점점 더 많이 채택되고 있는 추세다. 자바 개발자라면 한 번쯤 코틀린을 배워보는 것을 강력히 추천한다.

> 코틀린 붐은 온다! (언젠가)
