# FormData & FormGroup to JSON

{% code fullWidth="true" %}
```
let columns =  JSON.stringify(formGroupData.value.column);   //FormGroup to JSON

let jsonData = JSON.stringify(Object.fromEntries(formData));  //FormData to JSON
```
{% endcode %}
