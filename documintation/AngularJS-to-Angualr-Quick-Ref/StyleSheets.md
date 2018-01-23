# StyleSheets

Style sheets give your application a nice look. In Angular 1, you specify the style sheets for your entire application. As the application grows over time, the styles for the many parts of the application merge, which can cause unexpected results. In Angular 2, you can still define style sheets for your entire application. But now you can also encapsulate a style sheet within a specific component.

## Link tag

### Angular 1

```html
<link href="styles.css" rel="stylesheet" />
```
Angular 1, uses a link tag in the head section of the index.html file to define the styles for the application.

### Angular 2

```html
<link rel="stylesheet" href="styles.css">
```
In Angular 2, you can continue to use the link tag to define the styles for your application in the index.html file. But now you can also encapsulate styles for your components.

## StyleUrls

### Angular 1

None, not supported.

### Angular 2

In Angular 2, you can use the `styles` or `styleUrls` property of the `@Component` metadata to define a style sheet for a particular component.

```typescript
styleUrls: [ './views/movie-list.component.css' ]
```

This allows you to set appropriate styles for individual components that won’t leak into other parts of the application.
