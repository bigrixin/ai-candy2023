# sticky \<th>  and \<td> content

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