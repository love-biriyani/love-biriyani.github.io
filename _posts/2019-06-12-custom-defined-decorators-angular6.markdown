---
layout: post
title:  "Custom Defined Decorators in Angular 6"
date:   2019-06-12 01:28:000 +0100
categories: Web Programming Angular Frontend
---


## Method Decorator
Decorators are another feature used in Angular 2 and onwards. With Method decorator we can add, modify or replace  the functions in a class. In the following piece of code, with the declaration of the decorator the first three console logs will be printed. Then when the function will be invoked from the constructor it will print the arguments in the fourth line, then the original function will be called and the console log inside the original functon will be printed. After that the the original function will send the result to the new defined function and the result will be printed. At the end the colsole log in the constructor would be printed.

```typescript
function myDecorator(target: Object, name: String, descriptor: PropertyDescriptor): PropertyDescriptor {
  console.log(target);
  console.log(name);
  console.log(descriptor);

  const originalFn = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(args);
    const result = originalFn.apply(target, args);
    console.log("Result: ", result);
    return result;
  }

  return descriptor;
}


@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.sass']
})

export class AppComponent {

  @myDecorator
  myDecoratorOnFunction(x: number, y: number) {
    console.log('I am in the original function')
    return x + y ;
  }

  constructor() {
    console.log('Result in Constructor:' +this.myDecoratorOnFunction(5, 3));  
  }
}
```
The Program would print this output to the console:

```
// {myDecoratorOnFunction: ƒ, constructor: ƒ}
// myDecoratorOnFunction
// {value: ƒ, writable: true, enumerable: true, configurable: true}
// (2) [5, 3]
    0: 5
    1: 3
    length: 2
// I am in the original function
// Result:  8
// Result in Constructor:8

```

## Class Decorator
Class Decorator is used for class. The following decorator `myClassDecorator` is used to override the constructor for the `myTestClass` class.

```typescript
function myClassDecorator<T extends { new(...args: any[]): {} }>(constructor: T) {
  console.log('className: ' + constructor);
  return class extends constructor {
    greeting = "override";
  }
}

@myClassDecorator
class myTestClass {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
}

console.log (new myTestClass("world"));
```


The Program would print this output to the console:

```
// className: function myTestClass(message) {
        this.greeting = message;
    }
// class_1 {greeting: "override"}

```