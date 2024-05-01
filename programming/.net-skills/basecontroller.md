# BaseController



{% code fullWidth="true" %}
```
// basecontroller

public class BaseController<T> : ControllerBase where T : BaseController<T>
{
    private IMapper mapper;
    private ILogger<T> logger;
    private IUnitOfWork unitOfWork;
    
    protected IMapper _mapper => mapper ??= HttpContext.RequestServices.GetRequiredService<IMapper>();
    protected ILogger<T> _logger => logger ??= HttpContext.RequestServices.GetRequiredService<ILogger<T>>();
    protected IUnitOfWork _unitOfWork => unitOfWork ??= HttpContext.RequestServices.GetRequiredService<IUnitOfWork>();
}

```
{% endcode %}



{% code fullWidth="true" %}
```
// using

[ApiController]
[Authorize]
public class CompanyController : BaseController<CompanyController>
{

     var result = _mapper.Map<List<CompanyVM>>(companies);
     
     _logger.LogInformation($"Exception: " + ex.Message);  

}
```
{% endcode %}
