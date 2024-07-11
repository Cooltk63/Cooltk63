
2024-07-11T13:16:39.157+05:30  INFO 2688 --- [CS] [  restartedMain] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2024-07-11T13:16:39.376+05:30  INFO 2688 --- [CS] [  restartedMain] o.s.d.j.r.query.QueryEnhancerFactory     : Hibernate is in classpath; If applicable, HQL parser will be used.
2024-07-11T13:16:39.434+05:30 ERROR 2688 --- [CS] [  restartedMain] o.s.b.web.embedded.tomcat.TomcatStarter  : Error starting Tomcat context. Exception: org.springframework.beans.factory.UnsatisfiedDependencyException. Message: Error creating bean with name 'jwtFilter': Unsatisfied dependency expressed through field 'jwtUtil': Error creating bean with name 'jwtUtil': Unsatisfied dependency expressed through field 'userDetailsService': Error creating bean with name 'customUserDetailsService': Unsatisfied dependency expressed through field 'userMasterRepository': Error creating bean with name 'userMasterRepository' defined in com.crs.cs.Repository.UserMasterRepository defined in @EnableJpaRepositories declared on CsApplication: Could not create query for public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); Reason: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
2024-07-11T13:16:39.462+05:30  INFO 2688 --- [CS] [  restartedMain] o.apache.catalina.core.StandardService   : Stopping service [Tomcat]
2024-07-11T13:16:39.464+05:30  WARN 2688 --- [CS] [  restartedMain] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [oracle.jdbc.driver.BlockSource.ThreadedCachingBlockSource.BlockReleaser] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.base/jdk.internal.misc.Unsafe.park(Native Method)
 java.base/java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:269)
 java.base/java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:1758)
 oracle.jdbc.internal.Monitor$WaitableMonitor.monitorWait(Monitor.java:258)
 oracle.jdbc.internal.Monitor$WaitableMonitor.monitorWait(Monitor.java:240)
 oracle.jdbc.driver.BlockSource$ThreadedCachingBlockSource$BlockReleaser.run(BlockSource.java:345)
2024-07-11T13:16:39.464+05:30  WARN 2688 --- [CS] [  restartedMain] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [InterruptTimer] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.base/java.lang.Object.wait0(Native Method)
 java.base/java.lang.Object.wait(Object.java:375)
 java.base/java.util.TimerThread.mainLoop(Timer.java:568)
 java.base/java.util.TimerThread.run(Timer.java:521)
2024-07-11T13:16:39.464+05:30  WARN 2688 --- [CS] [  restartedMain] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [OJDBC-WORKER-THREAD-1] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.base/jdk.internal.misc.Unsafe.park(Native Method)
 java.base/java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:410)
 java.base/java.util.concurrent.LinkedTransferQueue$DualNode.await(LinkedTransferQueue.java:452)
 java.base/java.util.concurrent.SynchronousQueue$Transferer.xferLifo(SynchronousQueue.java:194)
 java.base/java.util.concurrent.SynchronousQueue.xfer(SynchronousQueue.java:233)
 java.base/java.util.concurrent.SynchronousQueue.poll(SynchronousQueue.java:336)
 java.base/java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1069)
 java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130)
 java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:642)
 java.base/java.lang.Thread.run(Thread.java:1570)
2024-07-11T13:16:39.464+05:30  WARN 2688 --- [CS] [  restartedMain] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [HikariPool-1 housekeeper] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.base/jdk.internal.misc.Unsafe.park(Native Method)
 java.base/java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:269)
 java.base/java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:1758)
 java.base/java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(ScheduledThreadPoolExecutor.java:1182)
 java.base/java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(ScheduledThreadPoolExecutor.java:899)
 java.base/java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1070)
 java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130)
 java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:642)
 java.base/java.lang.Thread.run(Thread.java:1570)
