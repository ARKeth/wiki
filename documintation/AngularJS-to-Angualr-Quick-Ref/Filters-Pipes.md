# Filters and Pipes

Angular 2 **pipes** provide formatting and transformation for data in our template, similar to Angular 1 **filters**. Many of the built-in filters in Angular 1 have corresponding pipes in Angular 2.

## Currency

### Angular 1

```html
<td>{{movie.price | currency}}</td>
```
Formats a number as a currency.

### Angular 2

```html
<td>{{movie.price | currency:'USD':true}}</td>
```
The Angular 2 `currency` pipe is similar although some of the parameters have changed.

## Date 

### Angular 1

```html
<td>{{movie.releaseDate  | date}}</td>
```

Formats a date to a string based on the requested format.

### Angular 2

```html
<td>{{movie.releaseDate | date}}</td>
```
The Angular 2 date pipe is similar.

## Filter

### Angular 1 (filter)

```html
<tr ng-repeat="movie in movieList | filter: {title:listFilter}">
```
Selects a subset of items from the defined collection, based on the filter criteria.

### Angular 2 (none)

For performance reasons, no comparable pipe exists in Angular 2. Do all your filtering in the component. If you need the same filtering code in several templates, consider building a custom pipe.

## JSON 

### Angular 1

```html
<pre>{{movie | json}}</pre>
```
Converts a JavaScript object into a JSON string. This is useful for debugging.

### Angular 2

```html
<pre>{{movie | json}}</pre>
```
The Angular 2 json pipe does the same thing.

## limitTo vs slice

### Angular 1 (limitTo)

```html
<tr ng-repeat="movie in movieList | limitTo:2:0">
```
Selects up to the first parameter (2) number of items from the collection starting (optionally) at the beginning index (0).

### Angular 2 (slice)

```html
<tr *ngFor="let movie of movies | slice:0:2">
```
The `SlicePipe` does the same thing but the order of the parameters is reversed, in keeping with the JavaScript `Slice` method. The first parameter is the starting index; the second is the limit. As in Angular 1, coding this operation within the component instead could improve performance.

## lowercase

### Angular 1

```html
<div>{{movie.title | lowercase}}</div>
```
Converts the string to lowercase.

### Angular 2

```html
<div>{{movie.title | lowercase}}</div>
```
The Angular 2 `lowercase` pipe does the same thing.

## number

### Angular 1

```html
<td>{{movie.starRating  | number}}</td>
```
Formats a number as text.

### Angular 2

```html
<td>{{movie.starRating | number}}</td>
<td>{{movie.starRating | number:'1.1-2'}}</td>
<td>{{movie.approvalRating | percent: '1.0-2'}}</td>
```
The Angular 2 `number` pipe is similar. It provides more functionality when defining the decimal places, as shown in the second example above.

Angular 2 also has a `percent` pipe, which formats a number as a local percentage as shown in the third example.

## orderBy

### Angular 1

```html
<tr ng-repeat="movie in movieList | orderBy : 'title'">
```
Displays the collection in the order specified by the expression. In this example, the movie title orders the movieList.

### Angular 2

For performance reasons, no comparable pipe exists in Angular 2. Instead, use component code to order or sort results. If you need the same ordering or sorting code in several templates, consider building a custom pipe.




