# Dropdown list (customer)



{% code fullWidth="true" %}
````

```typescript
  methods = [
    { id: 1, name: "SINGLE" },
    { id: 2, name: "DOUBLE" }
  ];
```

```html
  <select class="form-select form-control oneline2-3" formControlName="method" placeholder="Please select">
    <option *ngFor="let method of methods" [ngValue]="method.name">{{method.name}}</option>
  </select>
```

````
{% endcode %}
