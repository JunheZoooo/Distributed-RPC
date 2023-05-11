# Introduction 
**After studying Netty, I decided to write a lightweight RPC framework based on Netty, ZooKeeper, and Spring, which has been a rewarding experience for me.**


# Features
- **Support for long connections**
- **Support for asynchronous invocations**
- **Support for heartbeat detection**
- **Support for JSON serialization**
- **Near-zero configuration, implemented through annotations**
- **Service registration center implemented based on ZooKeeper**
- **Support for dynamic management of client connections**
- **Support for client service listening and discovery functionality**
- **Support for server service registration functionality**
- **Implemented using Netty 4.X version**






# Quick Start
### Server-Side Development
- **Add your own Service under the Service package on the server-side, and annotate it with the @Service annotation.**
	<pre>
	@Service
	public class TestService {
		public void test(User user){
			System.out.println("调用了TestService.test");
		}
	}
	</pre>

- **Generating a Service Interface and its Implementation Class**
	######The interface is as follows:
	<pre>
	public interface TestRemote {
		public Response testUser(User user);  
	}
	</pre>
	###### In the implementation class, we add the @Remote annotation to indicate that this class is where the actual service invocation takes place. This class is responsible for invoking the service methods and can generate any form of response that you desire to be returned to the client

	<pre> 
	@Remote
	public class TestRemoteImpl implements TestRemote{
		@Resource
		private TestService service;
		public Response testUser(User user){
			service.test(user);
			Response response = ResponseUtil.createSuccessResponse(user);
			return response;
		}
	}	
	</pre>


### Client-Side Development
- **Generate an interface on the client-side that represents the interface you want to invoke.**
	<pre>
	public interface TestRemote {
		public Response testUser(User user);
	}
	</pre>

### Usage
- **Generate a property in the place where you want to make the invocation, in the form of an interface, and annotate the property with @RemoteInvoke.**
	<pre>
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(classes=RemoteInvokeTest.class)
	@ComponentScan("\\")
	public class RemoteInvokeTest {
		@RemoteInvoke
		public static TestRemote userremote;
		public static User user;
		@Test
		public void testSaveUser(){
			User user = new User();
			user.setId(1000);
			user.setName("张三");
			userremote.testUser(user);
		}
	}	
	</pre>

### Results
- **Results of ten thousand invocations**
![Markdown](https://s1.ax1x.com/2018/07/06/PZMMBF.png)

- **Results of One Hundred Thousand Invocations**
![Markdown](https://s1.ax1x.com/2018/07/06/PZM3N9.png)

- **Results of One Million Invocations**
![Markdown](https://s1.ax1x.com/2018/07/06/PZMY1x.png)



# Overview

![Markdown](https://s1.ax1x.com/2018/07/06/PZK3SP.png)
# Distributed-RPC
# Distributed-RPC
