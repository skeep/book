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


## Dependency injection as design pattern

> In software engineering, dependency injection is a software design pattern that implements inversion of control for resolving dependencies. A dependency is an object that can be used (a service). An injection is the passing of a dependency to a dependent object (a client) that would use it.

[Dependency injection - Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)

## Dependency injection as framework

## Multi-dependency Injection

## Injector Relationship

## Dependency Visibility
