# Pipes

> In software engineering, a pipeline consists of a chain of processing elements (processes, threads, coroutines, functions, etc.), arranged so that the output of each element is the input of the next; the name is by analogy to a physical pipeline.

*[Wikipedia](https://en.wikipedia.org/wiki/Pipeline_(software))*

In angular also pipes intend to achieve similar functionality. It takes a input, apply transformation, creates a output available to be the input for the next pipe.

For example, if we have a component property `birthday` whose value is current date time, when you display it on screen we should see something like this, `Fri Feb 19 2016 23:39:04 GMT+0530 (IST)`.

Although it shows all the necessary information it's not very readable. If we want to make it more legible we can pass it through a pipe like `{{birthday | date}}`. Here the value of `birthday` acts as the input for the pipe `date` which outputs a formatted date `Feb 19, 2016`

#### pipe.ts
```javascript
import {Component} from 'angular2/core';

@Component({
  selector: 'pipe',
  template: `<div>{{birthday | date}}</div>`
})
export class Pipe {
  birthday:Date = new Date();
}
```

## Parameterizing a Pipe

Pipes may accept parameters to fine-tune the output, eg. `date` pipe takes date format as a parameter to change default output format. We add parameters to a pipe by following the pipe name with a colon ( : ) and then the parameter value. eg.

* `{birthday | date}}` outputs `Feb 19, 2016`
* `{birthday | date:"MMMMdyy"}}` outputs `February 20, 16`
* `{birthday | date:"jm"}}` outputs `1:12 AM`

## Pipe chaining

We can add more pipes and do further transformation. Let's say we want to transform the text case to all upper. Then we can add another pipe `{{birthday | date | lowercase}}` and we will get output of `feb 20, 2016`.

## In-built Pipes with Angular 2

Angular 2 comes with 11 pipes in-build. 10 of them are Stateless or pure pipes.

>In computer programming, a function may be considered a **pure** function if both of the following statements about the function hold:
1. The function always evaluates the same result value given the same argument value(s). The function result value cannot depend on any hidden information or state that may change while program execution proceeds or between different executions of the program, nor can it depend on any external input from I/O devices (usually—see below).
2. Evaluation of the result does not cause any semantically observable side effect or output, such as mutation of mutable objects or output to I/O devices

*[Wikipedia](https://en.wikipedia.org/wiki/Pure_function)*

In angular pure pipes follow similar convention. Pipes when defined unless mentioned are **pure** by default. In later part of the chapter we will see how to use and create a **impure** or **stateful** pipe.

### Stateless Pipe
1. CurrencyPipe
2. DatePipe
3. DecimalPipe
4. JsonPipe
5. LowerCasePipe
6. PercentPipe
7. SlicePipe
8. UpperCasePipe
9. I18nPluralPipe
10. I18nSelectPipe
11. ReplacePipe

These pipe comes bundled with angular2 and their uses are exactly their name suggest.

Let's see some examples of the above list to get an idea. Details can be found at [Angular 2 API Documentation](https://angular.io/docs/ts/latest/api/).

4 of the stateless pipes namely Currency, Date, Decimal & Percent uses *[Internationalization API](http://www.ecma-international.org/ecma-402/1.0/)* to implement it's features. Before diving deeper into these pipes let's understand i18n ( aka Internationalization) API in short.

#### Internationalization API (ECMA 402)

> The ECMAScript Internationalization API is designed to complement the ECMAScript Language Specification by providing key language-sensitive functionality. ... The ECMAScript Internationalization API is designed to complement the ECMAScript Language Specification by providing key language-sensitive functionality.

[ECMAScript Internationalization API Specification](http://www.ecma-international.org/ecma-402/1.0/)

At the time of writing this book Safari 9, iOS Safari 9.2, and Opera Mini 8 did not have support for this API. Whereas IE 11+, Edge 12+, Firefox 29+, Chrome 24+, Opera 15+ supports it.

For unsupported browser we can use polyfill like [Intl.js](https://github.com/andyearnshaw/Intl.js/) to make it work.

```javascript
import {Component} from 'angular2/core';

@Component({
  selector: 'my-app',
  template: `
  <h1>Pure Pipes</h1>
  <ol>
    <li>Currency : {{3000000.50 | currency:'INR':true}}</li>
    <li>Date : {{ now | date }} </li>
    <li>i18nSelect : {{ gender | i18nSelect: inviteMap }}</li>
    <li>i18nPlural : {{ messages.length | i18nPlural: messageMapping }}</li>
    <li>Replace : {{ hello | replace:exp:"from ng2" }}</li>
    <li>Decimal : {{ pythagoras | number}}</li>
    <li>Percent : {{ percent | percent:'2.0-3'}}</li>
    <li>Slice : {{ messages | slice:1:6 }}</li>
  </ol>
  `
})

export class AppComponent {
  // for DatePipe
  now = new Date();

  // for i18nSelectPipe
  gender:string = 'male';
  inviteMap:any = {
    'male': 'Invite her.',
    'female': 'Invite him.',
    'other': 'Invite them.'
  };

  // for i18nPlural
  messages:any = [1, 2, 3, 4, 5, 6, 7, 8, 9];
  messageMapping:any = {
    '=0': 'No messages.',
    '=1': 'One message.',
    'other': '# messages.'
  };

  // for ReplacePipe
  hello:String = 'Hello World!!!';
  exp:RegExp = /World/gi;

  // for DecimalPipe
  pythagoras = 1.41421356237309504880168872420969807;

  // forPercentPipe
  percent = 0.656;

}
```


Angular comes bundled with 1 Stateful pipe. And we are going to learn little more about it.

### Stateful Pipes

#### AsyncPipe

> will this component get re-rendered ? once promise is resolved ? Probably mentioning this will make this more clear

Async pipe can subscribe to a *Promise* or *Observable* and will return the latest value it emitted. This is remarkably powerful tool while building reactive app. We will create two components one subscribing to a Promise and other one subscribing to an Observable.

Let's look at a simple example first.

```javascript
import {Component, OnInit} from 'angular2/core';

@Component({
  selector: 'my-app',
  template: `<h1>{{message | async}}</h1>`
})
export class AppComponent implements OnInit {

  message:Promise<string>;

  ngOnInit() {
    this.message = new Promise((resolve) => {
      setTimeout(() => resolve(`Hello World!!!`), 500);
    });
  }
}


```

Let's replace the local promise with a server call.

```javascript
this.message = fetch('https://api.github.com/orgs/github')
  .then((res) => {
    return res.json();
  })
  .catch((ex) => {
    console.error(ex);
  });
```

So what did we gain. We did not have to resolve the promise and send the data to view. Less code means less chance of bug. If we did not use `async` pipe the code would look like below.

```javascript
import {Component, OnInit} from 'angular2/core';

@Component({
  selector: 'my-app',
  template: `<p>{{message | json}}</p>`
})
export class AppComponent implements OnInit {

  message:Object;

  ngOnInit() {
    fetch('https://api.github.com/orgs/github')
      .then((res) => {
        this.message = res.json();
      })
      .catch((ex) => {
        console.error(ex);
      });
  }
}
```

Now lets try with an Observable.

```javascript
import {Component, OnInit} from 'angular2/core';
import {Observable} from 'rxjs/Rx';

@Component({
  selector: 'my-app',
  template: `<h1>{{message | async | json}}</h1>`
})
export class AppComponent implements OnInit {
  /*
  This is observable generics . , but what's <number> . will it resolve by number ?
  If yes lets have a small comment above this line saying that
  */
  message:Observable<number>;

  ngOnInit() {
    this.message = Observable
      .timer(200, 500)
      .take(10);
  }
}
```

Without `async` the same code would look like :

```javascript
import {Component, OnInit} from 'angular2/core';
import {Observable} from 'rxjs/Rx';

@Component({
  selector: 'my-app',
  template: `<h1>{{message}}</h1>`
})
export class AppComponent implements OnInit {

  message:number;

  ngOnInit() {
    Observable
      .timer(200, 500)
      .take(10)
      .subscribe(
        (x)=> this.message = x,
        (err)=> console.log(err),
        ()=>console.info('complete')
      );
  }
}
```

## Custom Pipes

### Creating a Stateless pipe

Creating a custom pipe is very easy in Angular 2. Consider that we would need a pipe which will add `Hello ` before the string. So if I pass a string `name` with value *Suman* to the view. Then `name` is passed on to the pipe `greet` we will get `Hello Suman` as output.

To define a pipe we use `@Pipe` decorator which implements Pipe class.
It have two members being `{name: string, pure?: boolean}`.
`pure` is `false` by default. We learn more about `pure` pipes later in the chapter.

Let's create out first Stateless/ impure pipe.

#### greet.pipe.ts
```javascript
import {Pipe} from 'angular2/core';

@Pipe({
  name: 'Greet'
})
export class Greet{
  transform(name:string) {
    return `Hello ${name}`;
  }
}
```
You could also pass on parameters to pipes to control it's output. We will modify the transform method a little to make that possible. Let's say we want to support two languages to say `Hello`, English and French. We will update the `transform` method like below.

```javascript
transform(name:string, args) {
  if(args[0] === 'en') {
    return `Hello ${name}`;
  } else if(args[0] === 'fr') {
    return `Bonjour ${name}`;
  } else {
    return `Hello ${name}`;
  }
}
```

And you should not be able to use the pipe as below:



One things to notice is that `args` is an Array. This is to facilitate passing of multiple parameters from where pipe is used.

A more methodical way of implementing pipe with arguments is to implement `PipeTransform` interface. The above code can be modified as:


```javascript
import {Pipe, PipeTransform} from 'angular2/core';

@Pipe({
  name: 'Greet'
})
export class Greet implements PipeTransform {
  transform(name:string, args:any[] = []):any {
    if (args[0] === 'en') {
      return `Hello ${name}`;
    } else if (args[0] === 'fr') {
      return `Bonjour ${name}`;
    } else {
      return `Hello ${name}`;
    }
  }
}
```

### Creating a Stateful pipe

Stateful pipes are the once which can manage the data they transform. They will maintain a subscription to the input and the return value will depend on that subscription.

We learnt previously that pipes are stateless or pure by default. To create a stateful pipe we have to mention `pure: false` while declaring the pipe.  This setting tells Angular’s change detection system to check the output of this pipe each cycle, whether its input has changed or not.

In the below example we will create a stateful pipe to display selected locale text.
When the pipe is invoked it will open the locale file, locate the key value and return it back to the view. Opening of the file is an asyncronus process and view will receive the data when available. This would not work if it's stateless pipe. But as it's a stateful pipe Angular's change detection system will check on every cycle if there is any change on not. If yet it's then passed on to the View.

#### en.json
```json
{
  "HELLO" : "Hello"
}

```
#### fr.json
```json
{
  "HELLO" : "Bonjour"
}
```

#### greet.component.ts
```javascript
import {Component} from 'angular2/core';
import {LangPipe} from './lang.pipe';

@Component({
  selector: 'my-app',
  template: `<h1>{{'HELLO' | lang:selectedLocale}} Suman</h1>`,
  pipes: [LangPipe]
})

export class AppComponent{
  selectedLocale = 'en';
}
```
#### lang.pipe.ts
```javascript
import {Pipe, PipeTransform} from 'angular2/core';

@Pipe({
  name: 'lang',
  pure: false
})

export class LangPipe implements PipeTransform {

  private locale:String;
  private fetchedValue:any;
  private fetchPromise:Promise<any>;

  transform(key:string, args:string[]):any {
    this.locale = !args[0] ? 'en' : args[0];
    if (!this.fetchPromise) {
      this.fetchPromise = window.fetch(`./app/pipes/${this.locale}.json`)
        .then((result:any) => result.json())
        .then((json:any) => this.fetchedValue = json[key]);
    }
    return this.fetchedValue;
  }
}
```
When we run the example we should see `Hello Suman`.
If we change `selectedLocale = 'en';` to `selectedLocale = 'fr';` we should see `Bonjour Suman`.

## Conclusion

 1. In Angular a pipe takes a input, apply transformation, creates a output available to be the input for the next pipe.
 2. Pipes may accept parameters to fine-tune the output.
 3. We can chain pipes. The output of the pipe on the left act as input for the pipe on the right.
 4. In Angular pipe are of two types : Stateless and Stateful. Stateless pipes transform data only once where as stateful will maintain a subscription to the input and the return value will depend on that subscription. Angular’s change detection system to check the output of this pipe each cycle, whether its input has changed or not.
 5. Angular comes bundled with 10 pipes of which `async` is a stateful pipe.