2024-07-11T13:16:39.465+05:30  WARN 2688 --- [CS] [  restartedMain] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [HikariPool-1 connection adder] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
 java.base/jdk.internal.misc.Unsafe.park(Native Method)
 java.base/java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:269)
 java.base/java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:1758)
 java.base/java.util.concurrent.LinkedBlockingQueue.poll(LinkedBlockingQueue.java:460)
 java.base/java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1069)
 java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130)
 java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:642)
 java.base/java.lang.Thread.run(Thread.java:1570)
2024-07-11T13:16:39.466+05:30  WARN 2688 --- [CS] [  restartedMain] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.context.ApplicationContextException: Unable to start web server
2024-07-11T13:16:39.467+05:30  INFO 2688 --- [CS] [  restartedMain] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
2024-07-11T13:16:39.469+05:30  INFO 2688 --- [CS] [  restartedMain] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
2024-07-11T13:16:39.504+05:30  INFO 2688 --- [CS] [  restartedMain] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
2024-07-11T13:16:39.515+05:30  INFO 2688 --- [CS] [  restartedMain] .s.b.a.l.ConditionEvaluationReportLogger : 

Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
2024-07-11T13:16:39.528+05:30 ERROR 2688 --- [CS] [  restartedMain] o.s.boot.SpringApplication               : Application run failed

org.springframework.context.ApplicationContextException: Unable to start web server
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:165) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:618) ~[spring-context-6.1.8.jar:6.1.8]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:754) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:456) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:335) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1363) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1352) ~[spring-boot-3.3.0.jar:3.3.0]
	at com.crs.cs.CsApplication.main(CsApplication.java:13) ~[classes/:na]
	at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103) ~[na:na]
	at java.base/java.lang.reflect.Method.invoke(Method.java:580) ~[na:na]
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:50) ~[spring-boot-devtools-3.3.0.jar:3.3.0]
Caused by: org.springframework.boot.web.server.WebServerException: Unable to start embedded Tomcat
	at org.springframework.boot.web.embedded.tomcat.TomcatWebServer.initialize(TomcatWebServer.java:147) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatWebServer.<init>(TomcatWebServer.java:107) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory.getTomcatWebServer(TomcatServletWebServerFactory.java:516) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory.getWebServer(TomcatServletWebServerFactory.java:222) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.createWebServer(ServletWebServerApplicationContext.java:188) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.onRefresh(ServletWebServerApplicationContext.java:162) ~[spring-boot-3.3.0.jar:3.3.0]
	... 11 common frames omitted
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'jwtFilter': Unsatisfied dependency expressed through field 'jwtUtil': Error creating bean with name 'jwtUtil': Unsatisfied dependency expressed through field 'userDetailsService': Error creating bean with name 'customUserDetailsService': Unsatisfied dependency expressed through field 'userMasterRepository': Error creating bean with name 'userMasterRepository' defined in com.crs.cs.Repository.UserMasterRepository defined in @EnableJpaRepositories declared on CsApplication: Could not create query for public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); Reason: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:787) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:767) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:145) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:508) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1421) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:599) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:522) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:337) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:335) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:205) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.getOrderedBeansOfType(ServletContextInitializerBeans.java:211) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.addAsRegistrationBean(ServletContextInitializerBeans.java:174) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.addAsRegistrationBean(ServletContextInitializerBeans.java:169) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.addAdaptableBeans(ServletContextInitializerBeans.java:154) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.<init>(ServletContextInitializerBeans.java:87) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.getServletContextInitializerBeans(ServletWebServerApplicationContext.java:266) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.selfInitialize(ServletWebServerApplicationContext.java:240) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.springframework.boot.web.embedded.tomcat.TomcatStarter.onStartup(TomcatStarter.java:52) ~[spring-boot-3.3.0.jar:3.3.0]
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:4414) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:171) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1203) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1193) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:317) ~[na:na]
	at org.apache.tomcat.util.threads.InlineExecutorService.execute(InlineExecutorService.java:75) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at java.base/java.util.concurrent.AbstractExecutorService.submit(AbstractExecutorService.java:145) ~[na:na]
	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:749) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.StandardHost.startInternal(StandardHost.java:772) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:171) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1203) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1193) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:317) ~[na:na]
	at org.apache.tomcat.util.threads.InlineExecutorService.execute(InlineExecutorService.java:75) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at java.base/java.util.concurrent.AbstractExecutorService.submit(AbstractExecutorService.java:145) ~[na:na]
	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:749) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.StandardEngine.startInternal(StandardEngine.java:203) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:171) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.StandardService.startInternal(StandardService.java:415) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:171) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.core.StandardServer.startInternal(StandardServer.java:874) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:171) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.apache.catalina.startup.Tomcat.start(Tomcat.java:437) ~[tomcat-embed-core-10.1.24.jar:10.1.24]
	at org.springframework.boot.web.embedded.tomcat.TomcatWebServer.initialize(TomcatWebServer.java:128) ~[spring-boot-3.3.0.jar:3.3.0]
	... 16 common frames omitted
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'jwtUtil': Unsatisfied dependency expressed through field 'userDetailsService': Error creating bean with name 'customUserDetailsService': Unsatisfied dependency expressed through field 'userMasterRepository': Error creating bean with name 'userMasterRepository' defined in com.crs.cs.Repository.UserMasterRepository defined in @EnableJpaRepositories declared on CsApplication: Could not create query for public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); Reason: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:787) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:767) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:145) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:508) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1421) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:599) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:522) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:337) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:335) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:200) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1443) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1353) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:784) ~[spring-beans-6.1.8.jar:6.1.8]
	... 58 common frames omitted
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'customUserDetailsService': Unsatisfied dependency expressed through field 'userMasterRepository': Error creating bean with name 'userMasterRepository' defined in com.crs.cs.Repository.UserMasterRepository defined in @EnableJpaRepositories declared on CsApplication: Could not create query for public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); Reason: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:787) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:767) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:145) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:508) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1421) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:599) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:522) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:337) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:335) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:200) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1443) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1353) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:784) ~[spring-beans-6.1.8.jar:6.1.8]
	... 72 common frames omitted
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userMasterRepository' defined in com.crs.cs.Repository.UserMasterRepository defined in @EnableJpaRepositories declared on CsApplication: Could not create query for public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); Reason: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1788) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:600) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:522) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:337) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:335) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:200) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1443) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1353) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.resolveFieldValue(AutowiredAnnotationBeanPostProcessor.java:784) ~[spring-beans-6.1.8.jar:6.1.8]
	... 86 common frames omitted
