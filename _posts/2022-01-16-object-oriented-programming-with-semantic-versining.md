---
title: Semantic Versioning으로 보는 오브젝트 지향 프로그래밍.
layout: post
---

오브젝트 지향 언어인 C++, Java, Kotlin을 쓴다고 해서 OOP(Object Oriented Programming)을 실행하는 건 아니다. OOP 언어를 쓴다는 건 어디까지나 컴파일러의 힘으로 오브젝트 지향 디자인 패턴을 쓰기 쉬운 문법으로 개발을 할 수 있는 것 뿐이다. 오브젝트 지향 언어를 쓰는 것과 오브젝트 지향 개발을 하는 건 거의 상관이 없는 일이다.

간단하게 [Semantic Versioning 2.0.0](https://semver.org) 사양을 활용한 기능으로 기초적인 OOP 코드 작성법을 보인다. 동시에 OOP가 아닌 코드를 비교한다.

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

    /**
     * @return version coreの文字列。
     */
    override fun toString(): String {
        var rv = "$major.$minor.$patch"
        // 생략
        return rv
    }
}
```

## 참고

- [Semantic Versioning 2.0.0](https://semver.org)
