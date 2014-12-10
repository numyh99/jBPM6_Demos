rewards-basic
=============

This is an example web application for jBPM 6 which is developed by Jiri Svitak in his GitHub repository [https://github.com/jsvitak/jbpm-6-examples/tree/master/rewards-basic]. It was created by forking the rewards-basic application by Toshiya Kobayashi for jBPM5 earlier.
[https://github.com/tkobayas/jbpm5example/tree/master/rewards-basic]

Also the project structure has changed from Java EE 5 to Java EE 6.

This simple example aims to provide an example usage of:
- Human tasks
- Persistence
- Transactions
- Singleton session manager
- Context dependency injection
- Maven
- [MOST IMPORTANT] In this example "DBUserGroupCallbackImpl" has been used instead of the packaged "RewardsUserGroupCallback" implementation as a "UserGroupCallback" implementation. It has been configured to use "java:jboss/datasources/ExampleDS" datasource configuration which is by default configured in EAP 6.x with H2 database.

### Steps to run
- Make sure you have at least Java 6 and Maven 3 installed
- Download somewhere JBoss EAP 6.1 or 6.2 (BPMS 6.0.3 is based on JBoss EAP 6.1, EAP 6.3 will not work)
- Start the application server (default datasource is ExampleDS, the same as in EAP, so the example works out of the box):
 - cd jboss-eap-6.1/bin
 - ./standalone.sh
- Build and deploy the example application:
 - cd rewards-basic
 - mvn clean package
 - mvn jboss-as:deploy
- Visit http://localhost:8080/rewards-basic/ with a web browser and it would present to you the following options to perform the specific operations.
 - [Start Reward Process] is to start a new process
 - [Jiri's Task] is to list Jiri's tasks and approve them
 - [Mary's Task] is to list Mary's tasks and approve them

- Interestingly, once you start the process you would see the following exception on the terminal.
~~~
02:14:30,418 ERROR [org.jbpm.services.task.identity.DBUserGroupCallbackImpl] (http-localhost.localdomain/127.0.0.1:8080-1) Error when checking user/group in db, parameter: Administrator: org.h2.jdbc.JdbcSQLException: Table "USERS" not found; SQL statement:
select userId from Users where userId = ? [42102-168]
	at org.h2.message.DbException.getJdbcSQLException(DbException.java:329)
	at org.h2.message.DbException.get(DbException.java:169)
	at org.h2.message.DbException.get(DbException.java:146)
	at org.h2.command.Parser.readTableOrView(Parser.java:4770)
	at org.h2.command.Parser.readTableFilter(Parser.java:1084)
	at org.h2.command.Parser.parseSelectSimpleFromPart(Parser.java:1690)
	at org.h2.command.Parser.parseSelectSimple(Parser.java:1797)
	at org.h2.command.Parser.parseSelectSub(Parser.java:1684)
	at org.h2.command.Parser.parseSelectUnion(Parser.java:1527)
	at org.h2.command.Parser.parseSelect(Parser.java:1515)
	at org.h2.command.Parser.parsePrepared(Parser.java:405)
	at org.h2.command.Parser.parse(Parser.java:279)
	at org.h2.command.Parser.parse(Parser.java:251)
	at org.h2.command.Parser.prepareCommand(Parser.java:217)
	at org.h2.engine.Session.prepareLocal(Session.java:415)
	at org.h2.engine.Session.prepareCommand(Session.java:364)
	at org.h2.jdbc.JdbcConnection.prepareCommand(JdbcConnection.java:1109)
	at org.h2.jdbc.JdbcPreparedStatement.<init>(JdbcPreparedStatement.java:74)
	at org.h2.jdbc.JdbcConnection.prepareStatement(JdbcConnection.java:626)
	at org.jboss.jca.adapters.jdbc.BaseWrapperManagedConnection.doPrepareStatement(BaseWrapperManagedConnection.java:738)
	at org.jboss.jca.adapters.jdbc.BaseWrapperManagedConnection.prepareStatement(BaseWrapperManagedConnection.java:724)
	at org.jboss.jca.adapters.jdbc.WrappedConnection.prepareStatement(WrappedConnection.java:405)
	at org.jbpm.services.task.identity.DBUserGroupCallbackImpl.checkExistence(DBUserGroupCallbackImpl.java:171) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.identity.DBUserGroupCallbackImpl.existsUser(DBUserGroupCallbackImpl.java:79) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.UserGroupCallbackTaskCommand.doCallbackUserOperation(UserGroupCallbackTaskCommand.java:80) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.UserGroupCallbackTaskCommand.doCallbackOperationForPeopleAssignments(UserGroupCallbackTaskCommand.java:211) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.AddTaskCommand.execute(AddTaskCommand.java:100) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.AddTaskCommand.execute(AddTaskCommand.java:53) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.TaskCommandExecutorImpl$SelfExecutionCommandService.execute(TaskCommandExecutorImpl.java:65) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.persistence.TaskTransactionInterceptor.execute(TaskTransactionInterceptor.java:54) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.TaskCommandExecutorImpl.execute(TaskCommandExecutorImpl.java:40) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.impl.command.CommandBasedTaskService.addTask(CommandBasedTaskService.java:471) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.runtime.manager.impl.task.SynchronizedTaskService.addTask(SynchronizedTaskService.java:456) [jbpm-runtime-manager-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.wih.LocalHTWorkItemHandler.executeWorkItem(LocalHTWorkItemHandler.java:61) [jbpm-human-task-workitems-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.jpa.processinstance.JPAWorkItemManager.internalExecuteWorkItem(JPAWorkItemManager.java:56) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.WorkItemNodeInstance.internalTrigger(WorkItemNodeInstance.java:128) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.trigger(NodeInstanceImpl.java:155) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.triggerNodeInstance(NodeInstanceImpl.java:347) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.triggerCompleted(NodeInstanceImpl.java:306) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.StartNodeInstance.triggerCompleted(StartNodeInstance.java:66) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.StartNodeInstance.internalTrigger(StartNodeInstance.java:43) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.trigger(NodeInstanceImpl.java:155) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.ruleflow.instance.RuleFlowProcessInstance.internalStart(RuleFlowProcessInstance.java:35) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.impl.ProcessInstanceImpl.start(ProcessInstanceImpl.java:226) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.WorkflowProcessInstanceImpl.start(WorkflowProcessInstanceImpl.java:363) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcessInstance(ProcessRuntimeImpl.java:194) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcess(ProcessRuntimeImpl.java:176) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcess(ProcessRuntimeImpl.java:168) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.common.AbstractWorkingMemory.startProcess(AbstractWorkingMemory.java:1575) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.impl.StatefulKnowledgeSessionImpl.startProcess(StatefulKnowledgeSessionImpl.java:366) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.runtime.process.StartProcessCommand.execute(StartProcessCommand.java:121) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.runtime.process.StartProcessCommand.execute(StartProcessCommand.java:40) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.DefaultCommandService.execute(DefaultCommandService.java:36) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.SingleSessionCommandService$TransactionInterceptor.execute(SingleSessionCommandService.java:533) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.jpa.OptimisticLockRetryInterceptor.execute(OptimisticLockRetryInterceptor.java:73) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.SingleSessionCommandService.execute(SingleSessionCommandService.java:377) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.CommandBasedStatefulKnowledgeSession.startProcess(CommandBasedStatefulKnowledgeSession.java:232) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.examples.ejb.ProcessBean.startProcess(ProcessBean.java:72) [classes:]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) [rt.jar:1.7.0_55]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57) [rt.jar:1.7.0_55]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) [rt.jar:1.7.0_55]
	at java.lang.reflect.Method.invoke(Method.java:606) [rt.jar:1.7.0_55]
	at org.jboss.as.ee.component.ManagedReferenceMethodInterceptorFactory$ManagedReferenceMethodInterceptor.processInvocation(ManagedReferenceMethodInterceptorFactory.java:72) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.WeavedInterceptor.processInvocation(WeavedInterceptor.java:53) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:374) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.weld.ejb.Jsr299BindingsInterceptor.doMethodInterception(Jsr299BindingsInterceptor.java:129) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.weld.ejb.Jsr299BindingsInterceptor.processInvocation(Jsr299BindingsInterceptor.java:137) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.WeavedInterceptor.processInvocation(WeavedInterceptor.java:53) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.invocationmetrics.ExecutionTimeInterceptor.processInvocation(ExecutionTimeInterceptor.java:43) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:374) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.concurrency.ContainerManagedConcurrencyInterceptor.processInvocation(ContainerManagedConcurrencyInterceptor.java:104) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.tx.EjbBMTInterceptor.handleInvocation(EjbBMTInterceptor.java:104) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ejb3.tx.BMTInterceptor.processInvocation(BMTInterceptor.java:56) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.weld.ejb.EjbRequestScopeActivationInterceptor.processInvocation(EjbRequestScopeActivationInterceptor.java:74) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InitialInterceptor.processInvocation(InitialInterceptor.java:21) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.ComponentDispatcherInterceptor.processInvocation(ComponentDispatcherInterceptor.java:53) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.singleton.SingletonComponentInstanceAssociationInterceptor.processInvocation(SingletonComponentInstanceAssociationInterceptor.java:52) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.CurrentInvocationContextInterceptor.processInvocation(CurrentInvocationContextInterceptor.java:41) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.ShutDownInterceptorFactory$1.processInvocation(ShutDownInterceptorFactory.java:64) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.LoggingInterceptor.processInvocation(LoggingInterceptor.java:59) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.NamespaceContextInterceptor.processInvocation(NamespaceContextInterceptor.java:50) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.AdditionalSetupInterceptor.processInvocation(AdditionalSetupInterceptor.java:55) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.TCCLInterceptor.processInvocation(TCCLInterceptor.java:45) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.ViewService$View.invoke(ViewService.java:165) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ee.component.ViewDescription$1.processInvocation(ViewDescription.java:182) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.ProxyInvocationHandler.invoke(ProxyInvocationHandler.java:72) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jbpm.examples.ejb.ProcessLocal$$$view3.startProcess(Unknown Source) [classes:]
	at org.jbpm.examples.web.ProcessServlet.doPost(ProcessServlet.java:46) [classes:]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:754) [jboss-servlet-api_3.0_spec-1.0.2.Final-redhat-1.jar:1.0.2.Final-redhat-1]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:847) [jboss-servlet-api_3.0_spec-1.0.2.Final-redhat-1.jar:1.0.2.Final-redhat-1]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) [rt.jar:1.7.0_55]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57) [rt.jar:1.7.0_55]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) [rt.jar:1.7.0_55]
	at java.lang.reflect.Method.invoke(Method.java:606) [rt.jar:1.7.0_55]
	at org.apache.catalina.security.SecurityUtil$1.run(SecurityUtil.java:263) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.security.SecurityUtil$1.run(SecurityUtil.java:261) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.security.AccessController.doPrivileged(Native Method) [rt.jar:1.7.0_55]
	at javax.security.auth.Subject.doAsPrivileged(Subject.java:536) [rt.jar:1.7.0_55]
	at org.apache.catalina.security.SecurityUtil.execute(SecurityUtil.java:295) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.security.SecurityUtil.doAsPrivilege(SecurityUtil.java:155) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:288) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain.access$000(ApplicationFilterChain.java:59) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain$1.run(ApplicationFilterChain.java:197) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.security.AccessController.doPrivileged(Native Method) [rt.jar:1.7.0_55]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:193) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:230) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:149) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.jboss.as.jpa.interceptor.WebNonTxEmCloserValve.invoke(WebNonTxEmCloserValve.java:50) [jboss-as-jpa-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.jpa.interceptor.WebNonTxEmCloserValve.invoke(WebNonTxEmCloserValve.java:50) [jboss-as-jpa-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.web.security.SecurityContextAssociationValve.invoke(SecurityContextAssociationValve.java:169) [jboss-as-web-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:145) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:97) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.authenticator.SingleSignOn.invoke(SingleSignOn.java:389) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:102) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:336) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.coyote.http11.Http11Processor.process(Http11Processor.java:856) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler.process(Http11Protocol.java:653) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.tomcat.util.net.JIoEndpoint$Worker.run(JIoEndpoint.java:920) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.lang.Thread.run(Thread.java:745) [rt.jar:1.7.0_55]

