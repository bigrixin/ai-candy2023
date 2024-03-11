---
description: Azure.Messaging.ServiceBus
---

# Azure service bus message (queue)



{% code fullWidth="true" %}
```
// Send message to queue

[HttpPost]
[Route("api/service-bus/create")]
public async Task<IActionResult> CreateQueue()
{  
    string connectionString = "Endpoint=sb://..;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=fsb/VbThEPnq.";
    await using var client = new ServiceBusClient(connectionString);
    
    /*  or below code
        var credential = new AzureNamedKeyCredential("RootManageSharedAccessKey", "fsb/VbThEPnq...");
        await using var client = new ServiceBusClient("metcash.servicebus.windows.net", credential);
    */
    
    /*
          /// but below code does not work
         string fullyNamespace = "***.servicebus.windows.net";
         await using var client = new ServiceBusClient(fullyNamespace, new DefaultAzureCredential());
    */
    
    ServiceBusSender sender = client.CreateSender("admindata");
    
    var message = "test11";
    var encodedMessage = new ServiceBusMessage(Encoding.UTF8.GetBytes(message));
    await sender.SendMessageAsync(encodedMessage);
    
    return Ok();
}
```
{% endcode %}
