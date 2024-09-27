---
icon: gauge-simple-min
---

# datetime-local set date range

{% code fullWidth="true" %}
```
// Some code

<input type="datetime-local" class="half-form-control" formControlName="DateTime"  max={{TodayStr}}
              style="width: 420px;"><span class="pe-3"></span>

```
{% endcode %}



{% code fullWidth="true" %}
```html
// Some code

import { DatePipe, formatDate } from '@angular/common';
import * as moment from 'moment';

solution 1:

  let today = new Date()
  this.TodayStr = (moment(today)).format('yyyy-MM-DD')+'T00:00'

solution 2:

  this.TodayStr = formatDate(Date.now(),'yyyy-MM-ddThh:mm','en-AU');
```
{% endcode %}
