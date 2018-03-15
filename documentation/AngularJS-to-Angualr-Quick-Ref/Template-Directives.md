# Template Directives

Angular 1 provides more than seventy built-in directives for templates. Many of them aren't needed in Angular 2 because of its more capable and expressive binding system. The following are some of the key Angular 1 built-in directives and their equivalents in Angular 2.

## ng-app vs Bootstrapping

### Angular 1 (ng-app)

```html
<body ng-app="movieHunter">
```

The application startup process is called bootstrapping.

Although you can bootstrap an Angular 1 app in code, many applications bootstrap declaratively with the ng-app directive, giving it the name of the application's module (movieHunter).

### Angular 2 (Bootstrapping)

main.ts
```typescript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```

app.module.ts
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

Angular 2 doesn't have a bootstrap directive. To launch the app in code, explicitly bootstrap the application's root module (AppModule) in main.ts and the application's root component (AppComponent) in app.module.ts.

## ng-class vs ngClass

### Angular 1 (ng-class)

```html
<div ng-class="{active: isActive}">
<div ng-class="{active: isActive,
                   shazam: isImportant}">
```

In Angular 1, the ng-class directive includes/excludes CSS classes based on an expression. That expression is often a key-value control object with each key of the object defined as a CSS class name, and each value defined as a template expression that evaluates to a Boolean value.

In the first example, the active class is applied to the element if isActive is true.

You can specify multiple classes, as shown in the second example.

### Angular 2 (ngClass)

```html
<div [ngClass]="{active: isActive}">
<div [ngClass]="{active: isActive,
                 shazam: isImportant}">
<div [class.active]="isActive">
```

In Angular 2, the ngClass directive works similarly. It includes/excludes CSS classes based on an expression.

In the first example, the active class is applied to the element if isActive is true.

You can specify multiple classes, as shown in the second example.

Angular 2 also has class binding, which is a good way to add or remove a single class, as shown in the third example.

For more information see the Attribute, Class, and Style Bindings section of the Template Syntax page.

## ng-click vs binding click event

### Angular 1 (ng-click)

```html
<button ng-click="vm.toggleImage()">
<button ng-click="vm.toggleImage($event)">
```

In Angular 1, the ng-click directive allows you to specify custom behavior when an element is clicked.

In the first example, when the user clicks the button, the toggleImage() method in the controller referenced by the vm controller as alias is executed.

The second example demonstrates passing in the $event object, which provides details about the event to the controller.

### Angular 2 (click)

```html
<button (click)="toggleImage()">
<button (click)="toggleImage($event)">
```

Angular 1 event-based directives do not exist in Angular 2. Rather, define one-way binding from the template view to the component using event binding.

For event binding, define the name of the target event within parenthesis and specify a template statement, in quotes, to the right of the equals. Angular 2 then sets up an event handler for the target event. When the event is raised, the handler executes the template statement.

In the first example, when a user clicks the button, the toggleImage() method in the associated component is executed.

The second example demonstrates passing in the $event object, which provides details about the event to the component.

For a list of DOM events, see: https://developer.mozilla.org/en-US/docs/Web/Events.

For more information, see the Event Binding section of the Template Syntax page.

## ng-controller vs Component decorator

### Angular 1 (ng-controller)

```html
<div ng-controller="MovieListCtrl as vm">
```

In Angular 1, the ng-controller directive attaches a controller to the view. Using the ng-controller (or defining the controller as part of the routing) ties the view to the controller code associated with that view.

### Angular 2 (Component)

```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'movie-list',
  templateUrl: './views/movie-list.component.html',
  styleUrls: [ './views/movie-list.component.css' ],
})
```

In Angular 2, the template no longer specifies its associated controller. Rather, the component specifies its associated template as part of the component class decorator.

## ng-hide / ng-show vs hidden binding

### Angular 1 (ng-hide)

```html
<h3 ng-show="vm.favoriteHero">
  Your favorite hero is: {{vm.favoriteHero}}
</h3>
```

In Angular 1, the ng-show directive shows or hides the associated DOM element, based on an expression.

In this example, the div element is shown if the favoriteHero variable is truthy.

### Angular 2 (Hidden)

```html
<h3 [hidden]="!favoriteHero">
  Your favorite hero is: {{favoriteHero}}
</h3>
```

Angular 2, uses property binding; there is no built-in show directive. For hiding and showing elements, bind to the HTML hidden property.

To conditionally display an element, place the element's hidden property in square brackets and set it to a quoted template expression that evaluates to the opposite of show.

In this example, the div element is hidden if the favoriteHero variable is not truthy.

## ng-href vs href binding

### Angular 1 (ng-href)

```html
<a ng-href="angularDocsUrl">Angular Docs</a>
```

The ng-href directive allows Angular 1 to preprocess the href property so that it can replace the binding expression with the appropriate URL before the browser fetches from that URL.

In Angular 1, the ng-href is often used to activate a route as part of navigation.

```html
<a ng-href="#movies">Movies</a>
```

Routing is handled differently in Angular 2.

### Angular 2 (href)

```html
<a [href]="angularDocsUrl">Angular Docs</a>
```

Angular 2, uses property binding; there is no built-in href directive. Place the element's href property in square brackets and set it to a quoted template expression.

In Angular 2, href is no longer used for routing. Routing uses routerLink, as shown in this example:

```html
<a [routerLink]="['/movies']">Movies</a>
```

## ng-if vs *ngIf

### Angular 1 (ng-if)

```html
<table ng-if="movies.length">
```

In Angular 1, the ng-if directive removes or recreates a portion of the DOM, based on an expression. If the expression is false, the element is removed from the DOM.

In this example, the table element is removed from the DOM unless the movies array has a length greater than zero.

### Angular 2 (*ngIf)

```html
<table *ngIf="movies.length">
```

The `*ngIf` directive in Angular 2 works the same as the `ng-if` directive in Angular 1. It removes or recreates a portion of the DOM based on an expression.

In this example, the table element is removed from the DOM unless the movies array has a length.

The (*) before ngIf is required in this example.

## ng-model vs ngModel

### Angular 1 (ng-model)

```html
<input ng-model="vm.favoriteHero"/>
```

In Angular 1, the `ng-model` directive binds a form control to a property in the controller associated with the template. This provides two-way binding, whereby any change made to the value in the view is synchronized with the model, and any change to the model is synchronized with the value in the view.

### Angular 2 (ngModel)

```html
<input [(ngModel)]="favoriteHero" />
```

In Angular 2, two-way binding is denoted by `[()]`, descriptively referred to as a **"banana in a box"**. This syntax is a shortcut for defining both property binding (from the component to the view) and event binding (from the view to the component), thereby providing two-way binding.

## ng-repeat vs *ngFor

### Angular 1 (ng-repeat)

```html
<tr ng-repeat="movie in vm.movies">
```

In Angular 1, the `ng-repeat` directive repeats the associated DOM element for each item in the specified collection.

In this example, the table row (`tr`) element repeats for each movie object in the collection of movies.

### Angular 2 (ngFor)

```html
<tr *ngFor="let movie of movies">
```

The *ngFor directive in Angular 2 is similar to the `ng-repeat` directive in Angular 1. It repeats the associated DOM element for each item in the specified collection. More accurately, it turns the defined element (`tr` in this example) and its contents into a template and uses that template to instantiate a view for each item in the list.

Notice the other syntax differences: The (*) before `ngFor` is required; the `let` keyword identifies `movie` as an input variable; the list preposition is `of`, not `in`.

## ng-src vs src binding

### Angular 1 (ng-src)

```html
<img ng-src="{{movie.imageurl}}">
```

The `ng-src` directive allows Angular 1 to preprocess the src property so that it can replace the binding expression with the appropriate URL before the browser fetches from that URL.

### Angular 2 (src)

```html
<img [src]="movie.imageurl">
```

Angular 2, uses property binding; there is no built-in *src* directive. Place the `src` property in square brackets and set it to a quoted template expression.

## ng-style vs ngStyle

### Angular 1 (ng-style)

```html
<div ng-style="{color: colorPreference}">
```

In Angular 1, the `ng-style` directive sets a CSS style on an HTML element based on an expression. That expression is often a key-value control object with each key of the object defined as a CSS style name, and each value defined as an expression that evaluates to a value appropriate for the style.

In the example, the `color` style is set to the current value of the `colorPreference` variable.

### Angular 2 (ngStyle)

```html
<div [ngStyle]="{color: colorPreference}">
<div [style.color]="colorPreference">
```

In Angular 2, the `ngStyle` directive works similarly. It sets a CSS style on an HTML element based on an expression.

In the first example, the `color` style is set to the current value of the `colorPreference` variable.

Angular 2 also has **style binding**, which is good way to set a single style. This is shown in the second example.

## ng-switch vs ngSwitch

### Angular 1 (ng-switch)

```html
<div ng-switch="vm.favoriteHero &&
                vm.checkMovieHero(vm.favoriteHero)">
    <div ng-switch-when="true">
      Excellent choice!
    </div>
    <div ng-switch-when="false">
      No movie, sorry!
    </div>
    <div ng-switch-default>
      Please enter your favorite hero.
    </div>
</div>
```

In Angular 1, the `ng-switch` directive swaps the contents of an element by selecting one of the templates based on the current value of an expression.

In this example, if `favoriteHero` is not set, the template displays "Please enter ...". If `favoriteHero` is set, it checks the movie hero by calling a controller method. If that method returns `true`, the template displays "Excellent choice!". If that methods returns `false`, the template displays "No movie, sorry!".

### Angular 2 (ngSwitch)

```html
<span [ngSwitch]="favoriteHero &&
               checkMovieHero(favoriteHero)">
  <p *ngSwitchCase="true">
    Excellent choice!
  </p>
  <p *ngSwitchCase="false">
    No movie, sorry!
  </p>
  <p *ngSwitchDefault>
    Please enter your favorite hero.
  </p>
</span>
```
In Angular 2, the `ngSwitch` directive works similarly. It displays an element whose `*ngSwitchCase` matches the current `ngSwitch` expression value.

In this example, if `favoriteHero` is not set, the `ngSwitch` value is `null` and `*ngSwitchDefault` displays, "Please enter ...". If `favoriteHero` is set, the app checks the movie hero by calling a component method. If that method returns `true`, the app selects `*ngSwitchCase="true"` and displays: "Excellent choice!" If that methods returns `false`, the app selects `*ngSwitchCase="false"` and displays: "No movie, sorry!"

The (*) before `ngSwitchCase` and `ngSwitchDefault` is required in this example.