02:14:30,450 ERROR [org.jbpm.services.task.identity.DBUserGroupCallbackImpl] (http-localhost.localdomain/127.0.0.1:8080-1) Error when checking user/group in db, parameter: Administrators: org.h2.jdbc.JdbcSQLException: Table "GROUPS" not found; SQL statement:
select groupId from Groups where groupId = ? [42102-168]
	at org.h2.message.DbException.getJdbcSQLException(DbException.java:329)
	at org.h2.message.DbException.get(DbException.java:169)
	at org.h2.message.DbException.get(DbException.java:146)
	at org.h2.command.Parser.readTableOrView(Parser.java:4770)
	at org.h2.command.Parser.readTableFilter(Parser.java:1084)
	at org.h2.command.Parser.parseSelectSimpleFromPart(Parser.java:1690)
	at org.h2.command.Parser.parseSelectSimple(Parser.java:1797)
	at org.h2.command.Parser.parseSelectSub(Parser.java:1684)
	at org.h2.command.Parser.parseSelectUnion(Parser.java:1527)
	at org.h2.command.Parser.parseSelect(Parser.java:1515)
	at org.h2.command.Parser.parsePrepared(Parser.java:405)
	at org.h2.command.Parser.parse(Parser.java:279)
	at org.h2.command.Parser.parse(Parser.java:251)
	at org.h2.command.Parser.prepareCommand(Parser.java:217)
	at org.h2.engine.Session.prepareLocal(Session.java:415)
	at org.h2.engine.Session.prepareCommand(Session.java:364)
	at org.h2.jdbc.JdbcConnection.prepareCommand(JdbcConnection.java:1109)
	at org.h2.jdbc.JdbcPreparedStatement.<init>(JdbcPreparedStatement.java:74)
	at org.h2.jdbc.JdbcConnection.prepareStatement(JdbcConnection.java:626)
	at org.jboss.jca.adapters.jdbc.BaseWrapperManagedConnection.doPrepareStatement(BaseWrapperManagedConnection.java:738)
	at org.jboss.jca.adapters.jdbc.BaseWrapperManagedConnection.prepareStatement(BaseWrapperManagedConnection.java:724)
	at org.jboss.jca.adapters.jdbc.WrappedConnection.prepareStatement(WrappedConnection.java:405)
	at org.jbpm.services.task.identity.DBUserGroupCallbackImpl.checkExistence(DBUserGroupCallbackImpl.java:171) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.identity.DBUserGroupCallbackImpl.existsGroup(DBUserGroupCallbackImpl.java:86) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.UserGroupCallbackTaskCommand.doCallbackGroupOperation(UserGroupCallbackTaskCommand.java:90) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.UserGroupCallbackTaskCommand.doCallbackOperationForPeopleAssignments(UserGroupCallbackTaskCommand.java:217) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.AddTaskCommand.execute(AddTaskCommand.java:100) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.AddTaskCommand.execute(AddTaskCommand.java:53) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.TaskCommandExecutorImpl$SelfExecutionCommandService.execute(TaskCommandExecutorImpl.java:65) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.persistence.TaskTransactionInterceptor.execute(TaskTransactionInterceptor.java:54) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.TaskCommandExecutorImpl.execute(TaskCommandExecutorImpl.java:40) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.impl.command.CommandBasedTaskService.addTask(CommandBasedTaskService.java:471) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.runtime.manager.impl.task.SynchronizedTaskService.addTask(SynchronizedTaskService.java:456) [jbpm-runtime-manager-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.wih.LocalHTWorkItemHandler.executeWorkItem(LocalHTWorkItemHandler.java:61) [jbpm-human-task-workitems-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.jpa.processinstance.JPAWorkItemManager.internalExecuteWorkItem(JPAWorkItemManager.java:56) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.WorkItemNodeInstance.internalTrigger(WorkItemNodeInstance.java:128) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.trigger(NodeInstanceImpl.java:155) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.triggerNodeInstance(NodeInstanceImpl.java:347) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.triggerCompleted(NodeInstanceImpl.java:306) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.StartNodeInstance.triggerCompleted(StartNodeInstance.java:66) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.StartNodeInstance.internalTrigger(StartNodeInstance.java:43) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.trigger(NodeInstanceImpl.java:155) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.ruleflow.instance.RuleFlowProcessInstance.internalStart(RuleFlowProcessInstance.java:35) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.impl.ProcessInstanceImpl.start(ProcessInstanceImpl.java:226) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.WorkflowProcessInstanceImpl.start(WorkflowProcessInstanceImpl.java:363) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcessInstance(ProcessRuntimeImpl.java:194) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcess(ProcessRuntimeImpl.java:176) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcess(ProcessRuntimeImpl.java:168) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.common.AbstractWorkingMemory.startProcess(AbstractWorkingMemory.java:1575) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.impl.StatefulKnowledgeSessionImpl.startProcess(StatefulKnowledgeSessionImpl.java:366) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.runtime.process.StartProcessCommand.execute(StartProcessCommand.java:121) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.runtime.process.StartProcessCommand.execute(StartProcessCommand.java:40) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.DefaultCommandService.execute(DefaultCommandService.java:36) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.SingleSessionCommandService$TransactionInterceptor.execute(SingleSessionCommandService.java:533) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.jpa.OptimisticLockRetryInterceptor.execute(OptimisticLockRetryInterceptor.java:73) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.SingleSessionCommandService.execute(SingleSessionCommandService.java:377) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.CommandBasedStatefulKnowledgeSession.startProcess(CommandBasedStatefulKnowledgeSession.java:232) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.examples.ejb.ProcessBean.startProcess(ProcessBean.java:72) [classes:]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) [rt.jar:1.7.0_55]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57) [rt.jar:1.7.0_55]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) [rt.jar:1.7.0_55]
	at java.lang.reflect.Method.invoke(Method.java:606) [rt.jar:1.7.0_55]
	at org.jboss.as.ee.component.ManagedReferenceMethodInterceptorFactory$ManagedReferenceMethodInterceptor.processInvocation(ManagedReferenceMethodInterceptorFactory.java:72) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.WeavedInterceptor.processInvocation(WeavedInterceptor.java:53) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:374) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.weld.ejb.Jsr299BindingsInterceptor.doMethodInterception(Jsr299BindingsInterceptor.java:129) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.weld.ejb.Jsr299BindingsInterceptor.processInvocation(Jsr299BindingsInterceptor.java:137) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.WeavedInterceptor.processInvocation(WeavedInterceptor.java:53) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.invocationmetrics.ExecutionTimeInterceptor.processInvocation(ExecutionTimeInterceptor.java:43) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:374) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.concurrency.ContainerManagedConcurrencyInterceptor.processInvocation(ContainerManagedConcurrencyInterceptor.java:104) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.tx.EjbBMTInterceptor.handleInvocation(EjbBMTInterceptor.java:104) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ejb3.tx.BMTInterceptor.processInvocation(BMTInterceptor.java:56) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.weld.ejb.EjbRequestScopeActivationInterceptor.processInvocation(EjbRequestScopeActivationInterceptor.java:74) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InitialInterceptor.processInvocation(InitialInterceptor.java:21) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.ComponentDispatcherInterceptor.processInvocation(ComponentDispatcherInterceptor.java:53) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.singleton.SingletonComponentInstanceAssociationInterceptor.processInvocation(SingletonComponentInstanceAssociationInterceptor.java:52) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.CurrentInvocationContextInterceptor.processInvocation(CurrentInvocationContextInterceptor.java:41) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.ShutDownInterceptorFactory$1.processInvocation(ShutDownInterceptorFactory.java:64) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.LoggingInterceptor.processInvocation(LoggingInterceptor.java:59) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.NamespaceContextInterceptor.processInvocation(NamespaceContextInterceptor.java:50) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.AdditionalSetupInterceptor.processInvocation(AdditionalSetupInterceptor.java:55) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.TCCLInterceptor.processInvocation(TCCLInterceptor.java:45) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.ViewService$View.invoke(ViewService.java:165) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ee.component.ViewDescription$1.processInvocation(ViewDescription.java:182) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.ProxyInvocationHandler.invoke(ProxyInvocationHandler.java:72) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jbpm.examples.ejb.ProcessLocal$$$view3.startProcess(Unknown Source) [classes:]
	at org.jbpm.examples.web.ProcessServlet.doPost(ProcessServlet.java:46) [classes:]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:754) [jboss-servlet-api_3.0_spec-1.0.2.Final-redhat-1.jar:1.0.2.Final-redhat-1]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:847) [jboss-servlet-api_3.0_spec-1.0.2.Final-redhat-1.jar:1.0.2.Final-redhat-1]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) [rt.jar:1.7.0_55]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57) [rt.jar:1.7.0_55]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) [rt.jar:1.7.0_55]
	at java.lang.reflect.Method.invoke(Method.java:606) [rt.jar:1.7.0_55]
	at org.apache.catalina.security.SecurityUtil$1.run(SecurityUtil.java:263) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.security.SecurityUtil$1.run(SecurityUtil.java:261) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.security.AccessController.doPrivileged(Native Method) [rt.jar:1.7.0_55]
	at javax.security.auth.Subject.doAsPrivileged(Subject.java:536) [rt.jar:1.7.0_55]
	at org.apache.catalina.security.SecurityUtil.execute(SecurityUtil.java:295) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.security.SecurityUtil.doAsPrivilege(SecurityUtil.java:155) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:288) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain.access$000(ApplicationFilterChain.java:59) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain$1.run(ApplicationFilterChain.java:197) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.security.AccessController.doPrivileged(Native Method) [rt.jar:1.7.0_55]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:193) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:230) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:149) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.jboss.as.jpa.interceptor.WebNonTxEmCloserValve.invoke(WebNonTxEmCloserValve.java:50) [jboss-as-jpa-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.jpa.interceptor.WebNonTxEmCloserValve.invoke(WebNonTxEmCloserValve.java:50) [jboss-as-jpa-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.web.security.SecurityContextAssociationValve.invoke(SecurityContextAssociationValve.java:169) [jboss-as-web-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:145) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:97) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.authenticator.SingleSignOn.invoke(SingleSignOn.java:389) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:102) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:336) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.coyote.http11.Http11Processor.process(Http11Processor.java:856) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler.process(Http11Protocol.java:653) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.tomcat.util.net.JIoEndpoint$Worker.run(JIoEndpoint.java:920) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.lang.Thread.run(Thread.java:745) [rt.jar:1.7.0_55]

