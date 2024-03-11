# Html text alignment

[https://getbootstrap.com/docs/4.0/utilities/text/](https://getbootstrap.com/docs/4.0/utilities/text/)

<pre data-full-width="true"><code>// &#x3C;span> center
return '&#x3C;span style="color:blue; <a data-footnote-ref href="#user-content-fn-1">display: flex; justify-content: center; align-items: center;</a>">' + data + '&#x3C;/span>'

// &#x3C;span> right
return '&#x3C;span style="color:green;  <a data-footnote-ref href="#user-content-fn-2">display: inline-block; float: right;</a>">' + formatNumber(Number(data), 'en-AU', '1.2-2') + '&#x3C;/span>'
</code></pre>



{% code fullWidth="true" %}
```
// <tr><td> center
<tr align="center" class="align-middle center">

// <div> center
<div class="text-center" >

```
{% endcode %}

{% code fullWidth="true" %}
```
// Some code

<p class="text-left">Left aligned text on all viewport sizes.</p>
<p class="text-center">Center aligned text on all viewport sizes.</p>
<p class="text-right">Right aligned text on all viewport sizes.</p>

<p class="text-sm-left">Left aligned text on viewports sized SM (small) or wider.</p>
<p class="text-md-left">Left aligned text on viewports sized MD (medium) or wider.</p>
<p class="text-lg-left">Left aligned text on viewports sized LG (large) or wider.</p>
<p class="text-xl-left">Left aligned text on viewports sized XL (extra-large) or wider.</p>
```
{% endcode %}



{% code fullWidth="true" %}
```html
//vertical-alignment 

<span class="align-baseline">baseline</span>
<span class="align-top">top</span>
<span class="align-middle">middle</span>
<span class="align-bottom">bottom</span>
<span class="align-text-top">text-top</span>
<span class="align-text-bottom">text-bottom</span>
```
{% endcode %}

[^1]: center

[^2]: 
