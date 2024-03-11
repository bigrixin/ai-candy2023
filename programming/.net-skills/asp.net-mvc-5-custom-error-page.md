---
description: error handle
---

# ASP.NET MVC 5 Custom Error Page

* Remove all 'customErrors' & 'httpErrors' from Web.config
* Check 'App\_Start/FilterConfig.cs' looks like this:

```
public class FilterConfig
{
    public static void RegisterGlobalFilters(GlobalFilterCollection filters)
    {
        filters.Add(new HandleErrorAttribute());
    }
}
```

* In 'Global.asax' add this method:

```
public void Application_Error(Object sender, EventArgs e)
{
    Exception exception = Server.GetLastError();
    Server.ClearError();

    var routeData = new RouteData();
    routeData.Values.Add("controller", "ErrorPage");
    routeData.Values.Add("action", "Error");
    routeData.Values.Add("exception", exception);

    if (exception.GetType() == typeof(HttpException))
    {
        routeData.Values.Add("statusCode", ((HttpException)exception).GetHttpCode());
    }
    else
    {
        routeData.Values.Add("statusCode", 500);
    }

    Response.TrySkipIisCustomErrors = true;
    IController controller = new ErrorPageController();
    controller.Execute(new RequestContext(new HttpContextWrapper(Context), routeData));
    Response.End();
}
```

* Add 'Controllers/ErrorPageController.cs'

```
public class ErrorPageController : Controller
{
    public ActionResult Error(int statusCode, Exception exception)
    {
         Response.StatusCode = statusCode;
         ViewBag.StatusCode = statusCode + " Error";
         return View();
    }
}
```

* Add 'Views/Shared/Error.cshtml'

```
@model System.Web.Mvc.HandleErrorInfo
@{
    ViewBag.Title = (!String.IsNullOrEmpty(ViewBag.StatusCode)) ? ViewBag.StatusCode : "500 Error";
}

<h1 class="error">@(!String.IsNullOrEmpty(ViewBag.StatusCode) ? ViewBag.StatusCode : "500 Error"):</h1>

//@Model.ActionName
//@Model.ControllerName
//@Model.Exception.Message
//@Model.Exception.StackTrace
```

* handle 403 error replace

```
context.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
```

with

```
throw new HttpException((int)System.Net.HttpStatusCode.Forbidden, "Forbidden");
```

ref: [https://newbedev.com/asp-net-mvc-5-custom-error-page](https://newbedev.com/asp-net-mvc-5-custom-error-page)