02:14:30,483 ERROR [org.jbpm.services.task.wih.LocalHTWorkItemHandler] (http-localhost.localdomain/127.0.0.1:8080-1) Sat Dec 06 02:14:30 IST 2014: Error when creating task on task server for work item id 1. Error reported by task server: There are no known Business Administrators, task cannot be created according to WS-HT specification: org.jbpm.services.task.exception.CannotAddTaskException: There are no known Business Administrators, task cannot be created according to WS-HT specification
	at org.jbpm.services.task.commands.UserGroupCallbackTaskCommand.doCallbackOperationForPeopleAssignments(UserGroupCallbackTaskCommand.java:232) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.AddTaskCommand.execute(AddTaskCommand.java:100) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.AddTaskCommand.execute(AddTaskCommand.java:53) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.TaskCommandExecutorImpl$SelfExecutionCommandService.execute(TaskCommandExecutorImpl.java:65) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.persistence.TaskTransactionInterceptor.execute(TaskTransactionInterceptor.java:54) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.commands.TaskCommandExecutorImpl.execute(TaskCommandExecutorImpl.java:40) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.impl.command.CommandBasedTaskService.addTask(CommandBasedTaskService.java:471) [jbpm-human-task-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.runtime.manager.impl.task.SynchronizedTaskService.addTask(SynchronizedTaskService.java:456) [jbpm-runtime-manager-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.services.task.wih.LocalHTWorkItemHandler.executeWorkItem(LocalHTWorkItemHandler.java:61) [jbpm-human-task-workitems-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.jpa.processinstance.JPAWorkItemManager.internalExecuteWorkItem(JPAWorkItemManager.java:56) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.WorkItemNodeInstance.internalTrigger(WorkItemNodeInstance.java:128) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.trigger(NodeInstanceImpl.java:155) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.triggerNodeInstance(NodeInstanceImpl.java:347) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.triggerCompleted(NodeInstanceImpl.java:306) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.StartNodeInstance.triggerCompleted(StartNodeInstance.java:66) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.node.StartNodeInstance.internalTrigger(StartNodeInstance.java:43) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.NodeInstanceImpl.trigger(NodeInstanceImpl.java:155) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.ruleflow.instance.RuleFlowProcessInstance.internalStart(RuleFlowProcessInstance.java:35) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.impl.ProcessInstanceImpl.start(ProcessInstanceImpl.java:226) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.workflow.instance.impl.WorkflowProcessInstanceImpl.start(WorkflowProcessInstanceImpl.java:363) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcessInstance(ProcessRuntimeImpl.java:194) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcess(ProcessRuntimeImpl.java:176) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.process.instance.ProcessRuntimeImpl.startProcess(ProcessRuntimeImpl.java:168) [jbpm-flow-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.common.AbstractWorkingMemory.startProcess(AbstractWorkingMemory.java:1575) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.impl.StatefulKnowledgeSessionImpl.startProcess(StatefulKnowledgeSessionImpl.java:366) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.runtime.process.StartProcessCommand.execute(StartProcessCommand.java:121) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.runtime.process.StartProcessCommand.execute(StartProcessCommand.java:40) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.DefaultCommandService.execute(DefaultCommandService.java:36) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.SingleSessionCommandService$TransactionInterceptor.execute(SingleSessionCommandService.java:533) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.AbstractInterceptor.executeNext(AbstractInterceptor.java:41) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.jpa.OptimisticLockRetryInterceptor.execute(OptimisticLockRetryInterceptor.java:73) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.persistence.SingleSessionCommandService.execute(SingleSessionCommandService.java:377) [drools-persistence-jpa-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.drools.core.command.impl.CommandBasedStatefulKnowledgeSession.startProcess(CommandBasedStatefulKnowledgeSession.java:232) [drools-core-6.0.3-redhat-6.jar:6.0.3-redhat-6]
	at org.jbpm.examples.ejb.ProcessBean.startProcess(ProcessBean.java:72) [classes:]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) [rt.jar:1.7.0_55]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57) [rt.jar:1.7.0_55]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) [rt.jar:1.7.0_55]
	at java.lang.reflect.Method.invoke(Method.java:606) [rt.jar:1.7.0_55]
	at org.jboss.as.ee.component.ManagedReferenceMethodInterceptorFactory$ManagedReferenceMethodInterceptor.processInvocation(ManagedReferenceMethodInterceptorFactory.java:72) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.WeavedInterceptor.processInvocation(WeavedInterceptor.java:53) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:374) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.weld.ejb.Jsr299BindingsInterceptor.doMethodInterception(Jsr299BindingsInterceptor.java:129) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.weld.ejb.Jsr299BindingsInterceptor.processInvocation(Jsr299BindingsInterceptor.java:137) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.WeavedInterceptor.processInvocation(WeavedInterceptor.java:53) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.UserInterceptorFactory$1.processInvocation(UserInterceptorFactory.java:58) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.invocationmetrics.ExecutionTimeInterceptor.processInvocation(ExecutionTimeInterceptor.java:43) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext$Invocation.proceed(InterceptorContext.java:374) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.concurrency.ContainerManagedConcurrencyInterceptor.processInvocation(ContainerManagedConcurrencyInterceptor.java:104) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.tx.EjbBMTInterceptor.handleInvocation(EjbBMTInterceptor.java:104) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ejb3.tx.BMTInterceptor.processInvocation(BMTInterceptor.java:56) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.weld.ejb.EjbRequestScopeActivationInterceptor.processInvocation(EjbRequestScopeActivationInterceptor.java:74) [jboss-as-weld-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InitialInterceptor.processInvocation(InitialInterceptor.java:21) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.interceptors.ComponentDispatcherInterceptor.processInvocation(ComponentDispatcherInterceptor.java:53) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.singleton.SingletonComponentInstanceAssociationInterceptor.processInvocation(SingletonComponentInstanceAssociationInterceptor.java:52) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.CurrentInvocationContextInterceptor.processInvocation(CurrentInvocationContextInterceptor.java:41) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.ShutDownInterceptorFactory$1.processInvocation(ShutDownInterceptorFactory.java:64) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.LoggingInterceptor.processInvocation(LoggingInterceptor.java:59) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.NamespaceContextInterceptor.processInvocation(NamespaceContextInterceptor.java:50) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ejb3.component.interceptors.AdditionalSetupInterceptor.processInvocation(AdditionalSetupInterceptor.java:55) [jboss-as-ejb3-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.TCCLInterceptor.processInvocation(TCCLInterceptor.java:45) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.ViewService$View.invoke(ViewService.java:165) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.ee.component.ViewDescription$1.processInvocation(ViewDescription.java:182) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.invocation.InterceptorContext.proceed(InterceptorContext.java:288) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.invocation.ChainedInterceptor.processInvocation(ChainedInterceptor.java:61) [jboss-invocation-1.1.2.Final-redhat-1.jar:1.1.2.Final-redhat-1]
	at org.jboss.as.ee.component.ProxyInvocationHandler.invoke(ProxyInvocationHandler.java:72) [jboss-as-ee-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jbpm.examples.ejb.ProcessLocal$$$view3.startProcess(Unknown Source) [classes:]
	at org.jbpm.examples.web.ProcessServlet.doPost(ProcessServlet.java:46) [classes:]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:754) [jboss-servlet-api_3.0_spec-1.0.2.Final-redhat-1.jar:1.0.2.Final-redhat-1]
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:847) [jboss-servlet-api_3.0_spec-1.0.2.Final-redhat-1.jar:1.0.2.Final-redhat-1]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) [rt.jar:1.7.0_55]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57) [rt.jar:1.7.0_55]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) [rt.jar:1.7.0_55]
	at java.lang.reflect.Method.invoke(Method.java:606) [rt.jar:1.7.0_55]
	at org.apache.catalina.security.SecurityUtil$1.run(SecurityUtil.java:263) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.security.SecurityUtil$1.run(SecurityUtil.java:261) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.security.AccessController.doPrivileged(Native Method) [rt.jar:1.7.0_55]
	at javax.security.auth.Subject.doAsPrivileged(Subject.java:536) [rt.jar:1.7.0_55]
	at org.apache.catalina.security.SecurityUtil.execute(SecurityUtil.java:295) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.security.SecurityUtil.doAsPrivilege(SecurityUtil.java:155) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:288) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain.access$000(ApplicationFilterChain.java:59) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.ApplicationFilterChain$1.run(ApplicationFilterChain.java:197) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.security.AccessController.doPrivileged(Native Method) [rt.jar:1.7.0_55]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:193) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:230) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:149) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.jboss.as.jpa.interceptor.WebNonTxEmCloserValve.invoke(WebNonTxEmCloserValve.java:50) [jboss-as-jpa-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.jpa.interceptor.WebNonTxEmCloserValve.invoke(WebNonTxEmCloserValve.java:50) [jboss-as-jpa-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.jboss.as.web.security.SecurityContextAssociationValve.invoke(SecurityContextAssociationValve.java:169) [jboss-as-web-7.2.1.Final-redhat-10.jar:7.2.1.Final-redhat-10]
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:145) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:97) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.authenticator.SingleSignOn.invoke(SingleSignOn.java:389) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:102) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:336) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.coyote.http11.Http11Processor.process(Http11Processor.java:856) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.coyote.http11.Http11Protocol$Http11ConnectionHandler.process(Http11Protocol.java:653) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at org.apache.tomcat.util.net.JIoEndpoint$Worker.run(JIoEndpoint.java:920) [jbossweb-7.2.2.Final-redhat-1.jar:7.2.2.Final-redhat-1]
	at java.lang.Thread.run(Thread.java:745) [rt.jar:1.7.0_55]

