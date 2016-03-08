# Services

> A service is a mechanism to enable access to one or more capabilities, where the access is provided using a prescribed interface and is exercised consistent with constraints and policies as specified by the service description

[OASIS - Reference Model for Service Oriented Architecture 1.0](https://docs.oasis-open.org/soa-rm/v1.0/soa-rm.html)

Services are best suitable for some of the following purpose:
* State management
* Centralized management of internal and external resources
* Configuration management
* Encapsulating Complex calculation or business logic
* Utility methods

Let's quickly jump on to make our first angular service and understand how it works.

#### data.service.ts
```javascript
export class DataService {

  get() {
    return 'Hello!!!';
  }
}
```

#### app.component.ts
```javascript
import {Component} from "angular2/core";
import {DataService} from "./data.service";
@Component({
  selector: 'my-app',
  template: '',
  providers: [DataService]
})

export class ApiComponent {

  constructor(private _dataService:DataService) {
    console.log(_dataService.get())
  }
}
```

The `data.service.ts` file is simple enough. It's just a class with a method `get()` in it, which returns the text `Hello!!!`.

Now for `app.component.ts` it's mostly just a component definition except one decorator key `providers` and constructor parameters.
But what we just experienced is one of the most powerful feature of Angular namely dependency injection.

Before we understand Angular DI system in detail in the later part of the chapter we will understand a bare minimum so that we can understand what's happening in the above code.

Interestingly the above code is a shorthand and encapsulates quite a lot of implementation details. It will be good if we rewrite the code with little more verbosity it will easier to understand what is going on under the hood.

```javascript
import {DataService} from "./data.service";
import {Component, OnInit, Injector, Provider} from "angular2/core";

@Component({
  selector: 'my-app',
  template: ''
})

export class ApiVerboseComponent implements OnInit {

  _dataService;

  constructor() {

    var injector = Injector.resolveAndCreate([
      new Provider(DataService, {useClass: DataService})
    ]);

    this._dataService = injector.get(DataService);
  }

  ngOnInit() {
    console.log(this._dataService.get())
  }
}
```

The above is is verbose and intimidating to look at, but it clearly explains what exactly happens in Angular 2 DI framework.

1. `Injector` is a dependency injection container used for instantiating objects and resolving dependencies. It is a replacement for a new operator, which can automatically resolve the constructor dependencies.

2. `Provider` describes how the Injector should instantiate a given token. There quite few ways a token can be instantiated, but for now we will focus only on `useClass`

3. `useClass` binds a DI token to an implementation class. Important point to note here is in this case a separate instance of the `class` is returned.

## Dependency injection as design pattern

> In software engineering, dependency injection is a software design pattern that implements inversion of control for resolving dependencies. A dependency is an object that can be used (a service). An injection is the passing of a dependency to a dependent object (a client) that would use it.

[Dependency injection - Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)

## Dependency injection as framework

## Multi-dependency Injection

## Injector Relationship

## Dependency Visibility
