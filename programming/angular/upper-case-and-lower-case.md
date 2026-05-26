# Upper Case and Lower Case

<pre class="language-html"><code class="lang-html"><strong>&#x3C;input class="form-control" formControlName="rego" placeholder="Enter rego"
</strong>        oninput="this.value = this.value.toUpperCase()">
</code></pre>

```html
<input type="email" class="form-control" formControlName="email"  
    oninput="this.value = this.value.toLowerCase()"
      pattern="^[a-z0-9._%+\-]+@[a-z0-9.\-]+\.[a-z]{2,4}$" placeholder="name@example.com">
```

