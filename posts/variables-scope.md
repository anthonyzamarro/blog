---
title: 'Variables and Scope'
date: '2022-02-03'
---

## Scope: Hoisting

Whenever a file is executed, all variables and functions are brought to the top of their respective scoped environment. Using the **var** reserved word, variables are function scoped.
Whether it is the global scope, within a function, or other block, the javascript engine moves variables and nested functions to the top of the their respective scopes. This is called **hoisting** Let's look at some examples.

Variables and functions declared like this:
```javascript

function something() {
    ...code...
    var somethineElse = true;
}

var another = false;

var oneMore = function() {
    ...code...
    var moreStuff = '';
}

for(var i = 0; i < array.length; i++) {
    ...code...
    var stuff = '';
}
```

are converted to this at runtime:
```javascript
something, another, oneMore, index
function something() {
    somethineElse
    ...code...
    var somethingElse
}

var another = false;

var oneMore = function() {
    // moreStuff
    ...code...
    var moreStuff
}

for(var index = 0; index < array.length; index++) {
    stuff
    ...code...
    var stuff = array[index];
}
```

As you can see, variables and functions are hoisted to the top of the program at execution.

## let and const

Since the release of ES6, **let** and **const** were introduced to help address some hoisting issues.

Both **let** and **const** are block scoped meaning that their scoping environment is anything between the curly braces that they are declared. This is a bit different than **var** which is function scoped meaning that its scoping enviornment is the enclosing function and outter functions which includes the global scope. Let's see the differences between these:

```javascript
    console.log(a);
    console.log(b);
    console.log(c);
    function someFunc() {
        let a = 0;
        var b = 1;
        const c = 2;
    }

    someFunc();
```

Running the code above, we will get an error for the variables a and c. That is because variables a and c and defined with **let** and **const**, respectively. They are only available inside of `someFunc` and any other nested functions inside of `someFunc`, and they are note **hoisted**. But, if we comment out those two logs:

```javascript
    // console.log(a);
    console.log(b);
    // console.log(c);
    function someFunc() {
        let a = 0;
        var b = 1;
        const c = 2;
    }

    someFunc();
```

Then we get **undefined** logged to the console. This is because variable b is declared with **var** which means that it does get **hoisted**.
