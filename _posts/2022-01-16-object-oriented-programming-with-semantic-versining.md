---
title: Semantic Versioning으로 보는 오브젝트 지향 프로그래밍.
layout: post
---

오브젝트 지향 언어인 C++, Java, Kotlin을 쓴다고 해서 OOP(Object Oriented Programming)을 실행하는 건 아니다. OOP 언어를 쓴다는 건 어디까지나 컴파일러의 힘으로 오브젝트 지향 디자인 패턴을 쓰기 쉬운 문법으로 개발을 할 수 있는 것 뿐이다. 오브젝트 지향 언어를 쓰는 것과 오브젝트 지향 개발을 하는 건 거의 상관이 없는 일이다.

간단하게 [Semantic Versioning 2.0.0](https://semver.org) 사양을 활용한 기능으로 기초적인 OOP 코드 작성법을 보인다. 동시에 OOP가 아닌 코드를 비교한다.

## TL;DR

1. 오브젝트 생성장에서 입력값을 검증해라.
2. 오브젝트는 상태와 상태를 활용할 수 있는 기능을 함께 묶어라. 그 반대도 마찬가지다.

## 사양

1. 유의적 버전(semantic version)을 인식한다.
2. 요구 버전이 현재 버전보다 높다면 업데이트 한다.

## 생김새

### 요구조건

> 1. 유의적 버전을 쓰는 소프트웨어는 반드시 공개 API를 선언한다. 이 API는 코드 자체로 선언하거나 문서로 엄격히 명시해야 한다. 어떤 방식으로든, 정확하고 이해하기 쉬워야 한다.
> 2. 보통 버전 번호는 반드시 X.Y.Z의 형태로 하고, X, Y, Z는 각각 자연수(음이 아닌 정수)이고, 절대로 0이 앞에 붙어서는 안 된다. X는 주(主)버전 번호이고, Y는 부(部)버전 번호이며, Z는 수(修)버전 번호이다. 각각은 반드시 증가하는 수여야 한다. 예: 1.9.0 -> 1.10.0 -> 1.11.0.
> 3. 특정 버전으로 패키지를 배포하고 나면, 그 버전의 내용은 절대 변경하지 말아야 한다. 변경분이 있다면 반드시 새로운 버전으로 배포하도록 한다.
> 4. ...
>
> -- <cite>유의적 버전 명세 (SemVer)</cite>

### 문자열

단순 문자열로 비교할 경우, `1.2.3`과 `1.2.10`을 비교하면 `1.2.3`가 업데이트가 필요 없다고 인식하는 문제가 생긴다.

```Kotlin
fun checkUpdate(current: String, required: String): Boolean {
    return if (current < required) {
        true
    } else {
        false
    }
}
```

### 프로시져(Procedure)

프로시저 내부에서 버전 비교에 필요한 `major`, `minor`, `patch`를 추출해서 숫자료 변경하고 업데이트가 필요한지 판단한다.

```Kotlin
fun someLogic() {
    val current = "1.2.3"   // 어디선가 읽어온 현재 버전.
    val required = "1.2.4"  // 어디선가 읽어온 필요 버전.

    if (checkUpdate(current, required)) {
        // 업데이트 실행.
    }

    // 기타 기능.
}
```
```Kotlin
const val VERSION_CORE_PATTERN = "(\\d+)\\.(\\d+)\\.(\\d+)"
const val PRE_RELEASE_PATTERN = "[\\da-zA-Z]+(\\.[\\da-zA-Z]+)*"
const val BUILD_PATTERN = "[\\da-zA-Z]+"
const val VERSION_PATTERN = "$VERSION_CORE_PATTERN(-$PRE_RELEASE_PATTERN)?(\\+$BUILD_PATTERN)?"
val VERSION_REGEX = VERSION_PATTERN.toRegex()

fun checkUpdate(current: String, required: String): Boolean {
    val currentValues = VERSION_REGEX.find(current)!!.groupValues
    val requiredValues = VERSION_REGEX.find(required)!!.groupValues

    val currentMajor = currentValues[1].toInt(10)
    val currentMinor = currentValues[2].toInt(10)
    val currentPatch = currentValues[3].toInt(10)

    val requiredMajor = requiredValues[1].toInt(10)
    val requiredMinor = requiredValues[2].toInt(10)
    val requiredPatch = requiredValues[3].toInt(10)

    return if (
            currentMajor < requiredMajor ||
            (currentMajor == requiredMajor && currentMinor < requiredMinor) ||
            (currentMajor == requiredMajor && currentMinor == requiredMinor && currentPatch < requiredPatch)
    ) {
        true
    } else {
        false
    }
}
```

### 자료구조(Data Structure)

`major`, `minor`, `patch` 외에도 `preRelease`, `build`까지 정보를 유지하는 자료구조를를 사용해서 업데이트가 필요한지 판단한다.

```Kotlin
fun someLogic() {
    val current = "1.2.3"   // 어디선가 읽어온 현재 버전.
    val required = "1.2.4"  // 어디선가 읽어온 필요 버전.

    val currentVersion = convert(current)
    val requiredVersion = convert(required)

    if (checkUpdate(currentVersion, requiredVersion)) {
        // 업데이트 실행.
    }

    // 기타 기능.
}

const val VERSION_CORE_PATTERN = "(\\d+)\\.(\\d+)\\.(\\d+)"
const val PRE_RELEASE_PATTERN = "[\\da-zA-Z-]+(\\.[\\da-zA-Z-]+)*"
const val BUILD_PATTERN = "[\\da-zA-Z-]+(\\.[\\da-zA-Z-]+)*"
const val VERSION_PATTERN = "$VERSION_CORE_PATTERN(-($PRE_RELEASE_PATTERN))?(\\+($BUILD_PATTERN))?"
val VERSION_REGEX = VERSION_PATTERN.toRegex()

fun convert(name: String): SemanticVersion {
    val values = VERSION_REGEX.find(name)!!.groupValues

    val major = values[1].toInt(10)
    val minor = values[2].toInt(10)
    val patch = values[3].toInt(10)
    val preRelease = values[5].ifBlank { null }
    val build = values[8].ifBlank { null }

    return SemanticVersion(major, minor, patch, preRelease, build)
}
```
```Kotlin
data class SemanticVersion(
        val major: Int,
        val minor: Int,
        val patch: Int,
        val preRelease: String? = null,
        val build: String? = null
)
```
```Kotlin
fun checkUpdate(current: SemanticVersion, required: SemanticVersion): Boolean {
    return current.major < required.major ||
            (current.major == required.major && current.minor < required.minor) ||
            (current.major == required.major && current.minor == required.minor && current.patch < required.patch)
}
```

### 오브젝트(Object)

```Kotlin
fun someLogic() {
    val current = "1.2.3"   // 어디선가 읽어온 현재 버전.
    val required = "1.2.4"  // 어디선가 읽어온 필요 버전.

    val currentVersion = SemanticVersion(current)
    val requiredVersion = SemanticVersion(required)

    if (currentVersion < requiredVersion) {
        // 업데이트 실행.
    }

    // 기타 기능.
    println("current.major=${currentVersion.major}")
    println("required.major=${requiredVersion.major}")
}
```
```Kotlin
class SemanticVersion : Comparable<SemanticVersion> {
    companion object {
        const val VERSION_CORE_PATTERN = "(\\d+)\\.(\\d+)\\.(\\d+)"
        const val PRE_RELEASE_PATTERN = "[\\da-zA-Z-]+(\\.[\\da-zA-Z-]+)*"
        const val BUILD_PATTERN = "[\\da-zA-Z-]+(\\.[\\da-zA-Z-]+)*"
        const val VERSION_PATTERN = "$VERSION_CORE_PATTERN(-($PRE_RELEASE_PATTERN))?(\\+($BUILD_PATTERN))?"

        val VERSION_CORE_REGEX = VERSION_CORE_PATTERN.toRegex()
        val PRE_RELEASE_REGEX = PRE_RELEASE_PATTERN.toRegex()
        val BUILD_REGEX = BUILD_PATTERN.toRegex()
        val VERSION_REGEX = VERSION_PATTERN.toRegex()
    }

    var major: Int
        private set

    var minor: Int
        private set

    var patch: Int
        private set

    var preRelease: String?
        private set

    var build: String?
        private set

    constructor(name: String) {
        VERSION_REGEX.find(name)!!.groupValues.also {
            major = it[1].toInt(10)
            minor = it[2].toInt(10)
            patch = it[3].toInt(10)
            preRelease = it[5].ifBlank { null }
            build = it[8].ifBlank { null }
        }
    }

    constructor(
            major: Int, minor: Int, patch: Int,
            preRelease: String? = null, build: String? = null
    ) {
        this.major = major
        this.minor = minor
        this.patch = patch
        this.preRelease = preRelease
        this.build = build
    }


    override fun compareTo(other: SemanticVersion): Int {
        var rv = major.compareTo(other.major)
        // 생략
        return rv
    }

    override fun equals(other: Any?): Boolean {
        // 생략
        return false
    }

    override fun hashCode(): Int {
        var result = major
        // 생략
        return result
    }

    override fun toString(): String {
        var rv = "$major.$minor.$patch"
        // 생략
        return rv
    }
}
```

## 상태와 기능

OOP에서 말하는 오브젝트는 상태와 기능을 함께 관리하는 디자인패턴이다.

1. 상태(property 혹은 member field)와 함께 그 상태로 할 수 있는 기능(function 혹은 method)을 함께 관리한다.
2. 기능과 기능에 필요한 상태를 함께 관리한다.

### 문자열

문자열로 비교할 경우, `1.2.3`과 `1.2.10`을 비교하면 `patch`의 `3`이 `10`의 `1`보다 큰 문자로 인식하기 때문에 `1.2.3`이 더 높은 버전으로 인식된다. 그래서 문자열만으로는 버전으로 사용할 수 없고, 버전 문자열에서 각각의 요소를 추출해서 요소별로 비교하는 로직이 필요하다.

### 프로시저

더 높은 버전인지 검사하는 `checkUpdate` 메서드 내부에서 크기 비교에 사용할 `major`, `minor`, `patch`를 추출하고 적절한 비교가 가능하도록 정수로 변환한 후에 버전을 비교해서 결과를 반환한다.

```Kotlin
fun checkUpdate(current: String, required: String): Boolean {
    val currentValues = VERSION_REGEX.find(current)!!.groupValues
    val requiredValues = VERSION_REGEX.find(required)!!.groupValues

    val currentMajor = currentValues[1].toInt(10)
    val currentMinor = currentValues[2].toInt(10)
    val currentPatch = currentValues[3].toInt(10)

    val requiredMajor = requiredValues[1].toInt(10)
    val requiredMinor = requiredValues[2].toInt(10)
    val requiredPatch = requiredValues[3].toInt(10)

    return if (
            currentMajor < requiredMajor ||
            (currentMajor == requiredMajor && currentMinor < requiredMinor) ||
            (currentMajor == requiredMajor && currentMinor == requiredMinor && currentPatch < requiredPatch)
    ) {
        true
    } else {
        false
    }
}
```

프로시저 방식을 사용하면 업데이트가 필요한지는 알 수 있지만, `someLogic()`는 그외의 다른 정보를 얻을 수 없다. 또한 현재 버전의 `major` 값이 얼마인지도 알 수 없다.

### 자료구조

유의적 버전의 각 부분을 속성으로 가지는 자료구조를 정의하고 문자열을 자료구조로 변환하는 유틸리티 메서드를 추가했다.

```Kotlin
// 자료구조
data class SemanticVersion(
        val major: Int,
        val minor: Int,
        val patch: Int,
        val preRelease: String? = null,
        val build: String? = null
)
```

속성을 추출하는 코드를 메서드로 분리하면서 콜 스택의 증가, 코드와 변수의 메모리 사용량의 증가를 대가로 한 번 추출한 속성을 계속해서 사용할 수 있게 된다.

```Kotlin
// 변환 유틸리티 메서드.
fun convert(name: String): SemanticVersion {
    val values = VERSION_REGEX.find(name)!!.groupValues

    val major = values[1].toInt(10)
    val minor = values[2].toInt(10)
    val patch = values[3].toInt(10)
    val preRelease = values[5].ifBlank { null }
    val build = values[8].ifBlank { null }

    return SemanticVersion(major, minor, patch, preRelease, build)
}
```

그러나 버전을 비교하기 위해선 `checkUpdate(current: SemanticVersion, required: SemanticVersion)` 함수를 호출해야만 한다.

```Kotlin
fun checkUpdate(current: SemanticVersion, required: SemanticVersion): Boolean {
    return current.major < required.major ||
            (current.major == required.major && current.minor < required.minor) ||
            (current.major == required.major && current.minor == required.minor && current.patch < required.patch)
}
```

그리고 `someLogic()` 외에도 버전을 비교해야 하는 코드를 작성하는 개발자는 버전 문자열을 `convert(name: String)` 함수로 변환한 후에 `checkUpdate(current: SemanticVersion, required: SemanticVersion)` 함수로 버전을 비교하는 순서를 정확히 파악하고 그 순서에 맞춰 코드를 작성해야 한다.

### 오브젝트

`constructor(name: String)`에서 문자열을 분해하고, 각 setter에서 더 자세한 검증을 한다.

추가로 `constructor(major: Int, minor: Int, patch: Int, preRelease: String? = null, build: String? = null)`를 마련해 문자열을 해석하는 것이 아니라 코드상에서 직접 인스턴스를 만드는 용도로 준비한다.

```Kotlin
class SemanticVersion : Comparable<SemanticVersion> {
    var major: Int
      private set(value) {
        // value를 검사한 후에 backing field에 할당.
      }
    var minor: Int
    var patch: Int
    var preRelease: String?
    var build: String?

    constructor(name: String) {
      // 문자열에서 정보를 추출해서 속성에 할당.
    }

    constructor(
            major: Int, minor: Int, patch: Int,
            preRelease: String? = null, build: String? = null
    ) {
      // 각 인자를 속성에 할당.
    }

    override fun compareTo(other: SemanticVersion): Int {
        var rv = major.compareTo(other.major)
        // 기타 버전 비교.
        return rv
    }
}
```

단순 자료구조처럼 버전의 각 부분(`major`, `minor` 등)이 `SemanticVersion` 인스턴스로 묶어서 관리할 수 있으면서 `compareTo(other: SemanticVersion)` 메서드로 비교할 수 있다.

Kotlin 언어 특성으로 버전을 비교할 때는 `currentVersion < requiredVersion`처럼 부등호(`<`)로 직관적으로 버전을 비교할 수 있게 된다.

`checkUpdate(currentVersion, requiredVersion)` => `currentVersion < requiredVersion`

```Kotlin
fun someLogic() {
    val current = "1.2.3"
    val required = "1.2.4"

    val currentVersion = SemanticVersion(current)
    val requiredVersion = SemanticVersion(required)

    if (currentVersion < requiredVersion) {
        // 업데이트 실행.
    }

    // 기타 기능.
}
```

## 정리

OOP는 프로시저나 단순한 자료구조를 사용한 개발 방식에 비해 파일이 많아지고 전체 코드도 늘어나며, 콜스택도 늘어난다.

그럼에도 불구하고 오브젝트를 만들어서 사용하는 것에는 여러 장점이 있다.

첫째, 인스턴스를 만드는 과정에서 입력값이 맞는지 검증하는 과정을 만들 수 있다. 그래서 인스턴스를 가지고 있으면 그 인스턴스의 내용은 옳다는 전제하에 사용할 수 있다.

둘째, 메서드(기능)에 필요한 값은 되도록 인스턴스 속성(상태)으로 가지고 있기 때문에 인스턴스가 가지고 있을 수 없는 값만 메서드 인자로 받는다. 그래서 인스턴스를 사용하는 코드는 최소한의 변수만 관리하면 되기 때문에 코드가 간단해진다.

## 참고

- [샘플 코드 전체](https://github.com/JustBurrow/pages/tree/master/oop)
- [Semantic Versioning 2.0.0](https://semver.org)
