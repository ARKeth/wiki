# Modules / Controllers / Components

In both Angular 1 and Angular 2, Angular modules help you organize your application into cohesive blocks of functionality.

In Angular 1, you write the code that provides the model and the methods for the view in a **controller**. In Angular 2, you build a **component**.

Because much Angular 1 code is in JavaScript, JavaScript code is shown in the Angular 1 column. The Angular 2 code is shown using TypeScript.


## IIFE 

### Angular 1

```js
(function () {
  ...
}());
```
In Angular 1, you often defined an immediately invoked function expression (or IIFE) around your controller code. This kept your controller code out of the global namespace.

### Angular 2

You don't need to worry about this in Angular 2 because you use ES 2015 modules and modules handle the namespacing for you.


## Angular Modules 

### Angular 1

```js
angular.module("movieHunter", ["ngRoute"]);
```
In Angular 1, an Angular module keeps track of controllers, services, and other code. The second argument defines the list of other modules that this module depends upon.

### Angular 2

```typescript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent }  from './app.component';

@NgModule({
  imports: [ BrowserModule ],
  declarations: [ AppComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```
Angular 2 modules, defined with the `NgModule` decorator, serve the same purpose:

* `imports`: specifies the list of other modules that this module depends upon
* `declaration`: keeps track of your components, pipes, and directives.

## Controller Registration vs Component Decorator

### Angular 1 (Controller)

```js
angular
  .module("movieHunter")
  .controller("MovieListCtrl",
              ["movieService",
               MovieListCtrl]);
```
Angular 1, has code in each controller that looks up an appropriate Angular module and registers the controller with that module.

The first argument is the controller name. The second argument defines the string names of all dependencies injected into this controller, and a reference to the controller function.

### Angular 2 (Component)

```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'movie-list',
  templateUrl: './views/movie-list.component.html',
  styleUrls: [ './views/movie-list.component.css' ],
})
```
Angular 2, adds a decorator to the component class to provide any required metadata. The Component decorator declares that the class is a component and provides metadata about that component such as its selector (or tag) and its template.

This is how you associate a template with code, which is defined in the component class.

## Controller function vs Component class 

### Angular 1 (Controller)

```js
angular.module("movieHunter").controller("MovieListCtrl", function(movieService) {
  
});
```
In Angular 1, you write the code for the model and methods in a controller function.

### Angular 2 (Component)

```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'movie-list',
  templateUrl: './views/movie-list.component.html',
  styleUrls: [ './views/movie-list.component.css' ],
})
export class MovieListComponent {
}
```
In Angular 2, you create a component class.

NOTE: If you are using TypeScript with Angular 1, you must use the export keyword to export the component class.

## Dependency Injection 

### Angular 1

```js
MovieListCtrl.$inject = ['MovieService'];
function MovieListCtrl(movieService) {
}
```

In Angular 1, you pass in any dependencies as controller function arguments. This example injects a `MovieService`.

To guard against minification problems, tell Angular explicitly that it should inject an instance of the `MovieService` in the first parameter.

### Angular 2

```typescript
constructor(movieService: MovieService) {
}
```

In Angular 2, you pass in dependencies as arguments to the component class constructor. This example injects a `MovieService`. The first parameter's TypeScript type tells Angular what to inject, even after minification.
