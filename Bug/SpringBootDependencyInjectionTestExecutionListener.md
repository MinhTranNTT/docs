# SpringBootDependencyInjectionTestExecutionListener

```
2022-08-30 16:44:49.666 ERROR 11724 --- [           main] o.s.test.context.TestContextManager      : Caught exception while allowing TestExecutionListener [org.springframework.boot.test.autoconfigure.SpringBootDependencyInjectionTestExecutionListener@179ece50] to prepare test instance [com.cy.store.mapper.UserMapperTests@1e141e42]

org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'com.cy.store.mapper.UserMapperTests': Unsatisfied dependency expressed through field 'user'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.cy.store.entity.User' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:659) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:639) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:119) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:399) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1431) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireBeanProperties(AbstractAutowireCapableBeanFactory.java:417) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.test.context.support.DependencyInjectionTestExecutionListener.injectDependencies(DependencyInjectionTestExecutionListener.java:119) ~[spring-test-5.3.22.jar:5.3.22]
	at org.springframework.test.context.support.DependencyInjectionTestExecutionListener.prepareTestInstance(DependencyInjectionTestExecutionListener.java:83) ~[spring-test-5.3.22.jar:5.3.22]
	at org.springframework.boot.test.autoconfigure.SpringBootDependencyInjectionTestExecutionListener.prepareTestInstance(SpringBootDependencyInjectionTestExecutionListener.java:43) ~[spring-boot-test-autoconfigure-2.7.3.jar:2.7.3]
	at org.springframework.test.context.TestContextManager.prepareTestInstance(TestContextManager.java:248) ~[spring-test-5.3.22.jar:5.3.22]
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.createTest(SpringJUnit4ClassRunner.java:227) [spring-test-5.3.22.jar:5.3.22]
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner$1.runReflectiveCall(SpringJUnit4ClassRunner.java:289) [spring-test-5.3.22.jar:5.3.22]
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12) [junit-4.13.2.jar:4.13.2]
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.methodBlock(SpringJUnit4ClassRunner.java:291) [spring-test-5.3.22.jar:5.3.22]
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.runChild(SpringJUnit4ClassRunner.java:246) [spring-test-5.3.22.jar:5.3.22]
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.runChild(SpringJUnit4ClassRunner.java:97) [spring-test-5.3.22.jar:5.3.22]
	at org.junit.runners.ParentRunner$4.run(ParentRunner.java:331) [junit-4.13.2.jar:4.13.2]
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:79) [junit-4.13.2.jar:4.13.2]
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:329) [junit-4.13.2.jar:4.13.2]
	at org.junit.runners.ParentRunner.access$100(ParentRunner.java:66) [junit-4.13.2.jar:4.13.2]
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:293) [junit-4.13.2.jar:4.13.2]
	at org.springframework.test.context.junit4.statements.RunBeforeTestClassCallbacks.evaluate(RunBeforeTestClassCallbacks.java:61) [spring-test-5.3.22.jar:5.3.22]
	at org.springframework.test.context.junit4.statements.RunAfterTestClassCallbacks.evaluate(RunAfterTestClassCallbacks.java:70) [spring-test-5.3.22.jar:5.3.22]
	at org.junit.runners.ParentRunner$3.evaluate(ParentRunner.java:306) [junit-4.13.2.jar:4.13.2]
	at org.junit.runners.ParentRunner.run(ParentRunner.java:413) [junit-4.13.2.jar:4.13.2]
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.run(SpringJUnit4ClassRunner.java:190) [spring-test-5.3.22.jar:5.3.22]
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137) [junit-4.13.2.jar:4.13.2]
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:69) [junit-rt.jar:na]
	at com.intellij.rt.junit.IdeaTestRunner$Repeater$1.execute(IdeaTestRunner.java:38) [junit-rt.jar:na]
	at com.intellij.rt.execution.junit.TestsRepeater.repeat(TestsRepeater.java:11) [idea_rt.jar:na]
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:35) [junit-rt.jar:na]
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:235) [junit-rt.jar:na]
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:54) [junit-rt.jar:na]
Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.cy.store.entity.User' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:1801) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1357) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1311) ~[spring-beans-5.3.22.jar:5.3.22]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:656) ~[spring-beans-5.3.22.jar:5.3.22]
	... 32 common frames omitted


进程已结束,退出代码-1

```

实例化对象，springboot 不能注入类，必须通过接口依赖注入
