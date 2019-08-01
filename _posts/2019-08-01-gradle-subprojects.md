---
title: Gradle subprojects
layout: post
---

`:java-groovy-core` :
```groovy
dependencies {
  implementation("org.springframework.boot:spring-boot-starter")
}
```

`:java-groovy-test-util` :
```groovy
dependencies {
  implementation(project(":java-groovy-core"))
  implementation("org.springframework.boot:spring-boot-starter")
}
```

`./gradlew clean test`
```bash
➜  java-groovy git:(gradle-subprojects) ✗ ./gradlew clean test

Welcome to Gradle 5.5.1!

Here are the highlights of this release:
 - Kickstart Gradle plugin development with gradle init
 - Distribute organization-wide Gradle properties in custom Gradle distributions
 - Transform dependency artifacts on resolution

For more details see https://docs.gradle.org/5.5.1/release-notes.html


BUILD SUCCESSFUL in 5s
9 actionable tasks: 9 executed
```

JUnit test config :

![JUnit test config]({{ base.url }}/assets/gradle-subprojects/java-groovy/IntelliJ_JUnit_config.png)

JUnit test 실행 결과 :

```
java.lang.IllegalStateException: Failed to load ApplicationContext

	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:125)
	at org.springframework.test.context.support.DefaultTestContext.getApplicationContext(DefaultTestContext.java:108)
	at org.springframework.test.context.support.DependencyInjectionTestExecutionListener.injectDependencies(DependencyInjectionTestExecutionListener.java:118)
	at org.springframework.test.context.support.DependencyInjectionTestExecutionListener.prepareTestInstance(DependencyInjectionTestExecutionListener.java:83)
	at org.springframework.boot.test.autoconfigure.SpringBootDependencyInjectionTestExecutionListener.prepareTestInstance(SpringBootDependencyInjectionTestExecutionListener.java:43)
	at org.springframework.test.context.TestContextManager.prepareTestInstance(TestContextManager.java:246)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.createTest(SpringJUnit4ClassRunner.java:227)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner$1.runReflectiveCall(SpringJUnit4ClassRunner.java:289)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.methodBlock(SpringJUnit4ClassRunner.java:291)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.runChild(SpringJUnit4ClassRunner.java:246)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.runChild(SpringJUnit4ClassRunner.java:97)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.springframework.test.context.junit4.statements.RunBeforeTestClassCallbacks.evaluate(RunBeforeTestClassCallbacks.java:61)
	at org.springframework.test.context.junit4.statements.RunAfterTestClassCallbacks.evaluate(RunAfterTestClassCallbacks.java:70)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.run(SpringJUnit4ClassRunner.java:190)
	at org.junit.runners.Suite.runChild(Suite.java:128)
	at org.junit.runners.Suite.runChild(Suite.java:27)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
	at com.intellij.rt.execution.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:47)
	at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:242)
	at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:70)
Caused by: org.springframework.beans.factory.support.BeanDefinitionOverrideException: Invalid bean definition with name 'generalUtil' defined in kr.lul.pages.gradle.subprojects.java.groovy.test.util.TestUtilTestConfiguration: Cannot register bean definition [Root bean: class [null]; scope=; abstract=false; lazyInit=false; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=testUtilTestConfiguration; factoryMethodName=generalUtil; initMethodName=null; destroyMethodName=(inferred); defined in kr.lul.pages.gradle.subprojects.java.groovy.test.util.TestUtilTestConfiguration] for bean 'generalUtil': There is already [Root bean: class [null]; scope=; abstract=false; lazyInit=false; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=coreModuleTestConfiguration; factoryMethodName=generalUtil; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [kr/lul/pages/gradle/subprojects/java/groovy/core/CoreModuleTestConfiguration.class]] bound.
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.registerBeanDefinition(DefaultListableBeanFactory.java:893)
	at org.springframework.context.annotation.ConfigurationClassBeanDefinitionReader.loadBeanDefinitionsForBeanMethod(ConfigurationClassBeanDefinitionReader.java:274)
	at org.springframework.context.annotation.ConfigurationClassBeanDefinitionReader.loadBeanDefinitionsForConfigurationClass(ConfigurationClassBeanDefinitionReader.java:141)
	at org.springframework.context.annotation.ConfigurationClassBeanDefinitionReader.loadBeanDefinitions(ConfigurationClassBeanDefinitionReader.java:117)
	at org.springframework.context.annotation.ConfigurationClassPostProcessor.processConfigBeanDefinitions(ConfigurationClassPostProcessor.java:327)
	at org.springframework.context.annotation.ConfigurationClassPostProcessor.postProcessBeanDefinitionRegistry(ConfigurationClassPostProcessor.java:232)
	at org.springframework.context.support.PostProcessorRegistrationDelegate.invokeBeanDefinitionRegistryPostProcessors(PostProcessorRegistrationDelegate.java:275)
	at org.springframework.context.support.PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(PostProcessorRegistrationDelegate.java:95)
	at org.springframework.context.support.AbstractApplicationContext.invokeBeanFactoryPostProcessors(AbstractApplicationContext.java:705)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:531)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:742)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:389)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:311)
	at org.springframework.boot.test.context.SpringBootContextLoader.loadContext(SpringBootContextLoader.java:119)
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContextInternal(DefaultCacheAwareContextLoaderDelegate.java:99)
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:117)
	... 33 more
```

에러 내용은 `kr.lul.pages.gradle.subprojects.java.groovy.core.CoreModuleTestConfiguration`가 `kr.lul.pages.gradle.subprojects.java.groovy.util.GeneralUtil` bean을 이미 만들었는데 `kr.lul.pages.gradle.subprojects.java.groovy.test.util.TestUtilModuleTestConfiguration`에서 또 만드려고 한다는 거다.

`java-groovy-test-util` 모듈(서브프로젝트)의 테스트 코드가 `java-groovy-core` 모듈의 테스트 코드를 보고 있다는 뜻이다.

![`CoreModuleTestConfiguration` 접근 코드.]({{ base.url }}/assets/gradle-subprojects/java-groovy/test_code_access_code.png)

![`CoreModuleTestConfiguration` 접근 실패.]({{ base.url }}/assets/gradle-subprojects/java-groovy/test_code_access_fail.png)

그런데 이렇게 소스코드에서는 접근하지 못한다.