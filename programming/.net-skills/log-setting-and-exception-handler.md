# Log setting and exception handler

{% code fullWidth="true" %}
```
// nlog.config

<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      autoReload="true"
      internalLogLevel="Trace"    // INFO, WARN, and ERROR   <<<<<<<
      internalLogFile="${basedir}/logs/internal-nlog-AspNetCore.txt">   <<<<<<<<
```
{% endcode %}



{% code fullWidth="true" %}
```
using Microsoft.Extensions.Logging; 

public class ExceptionHandler : IExceptionFilter
{ 
    private readonly ILogger _logger;
    
    public ExceptionHandler(ILogger<ExceptionHandler> logger)
    {
        _logger = logger;
    }
    
    public void OnException(ExceptionContext context)
    {
        _logger.LogError($"Request body is not valid");
        _logger.LogError(context.Exception.Message);
        context.Result = new StatusCodeResult(500);
    }
}
```
{% endcode %}

{% code fullWidth="true" %}
```
// add to Startup.cs

services.AddControllers(config =>
{
    // Exceptions from controller will redirect to ExceptionHandler Class
    config.Filters.Add(typeof(ExceptionHandler));
}).AddJsonOptions(x =>
    x.JsonSerializerOptions.ReferenceHandler = ReferenceHandler.IgnoreCycles);
```
{% endcode %}
