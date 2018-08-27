Mule Multi-Threading
===
With mule it is astoundingly difficult to perform such a simple pattern as executing a list of tasks in parallel. As I found little help from google searches I eventually got this sample together, so others may benefit.
The code is largely self explanatory, but there is a few strange things worth noting.
## Exception Handling
When any of the VM flows throw an exception the main flow will just wait for ever. That is why it is important to 
 - Have a timeout on the Request-Reply
 - Have exception handlers that call the VM reply endpoint
 - Be able to asses after the request-reply if an error occurred
 
For a bit of extra weird mule you also need to add 
```xml
<vm:outbound-endpoint exchange-pattern="one-way" path="split" doc:name="VM">
 	<message-properties-transformer>
        	<delete-message-property key="MULE_REPLYTO" />
        </message-properties-transformer>
</vm:outbound-endpoint>
```
to the Request-reply outbound endpoint, or for run-time version 3.9 and above it will not wait for the reply.  
Beware of the versions of Anypoint Studio, mule run-time and munit. They are extremely buggy and if you are having trouble compare to the versions I use and ... good luck.
## Versions used
 - Anypoint Studio 6.5.0
 - 	<mule.version>3.9.1</mule.version>
 - <mule.munit.support.version>3.9.1</mule.munit.support.version> which must always match the run-time version
 - <munit.version>1.3.8</munit.version>
	