Caused by: org.springframework.data.repository.query.QueryCreationException: Could not create query for public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); Reason: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
	at org.springframework.data.repository.query.QueryCreationException.create(QueryCreationException.java:101) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.QueryExecutorMethodInterceptor.lookupQuery(QueryExecutorMethodInterceptor.java:115) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.QueryExecutorMethodInterceptor.mapMethodsToQuery(QueryExecutorMethodInterceptor.java:99) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.QueryExecutorMethodInterceptor.lambda$new$0(QueryExecutorMethodInterceptor.java:88) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at java.base/java.util.Optional.map(Optional.java:260) ~[na:na]
	at org.springframework.data.repository.core.support.QueryExecutorMethodInterceptor.<init>(QueryExecutorMethodInterceptor.java:88) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.RepositoryFactorySupport.getRepository(RepositoryFactorySupport.java:357) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.RepositoryFactoryBeanSupport.lambda$afterPropertiesSet$5(RepositoryFactoryBeanSupport.java:286) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.util.Lazy.getNullable(Lazy.java:135) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.util.Lazy.get(Lazy.java:113) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.RepositoryFactoryBeanSupport.afterPropertiesSet(RepositoryFactoryBeanSupport.java:292) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.jpa.repository.support.JpaRepositoryFactoryBean.afterPropertiesSet(JpaRepositoryFactoryBean.java:132) ~[spring-data-jpa-3.3.0.jar:3.3.0]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1835) ~[spring-beans-6.1.8.jar:6.1.8]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1784) ~[spring-beans-6.1.8.jar:6.1.8]
	... 96 common frames omitted
