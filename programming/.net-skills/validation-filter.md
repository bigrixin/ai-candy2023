# Validation filter

{% code fullWidth="true" %}
```
public class ModelStateValidationAttribute : Attribute, IAsyncActionFilter
{
    async Task IAsyncActionFilter.OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        if (!context.ModelState.IsValid)
            context.Result = new BadRequestObjectResult(context.ModelState);
        else
            await next();
    }
}
```
{% endcode %}

{% code fullWidth="true" %}
```
 //without validation filter
[HttpPost]
[Route("[controller]/create")]
public async Task<IActionResult> Create([FromForm] CompanyVM companyVM)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    ...
}


// use validation filter
[HttpPost]
[Route("[controller]/create")]
[ModelStateValidationAttribute]
public async Task<IActionResult> Create([FromForm] CompanyVM companyVM)
{
    ...

}
```
{% endcode %}
