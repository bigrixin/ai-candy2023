# sticky \<th>  and \<td> content

{% code fullWidth="true" %}
````scss
.stickyColumn {
  position: sticky !important;
  left: 0 !important;    // left 0 or right 0
  background-color: rgb(215, 219, 214);
  z-index: 1;    // this is for th
}


```html
    <thead class="table-dark">
      <tr align="center">
        <th class="stickyColumn">Id</th>
```

```html
      <tr scope="row" align="left">
        <td class="stickyColumn">{{dataItem.id}}</td>
```
````
{% endcode %}

{% code fullWidth="true" %}
```
// sticky left column

component.scss

::ng-deep .sticky-left-col {
  position: sticky;
  left: 0 !important; // left 0 or right 0
  background-color: rgb(220, 223, 227) !important;
  z-index: 1; // this is for th
  width: 100px;
}

```
{% endcode %}

{% code fullWidth="true" %}
```
// sticky right column

styles.scss

.sticky-right-col {
  position: sticky;
  //  align-self: flex-start;
  right: 0;
  background-color: rgb(220, 223, 227) !important;
  width: 100px;
}

```
{% endcode %}

{% code fullWidth="true" %}
```
// ***.component.html 
// left use style, right use class


<thead class="table-dark">
  <tr>
    <th style="position: sticky">Ticket No</th>   
 ….


// ***.component.ts

columns: [{
        data: 'No', render: function (data: any, type: any, row: any) {
            return '<div  class="text-primary" style="text-align: left;">' + data+ '</div>';
    }, className: 'sticky-left-col'
```
{% endcode %}