Caused by: java.lang.IllegalArgumentException: Failed to create query for method public abstract com.crs.cs.Model.UserMaster com.crs.cs.Repository.UserMasterRepository.findByPf_number(int); No property 'pf' found for type 'UserMaster'
	at org.springframework.data.jpa.repository.query.PartTreeJpaQuery.<init>(PartTreeJpaQuery.java:106) ~[spring-data-jpa-3.3.0.jar:3.3.0]
	at org.springframework.data.jpa.repository.query.JpaQueryLookupStrategy$CreateQueryLookupStrategy.resolveQuery(JpaQueryLookupStrategy.java:124) ~[spring-data-jpa-3.3.0.jar:3.3.0]
	at org.springframework.data.jpa.repository.query.JpaQueryLookupStrategy$CreateIfNotFoundQueryLookupStrategy.resolveQuery(JpaQueryLookupStrategy.java:258) ~[spring-data-jpa-3.3.0.jar:3.3.0]
	at org.springframework.data.jpa.repository.query.JpaQueryLookupStrategy$AbstractQueryLookupStrategy.resolveQuery(JpaQueryLookupStrategy.java:95) ~[spring-data-jpa-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.core.support.QueryExecutorMethodInterceptor.lookupQuery(QueryExecutorMethodInterceptor.java:111) ~[spring-data-commons-3.3.0.jar:3.3.0]
	... 108 common frames omitted
Caused by: org.springframework.data.mapping.PropertyReferenceException: No property 'pf' found for type 'UserMaster'
	at org.springframework.data.mapping.PropertyPath.<init>(PropertyPath.java:94) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.mapping.PropertyPath.create(PropertyPath.java:455) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.mapping.PropertyPath.create(PropertyPath.java:431) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.mapping.PropertyPath.lambda$from$0(PropertyPath.java:384) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at java.base/java.util.concurrent.ConcurrentMap.computeIfAbsent(ConcurrentMap.java:330) ~[na:na]
	at org.springframework.data.mapping.PropertyPath.from(PropertyPath.java:366) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.mapping.PropertyPath.from(PropertyPath.java:344) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.query.parser.Part.<init>(Part.java:81) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.query.parser.PartTree$OrPart.lambda$new$0(PartTree.java:259) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:212) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:194) ~[na:na]
	at java.base/java.util.Spliterators$ArraySpliterator.forEachRemaining(Spliterators.java:1024) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:556) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:546) ~[na:na]
	at java.base/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:921) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:265) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.collect(ReferencePipeline.java:702) ~[na:na]
	at org.springframework.data.repository.query.parser.PartTree$OrPart.<init>(PartTree.java:260) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.query.parser.PartTree$Predicate.lambda$new$0(PartTree.java:389) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at java.base/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:212) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:194) ~[na:na]
	at java.base/java.util.Spliterators$ArraySpliterator.forEachRemaining(Spliterators.java:1024) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:556) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:546) ~[na:na]
	at java.base/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:921) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:265) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.collect(ReferencePipeline.java:702) ~[na:na]
	at org.springframework.data.repository.query.parser.PartTree$Predicate.<init>(PartTree.java:390) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.repository.query.parser.PartTree.<init>(PartTree.java:103) ~[spring-data-commons-3.3.0.jar:3.3.0]
	at org.springframework.data.jpa.repository.query.PartTreeJpaQuery.<init>(PartTreeJpaQuery.java:100) ~[spring-data-jpa-3.3.0.jar:3.3.0]
	... 112 common frames omitted


Process finished with exit code 0
