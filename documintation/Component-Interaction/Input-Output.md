# Input

Pass data from parent to child with the `input` binding or vice versa using the `output` binding.

## When to use

Majority of child components will be using the `input` and `output` interaction patterns. It is the most maintainable method and also helps enforce reusability.
 
### Advantages

* Decoupled from parent components (**Reusable**)
* Black box design adding transparency to your team (**Maintainable**)
* Easy to document -- input & output

### Disadvantages

* Limited to only receiving data synchronously from parent
* Simple, cannot do complex operations


### Rules

* Avoid having too many inputs and outputs, no more than 5
* If you have more than 5 input/output, considering consolidating variables into an object and using ngOnChanges or
* Do not insert functions or preflights, getter's and setter's only. If needed use the EventEmitter.
* Avoid complexity, keep it simple, over engineering can destroy your page.
* Avoid being too specific, if you need this child component but need to do other tweaks then create a subclass.

## Input Example

`HeroChildComponent` has two **input properties**, adorned with @Input decorations.

hero-child.component.ts
```typescript
import { Component, Input } from '@angular/core';
import { Hero } from './hero';
@Component({
  selector: 'hero-child',
  template: `
    <h3>{{hero.name}} says:</h3>
    <p>I, {{hero.name}}, am at your service, {{master}}.</p>
  `
})
export class HeroChildComponent {
  @Input() public hero: Hero;
  @Input() public master: string;
}
```
The `HeroParentComponent` nests the child `HeroChildComponent` inside an `*ngFor` repeater, binding its `master` string property to the child's `master` alias and each iteration's `hero` instance to the child's `hero` property.

parent-child.component.ts
```typescript
import { Component } from '@angular/core';
import { HEROES } from './hero';
@Component({
  selector: 'hero-parent',
  template: `
    <h2>{{master}} controls {{heroes.length}} heroes</h2>
    <hero-child *ngFor="let hero of heroes"
      [hero]="hero"
      [master]="master">
    </hero-child>
  `
})
export class HeroParentComponent {
  public heroes: Array<HEROES>;
  public master: string;
  
  constructor() {
    this.heroes = HEROES;
    this.master = 'Master';
  }
}
```
## Input with getter 

Use an input property setter to intercept and act upon a value from the parent.

The setter of the `name` input property in the child `NameChildComponent` trims the whitespace from a name and replaces an empty value with default text.

name-child.component.ts
```typescript
import { Component, Input } from '@angular/core';
@Component({
  selector: 'name-child',
  template: '<h3>"{{name}}"</h3>'
})
export class NameChildComponent {
  
  private _name = '';
  
  constructor() {}
  
  @Input() public set name(name: string) {
    this._name = (name && name.trim()) || '<no name set>';
  }
  
  public get name(): string { return this._name; }
}
```

Here's the NameParentComponent demonstrating name variations including a name with all spaces:

name-parent.component.ts
```typescript
import { Component } from '@angular/core';
@Component({
  selector: 'name-parent',
  template: `
  <h2>Master controls {{names.length}} names</h2>
  <name-child *ngFor="let name of names" [name]="name"></name-child>
  `
})
export class NameParentComponent {
  // Displays 'Mr. IQ', '<no name set>', 'Bombasto'
  public names = ['Mr. IQ', '   ', '  Bombasto  '];
  
  constructor() {
    
  }
}
```

## Output

Majority of child component outputs are event emitters, they should only be used when signifying any alerts or changed data. It's recommended to keep these optional. 

In this example the child component exposes an `EventEmitter` property with which it `emits` events when something happens. The parent binds to that event property and reacts to those events.

The child's EventEmitter property is an **output property**, adorned with an `@Output decoration` as seen in this `VoterComponent`:

voter-child.component.ts
```typescript
import { Component, EventEmitter, Input, Output } from '@angular/core';
@Component({
  selector: 'my-voter',
  template: `
    <h4>{{name}}</h4>
    <button (click)="vote(true)"  [disabled]="voted">Agree</button>
    <button (click)="vote(false)" [disabled]="voted">Disagree</button>
  `
})
export class VoterComponent {
  @Input() public name: string;
  @Output() public onVoted = new EventEmitter<boolean>();
  public voted = false;
  
  constructor() {
    
  }
  
  public vote(agreed: boolean) {
    this.onVoted.emit(agreed);
    this.voted = true;
  }
}
```

Clicking a button triggers emission of a true or false (the boolean payload).

The parent VoteTakerComponent binds an event handler (onVoted) that responds to the child event payload ($event) and updates a counter.

voter-parent.component.ts
```typescript
import { Component } from '@angular/core';
@Component({
  selector: 'vote-taker',
  template: `
    <h2>Should mankind colonize the Universe?</h2>
    <h3>Agree: {{agreed}}, Disagree: {{disagreed}}</h3>
    <my-voter *ngFor="let voter of voters"
      [name]="voter"
      (onVoted)="onVoted($event)">
    </my-voter>
  `
})
export class VoteTakerComponent {
  public agreed: number = 0;
  public disagreed: number = 0;
  public voters: string[] = ['Mr. IQ', 'Ms. Universe', 'Bombasto'];
  public onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
```
The framework passes the event argument — represented by `$event` — to the handler method, and the method processes it:
