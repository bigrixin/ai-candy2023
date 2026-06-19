# Customer \<hr>

***

{% hint style="info" %}
```
-------------==== Vehicle ====-------------
```
{% endhint %}

```
<hr class="hr-text" data-content="Vehicle"> 
```

***

```

.hr-text {
  position: relative; 
  border: 0;
  height: 1.5em;
  text-align: center;
  opacity: 0.5;
}

.hr-text::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 0;
  width: 100%;
  height: 1px;
  background: linear-gradient(to right, transparent, #818078, transparent);
}

.hr-text::after {
  content: attr(data-content);
  position: relative;
  padding: 0 0.5em;
  background: #fcfcfa;
  color: #818078;
}
```

