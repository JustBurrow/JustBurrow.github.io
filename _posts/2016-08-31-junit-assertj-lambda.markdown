---
layout: post
title: JUnit, AssertJ 그리고 Lambda
---

단위테스트를 좀 더 잘, 예쁘고 멋지고 ~~타이핑 덜 하게~~ 만들어 보자.

## 테스트 대상

간단한 클래스 하나를 가정한다. 이 클래스는 어디까지나 테스트를 만들기 위한 것으로, 내부 상태를 가지고 있다.
테스트할 메서드는 `public boolean someMethod(String text)`로, `hidden` 필드의 값(상태)에 따라 동작이 달라진다.

```java
package kr.lul.pages.junitassertjlambda;

import java.util.Random;

public class SomeClass {
  private String hidden;

  public SomeClass() {
    this.hidden = "";
  }

  public void setAsRandom() {
    final int length = new Random().nextInt(100) - 1;

    if (0 <= length) {
      StringBuilder sb = new StringBuilder();
      for (int i = 0; i < length; i++) {
        sb.append(i % 10);
      }
      this.hidden = sb.toString();
    }
  }

  public boolean someMethod(String text) {
    if (null == text) {
      throw new NullPointerException("text");
    } else if (3 > text.length()) {
      throw new IllegalArgumentException("text length : " + text.length());
    }

    return this.hidden.length() < text.length();
  }
}

```

## 기본

그리고 이 메서드를 테스트할 단위 테스트를 만들자. JUnit4를 사용한 가장 기본적인 테스트 코드는 다음과 같은 형태가 될 것이다.

```java
package kr.lul.pages.junitassertjlambda;

import static org.hamcrest.Matchers.allOf;
import static org.hamcrest.Matchers.endsWith;
import static org.hamcrest.Matchers.startsWith;
import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertThat;
import static org.junit.Assert.fail;

import org.junit.Before;
import org.junit.Test;

import kr.lul.pages.junitassertjlambda.SomeClass;

/**
 * 기본 테스트 코드.
 */
public class SomeClassTest1Basic {
  private SomeClass sc;

  @Before
  public void setUp() throws Exception {
    this.sc = new SomeClass();
  }

  @Test
  public void testSomeMethod() throws Exception {
    this.sc.someMethod("123");
  }

  /**
   * 단순히 {@link NullPointerException}이 발생했다는 사실만 알 수 있을 뿐, 왜 예외가 발생했는지 알 수 없다.
   * 이 경우, {@link SomeClass}의 <code>hidden</code> 필드가 <code>null</code>일 경우에도 NPE가 발생할 수 있다.
   */
  @Test(expected = NullPointerException.class)
  public void testSomeMethodWithNull() {
    this.sc.someMethod(null);
  }

  /**
   * 예외를 대하는 바른 자세.
   */
  @Test
  public void testSomeMethodWithEmpty() throws Exception {
    try {
      this.sc.someMethod("");
      fail();
    } catch (IllegalArgumentException e) {
      assertEquals("text length : 0", e.getMessage());
    }
  }

  /**
   * 예외를 대하는 바른 자세..?
   */
  @Test
  public void testSomeMethodWithLength1() throws Exception {
    try {
      this.sc.someMethod("1");
      fail();
    } catch (IllegalArgumentException e) {
      assertEquals("text length : 1", e.getMessage());
    }
  }

  /**
   * 벌써 3번째 반복이다.
   * {@link SomeClass}는 예외가 발생하는 범위가 0, 1, 2의 3가지 경우의 수로 전체 테스트 코드 작성이 가능하다.
   * 하지만 10 ~ 30 정도의 테스트 케이스를 전부 작성하기엔 지저분하고 하지 않기엔 찝찝한 경우를 만날 수도 있다.
   * 예외에 대한 테스트를 좀 더 세분화했다.
   */
  @Test
  public void testSomeMethodWithLength2() throws Exception {
    try {
      this.sc.someMethod("12");
      fail();
    } catch (IllegalArgumentException e) {
      assertThat(e.getMessage(), allOf(startsWith("text length"), endsWith("" + "12".length())));
    }
  }
}
```

