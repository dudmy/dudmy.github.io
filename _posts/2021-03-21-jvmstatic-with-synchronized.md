---
layout: post
title: "JvmStatic with Synchronized"
categories: android
comments: true
---

JvmStatic & Synchronized 어노테이션이 동시에 선언된 정적 함수를 어디에서 (자바 or 코틀린) 호출하느냐에 따라 lock을 거는 객체가 달라지는 점을 주의해야 한다. 아래는 간략한 예시 코드이다.

```
sealed class A {
    object A1 : A()
    object A2 : A()

    companion object {
        @JvmStatic
        @Synchronized
        fun getInstance(): A {
            return if (/* ... */) A1() else A2()
        }
    }
}

class B { // Kotlin Class
    init {
        A.getInstance()
    }
}

public class C { // Java Class
    public C() {
        A.getInstance();
    }
}
```

### Thread Dump

```
"DEADLOCK_TEST-1" prio=3 waiting for monitor entry
  java.lang.Thread.State: BLOCKED
     waiting for DEADLOCK_TEST-2 to release lock on <0x6f5> (a java.lang.Class)
      at net.dudmy.example.A$Companion.getInstance(A.kt:55)
      - locked <0x3044> (a net.dudmy.example.A$Companion)
      at net.dudmy.example.B.<init>(B.kt:13)

"DEADLOCK_TEST-2" prio=3 waiting for monitor entry
  java.lang.Thread.State: BLOCKED
     waiting for DEADLOCK_TEST-1 to release lock on <0x3044> (a net.dudmy.example.A$Companion)
      at net.dudmy.example.A$Companion.getInstance(A.kt:46)
      at net.dudmy.example.A.getInstance(A.kt:-1)
      - locked <0x6f5> (a java.lang.Class)
      at net.dudmy.example.C.<init>(C.java:53)
```

### DEADLOCK_TEST-1 : Call from Kotlin

① `locked ...$Companion` ⟶ ③ `waiting to release lock on java.lang.Class`  
코틀린에서는 아래 함수를 호출하여 Companion의 lock을 먼저 획득

```
public static final class Companion {
    @JvmStatic
    @NotNull
    public final synchronized A getInstance() {
        // ...
    }
}
```

### DEADLOCK_TEST-2 : Call from Java

② `locked java.lang.Class` ⟶ ④ `waiting to release lock on ...$Companion`  
자바에서는 아래 함수를 호출하여 A Class의 lock을 먼저 획득

```
public abstract class A {
   @JvmStatic
   @NotNull
   public static final synchronized A getInstance() {
      return Companion.getInstance();
   }
}
```

### 개선 방향

JvmStatic 어노테이션 제거 후 자바에서는 `A.Companion.getInstance()` 호출하도록 하여 해결할 수도 있지만, Synchronized 어노테이션 대신 lock을 걸고자 하는 객체를 명시적으로 작성하는게 안전할 듯 하다.

```kotlin
sealed class A {
    object A1 : A()
    object A2 : A()

    companion object {
        @JvmStatic
        fun getInstance(): A {
            synchronized(A::class.java) {
                return if (/* ... */) A1() else A2()
            }
        }
    }
}
```
