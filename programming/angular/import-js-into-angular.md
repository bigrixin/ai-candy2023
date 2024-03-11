# Import JS into Angular

```
// Some code

1. add js file in assets/script/

2. tsconfig.json

   "compilerOptions": {
       "noImplicitAny":false,   <<<< add this line
       ...
    }
	

3. change js functions like below

	exports.Greet = function(){
	  alert("Hello");
	}


4. ts

	import {Greet} from '../../../assets/script/samplescript.js'

	  ngOnInit(): void {
	  Greet();
	}


5. html

   <button (click)="onClick()" class="btn btn-primary">Click Me</button>
```