## AsserJ를 도입하자

[AssertJ](http://joel-costigliola.github.io/assertj)를 사용하면 플루언트API(메서드체인)를 사용해 테스트를 작성할 수 있다.

```java
package kr.lul.pages.junitassertjlambda;

import static org.assertj.core.api.Assertions.assertThat;
import static org.assertj.core.api.Assertions.assertThatThrownBy;

import org.assertj.core.api.ThrowableAssert.ThrowingCallable;
import org.junit.Before;
import org.junit.Test;

import kr.lul.pages.junitassertjlambda.SomeClass;

/**
 * AssertJ 도입.
 * JUnit을 테스트케이스를 실행하는 실행환경으로 사용하고, 실재 테스트(assertion)는 AssertJ에 위임한다.
 */
public class SomeClassTest2AssertJ {
  private SomeClass sc;

  @Before
  public void setUp() throws Exception {
    this.sc = new SomeClass();
  }

  /**
   * AssertJ를 도입해 테스트를 만들었다.
   */
  @Test
  public void testSomeMethod() throws Exception {
    assertThat(this.sc.someMethod("123")).isEqualTo(true);
  }

  /**
   * AssertJ는 예외도 다각도로 테스트할 수 있도록 도와준다.
   * 예외를 테스트하기 위해 {@link ThrowingCallable}로 메서드 실행을 캡슐화 한다.
   */
  @Test
  public void testSomeMethodWithNull() {
    assertThatThrownBy(new ThrowingCallable() {
      @Override
      public void call() throws Throwable {
        SomeClassTest2AssertJ.this.sc.someMethod(null);
      }
    }).isInstanceOf(NullPointerException.class)
        .hasMessage("text");
  }

  /**
   * 람다를 사용하면 코드가 깔끔하게 정리된다.
   */
  @Test
  public void testSomeMethodWithShortText() throws Exception {
    // 이랬던 코드가
    assertThatThrownBy(new ThrowingCallable() {
      @Override
      public void call() throws Throwable {
        SomeClassTest2AssertJ.this.sc.someMethod("");
      }
    }).isInstanceOf(IllegalArgumentException.class)
        .hasMessageStartingWith("text length")
        .hasMessageEndingWith("" + "".length());

    // 이렇게 정리된다.
    // 2개의 테스트 케이스를 실행하지만, 줄 수는 익명 클래스를 만든 경우와 비슷하다.
    assertThatThrownBy(() -> this.sc.someMethod("1"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageStartingWith("text length")
        .hasMessageEndingWith("" + "1".length());
    assertThatThrownBy(() -> this.sc.someMethod("12"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageStartingWith("text length")
        .hasMessageEndingWith("" + "12".length());
  }
}
```

## 가변 변수는 좋은 것이다

AssertJ와 Java8의 람다를 사용하면 코드가 상당히 깔끔해진다. 하지만 테스트 코드(`isInstanceOf`~`hasMessageEndingWith`)의 반복이 있다. 이것도 줄여보자.

```java
package kr.lul.pages.junitassertjlambda;

import static org.assertj.core.api.Assertions.assertThatThrownBy;

import org.assertj.core.api.ThrowableAssert.ThrowingCallable;
import org.junit.Before;
import org.junit.Test;

import kr.lul.pages.junitassertjlambda.SomeClass;

public class SomeClassTest3Varargs {
  private SomeClass sc;

  @Before
  public void setUp() throws Exception {
    this.sc = new SomeClass();
  }

  /**
   * 이 테스트 코드를 좀 더 깔끔하게 만들어보자.
   */
  @Test
  public void testSomeMethodWithShortText() throws Exception {
    assertThatThrownBy(() -> this.sc.someMethod(""))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageStartingWith("text length")
        .hasMessageEndingWith("" + "".length());
    assertThatThrownBy(() -> this.sc.someMethod("1"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageStartingWith("text length")
        .hasMessageEndingWith("" + "1".length());
    assertThatThrownBy(() -> this.sc.someMethod("12"))
        .isInstanceOf(IllegalArgumentException.class)
        .hasMessageStartingWith("text length")
        .hasMessageEndingWith("" + "12".length());
  }

  /**
   * 가변 변수로 메서드 실행을 객체화한 {@link ThrowingCallable} 목록을 받고, 메서드 호출을 실행한 후 발생한 예외를 검증한다.
   */
  private void assertHelper(ThrowingCallable... callables) {
    for (ThrowingCallable callable : callables) {
      assertThatThrownBy(callable).isInstanceOf(IllegalArgumentException.class)
          .hasMessageMatching("text length : [0-2]");
    }
  }

  /**
   * 테스트 헬퍼({@link #assertHelper(ThrowingCallable...)})를 호출해 단위 테스트를 실행하자.
   */
  @Test
  public void testSomeMethodWithShortTextViaHelper() throws Exception {
    this.assertHelper(
        () -> this.sc.someMethod(""),
        () -> this.sc.someMethod("1"),
        () -> this.sc.someMethod("12"));
  }
}
```

## 코드 길이 줄어든다고 좋은 코드는 아니다

공통 테스트 코드를 `assertHelper` 메서드로 위임하면서 반복 코드를 없앴지만, 테스트 케이스(`@Test` 메서드)와 실재 검증(`assertHelper`) 코드가 따로 존재한다. 이래선 코드가 길어질 경우 가독성이 떨어진다.

```java
package kr.lul.pages.junitassertjlambda;

import static org.assertj.core.api.Assertions.assertThatThrownBy;

import org.assertj.core.api.ThrowableAssert.ThrowingCallable;
import org.junit.Test;

import kr.lul.pages.junitassertjlambda.SomeClass;

public class SomeClassTest4VarargsAndLambda {
  private SomeClass sc = new SomeClass();

  /**
   * 내가 람다다.
   */
  public static interface AssertLambda {
    public void exec(ThrowingCallable callable);
  }

  /**
   * 내가 헬퍼다.
   * 헬퍼의 기능이 람다에 메서드 실행을 하나씩 넘겨주는 것으로 단순해졌다.
   * 그와 동시에 어떤 테스트를 해야 할 것인지 알 필요가 없어졌기 때문에, 범용성이 좋아졌다.
   * 범용성이 좋아졌기 때문에, 어디서든 접근, 사용할 수 있도록 접근 제한자를 <code>public static</code>으로 변경한다.
   */
  public static void assertHelper(AssertLambda assertion, ThrowingCallable... callables) {
    for (ThrowingCallable callable : callables) {
      assertion.exec(callable);
    }
  }

  /**
   * 실재 테스트 내용을 별도의 메서드에 작성한 {@link SomeClassTest3Varargs}에 비교하면,
   * 테스트(assertion) 내용을 테스트 케이스({@link Test})에 작성하는 방식으로 바뀌었다.
   *
   * 테스트 케이스의 주석, 메서드 이름, 테스트 내용, 테스트 대상(메서드 실행)이 짧은 코드로 한곳에 모였다.
   * 이 정도면 상당히 가독성 좋은 코드라고 생각한다.
   */
  @Test
  public void testSomeMethodWithShortTextViaHelper() throws Exception {
    SomeClassTest4VarargsAndLambda.assertHelper(
        (callable) -> assertThatThrownBy(callable)
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessageMatching("text length : [0-2]"),
        () -> this.sc.someMethod(""),
        () -> this.sc.someMethod("1"),
        () -> this.sc.someMethod("12"));
  }
}
```