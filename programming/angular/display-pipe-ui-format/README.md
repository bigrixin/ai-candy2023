# Display pipe (UI format)



{% code fullWidth="true" %}
```
// Angular Pipe:

<span> {{message | uppercase}} </span>                         // upcase
<span> {{message | lowercase}} </span>                         // lowcase
<span> {{item.name | titlecase}} </span>                       // first letter upcase

<span> {{item.Time | date:'dd/MM/yyyy hh:mm'}} </span>         // date format

<span> {{ products | json }} </span>               
 
   // OUTPUT  [ { "Id": 1, "Name": "Angular" }, { "Id": 2, "Name": "Typescript" } ]

```
{% endcode %}



{% code fullWidth="true" %}
```
// Input: 10000, output: 10,000.00

import { LOCALE_ID } from '@angular/core';

data = 10000;

constructor(
    @Inject(LOCALE_ID) public locale: string
  ) { }


formatNumber(Number(data),  locale, '1.2-2') 


<td>{data | number:'1.2-2'}}</td>

output: 10,000.00
```
{% endcode %}

{% code fullWidth="true" %}
```
// Custom pipe:
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'square'
})
export class SquarePipe implements PipeTransform {
  transform(value: number): number {
    return value * value;
  }
}


<span> {{ 4 | square}} </span>  
 // OUTPUT 16
```
{% endcode %}
