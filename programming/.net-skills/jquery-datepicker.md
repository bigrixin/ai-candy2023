# Jquery Datepicker

Datepicker using Jquery

#### &#x20;View

```
 <div class="form-group">
	@Html.LabelFor(model => model.StartAt, htmlAttributes: new { @class = "control-label col-md-2" })
  <div class="col-md-10">
	@{ Html.EnableClientValidation(false); }
	@Html.EditorFor(model => model.StartAt, new { htmlAttributes = new { @class = "form-control datepicker" } })
	@Html.ValidationMessageFor(model => model.StartAt, "", new { @class = "text-danger" })
	@{ Html.EnableClientValidation(true); }
   </div>
 </div>
```



#### Javascript

```
jQuery(function ($) {
	// format date
	var currentTime = new Date();
	//currentTime = currentTime.setMonth(date.getMonth() - 1);
	var currentMonth = currentTime.getMonth();
	var currentDate = currentTime.getDate();
	var currentYear = currentTime.getFullYear();

      $('input[type=datetime]').datepicker({
		dateFormat: "dd/mm/yy",
		changeMonth: true,
		changeYear: true,
		showStatus: true,
		showWeeks: true,
		currentText: 'Now',
		autoSize: true,
		gotoCurrent: true,
		minDate: new Date(currentYear, currentMonth, currentDate)
	});
});
```



#### Model

```
[DisplayFormat(ApplyFormatInEditMode = true, DataFormatString = "{0:dd/MMM/yyyy}")] 
public DateTime? ExpiredAt { get; set; }
```

#### Web.config

```
<system.web>
   <globalization culture="en-AU" uiCulture="en-AU" /> 
<system.web>
```

