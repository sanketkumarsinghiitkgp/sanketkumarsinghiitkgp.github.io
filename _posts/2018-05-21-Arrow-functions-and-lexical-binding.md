---
layout: post
title: Arrow Functions (and how it's different from 'normal functions')
---

At the first glance, arrow functions look like a more concise version of the normal Javascript functions.
But there are some situations where they behave quite differently. For example, consider the following:-

```
let obj={
	x: 1,
	prnt: function(){ return this.x;}
};

console.log(obj.prnt());

```

```
output:-
1
```
This behaviour is pretty intuitive.

But when we change the `prnt` function to an arrow function, this is what we get:-

```
let obj={
	x: 1,
	prnt: ()=>{ return this.x;}
};

console.log(obj.prnt());
```

```
output:-
undefined
```
Adding a couple of additional lines to the program brings sheds some light on the reason behind this 'discrepancy'.

```
let obj={
	x: 1,
	prnt: function(){ 
			console.log(this==obj);
			console.log(this==window);
			return this.x;
		}
};

console.log(obj.prnt());

```

```
output:-
true
false
1
```
***So, the `this` inside a non-arrow function refers to the `obj` object.***

```
let obj={
	x: 1,
	prnt: ()=>{ 
              console.log(this==obj);
              console.log(this==window);
              return this.x;
          }
};

console.log(obj.prnt());
```

```
output:-
false
true
undefined
```

***Which shows that the `this` inside an arrow function refers to the originating context(the context where the function was called)***, which happens to be the `window` object in the above example. This also explains why `this.x` is `undefined`. It is because the originating context, the `window` has no variable called `x`.