02:14:30,512 INFO  [stdout] (http-localhost.localdomain/127.0.0.1:8080-1) Process started ... : processInstanceId = 1

~~~

It is thrown because the task service is not able to find any user associated with the "Administrators" group. The "DBUserGroupCallbackImpl" uses the "PRINCIPAL_QUERY, ROLES_QUERY, USER_ROLES_QUERY" queries to confirm if a "user" exists in the predefined database table or not, if the "group" exists on the predefined database table or not, and most importantly the "user" belongs to the "group" or not as per the same predefined database table. The details about the query can be found inside "jbpm.usergroup.callback.properties" file which should be lying inside the application's classpath. Now, these same queries are also used to check if the "Administrators" group exists or not in those predefined database table and whether any user belongs to it or not.

- Now, download the H2 database console from the following URL [https://github.com/jboss-developer/jboss-eap-quickstarts/archive/6.3.0.GA.zip] and deploy the WAR inside EAP 6 installation's "/deployments" directory.

- Access the H2 console from browser like this way [http://localhost:8080/h2console/] and use the H2 JDBC URL and credentials to log-in from the datasource configuration of the standalone.xml file which this project is using.
~~~
...
        <subsystem xmlns="urn:jboss:domain:datasources:1.1">
            <datasources>
                <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
                    <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1</connection-url>
                    <driver>h2</driver>
                    <security>
                        <user-name>sa</user-name>
                        <password>sa</password>
                    </security>
                </datasource>
...
~~~

- Now use the following queries to create the required DB tables in your database and also insert the sample values in those tables.
~~~
create table Users (userId varchar(255));
create table Groups (groupId varchar(255), userId varchar(255));
insert into Users (userId) values ('jiri');
insert into Users (userId) values ('mary');
insert into Groups (groupId, userId) values ('PM', 'jiri');
insert into Groups (groupId, userId) values ('HR', 'mary');
insert into Groups (groupId, userId) values ('Administrators', 'Administrator');
~~~

- Now go ahead, create process instance, view and complete tasks for 'jiri' and 'mary' and it should work fine now.

- In order to undeploy the application please use the following command
~~~
mvn jboss-as:undeploy

~~~
