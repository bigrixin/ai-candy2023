# Variable storage method



{% code fullWidth="true" %}
```
// sessionStorage
// Appropriate for data that needs to be retained only during the current session, like temporary form data and unfinished operation states.

  
myVariable: string;

  constructor() {
    const storedValue = sessionStorage.getItem('myVariable');
    if (storedValue) {
      this.myVariable = storedValue;
    } else {
      this.myVariable = '默认值';
    }
  }

  saveVariable() {
    sessionStorage.setItem('myVariable', this.myVariable);
  }

```
{% endcode %}

<pre data-full-width="true"><code>// localStorage 
<strong>// Suited for long-term data storage, such as user preference settings and theme selections.
</strong>
  myVariable: string;

  constructor() {
    // 从 localStorage 加载数据
    const storedValue = localStorage.getItem('myVariable');
    if (storedValue) {
      this.myVariable = storedValue;
    } else {
      this.myVariable = '默认值';
    }
  }

  saveVariable() {
    // 将变量保存到 localStorage
    localStorage.setItem('myVariable', this.myVariable);
  }
</code></pre>

{% code fullWidth="true" %}
```
// cookie 
// Primarily used for transmitting information between the client and the server. However, it has a relatively small storage capacity and is sent to the server with each request, which may affect performance.

  myVariable: string;

  constructor(private cookieService: CookieService) {
    const storedValue = this.cookieService.get('myVariable');
    if (storedValue) {
      this.myVariable = storedValue;
    } else {
      this.myVariable = '默认值';
    }
  }

  saveVariable() {
    this.cookieService.set('myVariable', this.myVariable);
  }

```
{% endcode %}
