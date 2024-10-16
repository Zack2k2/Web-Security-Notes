<h3>optional arguments</h3>

a function:

```
const square = function(x){
	return x*x;
}

// Giving optional arguments runs with out
// complaining
console.log(square(4,4.14,"templeOS",false)); // 16
```

This might and might (with a grain of salt)
could become handy. 

```  
const pow = function(base,n){

let result = 1;

	for (let index = 0; index < n; index++) {
		result *= base;
	}

	return result;
};

console.log(square(16,5.3,true,"TempleOS")); // 256

console.log(pow(5,6,{"make":"file","long_hair":true}["long_hair"],[13,53,0.1,53],true,false));
```

all the call to functions works like charm