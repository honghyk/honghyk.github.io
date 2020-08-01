---
layout: post
title: Dagger2 시작하기, 근데 왜?
tags: [Android, Dependency Injection, Dagger2]
comments: true
---

안드로이드에서  Dagger 같은 의존성 주입 프레임워크의 도입이 왜 필요할까?

> 본 포스트는 [Why do we use the Dependency Injection Framework like Dagger in Android?](https://blog.mindorks.com/why-do-we-use-the-dependency-injection-framework-in-android) 를 참고하여 작성 되었습니다.

<br>

## 의존성 주입(Dependency Injection) 이란?

의존성 주입은 Inversion of Control(IoC) 개념을 바탕으로 합니다. 클래스들 간의 의존성을 없애야 한다는 것입니다. 그래서 각 클래스들은 내부에서 직접 다른 클래스의 인스턴스를 생성하지 않고, 외부로부터 의존성을 주입 받아야 합니다.

이렇게 클래스 간 의존성을 제거함으로써 얻을 수 있는 몇 가지 이점이 있습니다.

1. 한 클래스를 변경 했을 때, 그 클래스를 사용하는 다른 클래스를 모두 바꿔야 하는 수고를 덜 수 있습니다.
2. 한 클래스를 변경 했을 때, 다른 클래스에서 나타날 수 있는 예기치 못한 에러를 방지할 수 있습니다.
3. 각 클래스들은 독립적으로 동작하기 때문에 코드의 재사용성을 높일 수 있습니다.

<br>

## 안드로이드에서 Dagger 같은 DI 프레임워크가 갖는 이점은?

안드로이드 개발은 Activity, Fragment와 같은 안드로이드 프레임워크 위에서 이루어지게 됩니다. 각 컴포넌트 간에도 강한 의존성을 가지며, 생명 주기(lifecycle)에 대한 관리도 필요합니다. 따라서 외부에서 의존성을 주입을 통한 독립적인 클래스 관리가 더 큰 중요성을 가집니다.

Activity A와 Activity B가 있고 두 Activity 모두 Downloader라는 객체가 필요한 경우를 예로 들어 봅시다. Downloader는 Request 객체에 요청을 보내고 Request 객체는 Executor와 HTTP Client 객체를 필요로 합니다.

![dagger](/images/dagger_part1_image.png)
출처: [https://blog.mindorks.com/why-do-we-use-the-dependency-injection-framework-in-android](https://blog.mindorks.com/why-do-we-use-the-dependency-injection-framework-in-android)


이 경우, Downloader 객체의 인스턴스를 생성한다고 하면 다음과 같은 과정이 필요합니다.

```kotlin
val executor = Executor()
val httpClient = HTTPClient()
val request = Request(executor, httpClient)

val downloader = Downloader(request)
```

activity A와 B에서 Downloader 객체를 사용한다고 하면, 매번 위의 코드를 작성 해줘야 합니다. 다른 컴포넌트에서도 Downloader 객체가 필요하다면 매번 위의 코드를 추가해 주어야 하고, 
Downloader 객체의 구현이 바뀐다면? 프로젝트 전체를 뒤지면서 코드를 새로 작성해야 하는 문제가 발생합니다.<br><br>

그래서, 이와 같은 boilerplate 코드를 줄이기 위해서 Factory 패턴을 적용하는 것도 하나의 방법이 될 수 있습니다.

```kotlin
val downloader = DownloaderFactory.create()
```

factory에 해당하는 코드는 아래와 같습니다.

```kotlin
object DownloaderFactory {
	fun create(): Downloader {
			val executor = Executor()
			val httpClient = HTTPClient()
			val request = Request(executor, httpClient)
			return Downloader(request
	}
} 
```

<br>

### 매번 직접 Factory class를 만드는 대신, 누가 대신 만들어 준다면?

factory class를 이용해서 객체를 생성하는 방법도 이전보다는 효율적 이긴 하지만, 이조차도 프로젝트가 커지고, 객체 간에 관계가 복잡해 진다면, 관리하는데 어려움이 따를 것 입니다.

Dagger는 이 과정을 개발자가 manual하게 하지 않아도 되게 해주기 때문에 상당한 효율을 가져오게 됩니다. 

이 뿐만 아니라 Dagger는 몇 가지 장점을 더 가집니다.

1. *scope*를 사용해서 객체를 재사용할지, 새롭게 생성할지 결정해 줍니다.
2. 컴파일 타임에 종속성 그래프를 만들기 때문에 런타임 에러를 방지할 수 있습니다.
3. unit 테스트와 integeration 테스트에 용이합니다.
4. 생성된 코드는 직접 작성한 것 처럼 보기 쉽고, 디버깅에 용이합니다.

<br>

---
Dagger는 `@Inject`,`@Provides` 같은 annotation 별 역할, module과 component 간의 관계, Scope에 대한 이해 등 초기 단계에서의 러닝 커브가 꽤 큰 편입니다.

하지만, 프로젝트 관리에서 분명한 이점을 가지고 있기에 장기적 관점에서 알고 가야 할 필요가 충분히 있는 라이브러리입니다.
<br><br>

### Reference

- [https://developer.android.com/training/dependency-injection/dagger-basics](https://developer.android.com/training/dependency-injection/dagger-basics)
- [https://android.jlelse.eu/dagger-2-part-i-basic-principles-graph-dependencies-scopes-3dfd032ccd82](https://android.jlelse.eu/dagger-2-part-i-basic-principles-graph-dependencies-scopes-3dfd032ccd82)
- [https://blog.mindorks.com/why-do-we-use-the-dependency-injection-framework-in-android](https://blog.mindorks.com/why-do-we-use-the-dependency-injection-framework-in-android)