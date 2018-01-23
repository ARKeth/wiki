# Template Basics

Templates are the user-facing part of an Angular application and are written in HTML. The following table lists some of the key Angular 1 template features with their equivalent Angular 2 template syntax.

## Bindings / Interpolation

### Angular 1

```js
Your favorite hero is: {{vm.favoriteHero}}
```

In Angular 1, an expression in curly braces denotes one-way binding. This binds the value of the element to a property in the controller associated with this template.

When using the controller as syntax, the binding is prefixed with the controller alias (vm or $ctrl) because you have to be specific about the source of the binding.

### Angular 2

```js
Your favorite hero is: {{favoriteHero}}
```

In Angular 2, a template expression in curly braces still denotes one-way binding. This binds the value of the element to a property of the component. The context of the binding is implied and is always the associated component, so it needs no reference variable.

For more information, see the Interpolation section of the Template Syntax page.

## Filters / Pipes

### Angular 1 (Filters)

```html
<td>{{movie.title | uppercase}}</td>
```

To filter output in Angular 1 templates, use the pipe character (|) and one or more filters.

This example filters the title property to uppercase.

### Angular 2 (Pipes)

```html
<td>{{movie.title | uppercase}}</td>
```

In Angular 2 you use similar syntax with the pipe (|) character to filter output, but now you call them pipes. Many (but not all) of the built-in filters from Angular 1 are built-in pipes in Angular 2.

For more information, see the heading Filters/pipes below.

## Local / Input Variables

### Angular 1 (Local)

```html
<tr ng-repeat="movie in vm.movies">
  <td>{{movie.title}}</td>
</tr>
```

Here, `movie` is a user-defined local variable.

### Angular 2 (Input)

```html
<tr *ngFor="let movie of movies">
  <td>{{movie.title}}</td>
</tr>
```
Angular 2 has true template input variables that are explicitly defined using the let keyword.

For more information, see the ngFor micro-syntax section of the Template Syntax page.
