# ngOnChanges

### *Intercept input property changes with ngOnChanges*

Detect and act upon changes to input property values with the `ngOnChanges` method of the `OnChanges` lifecycle hook interface.

## When to use

Majority of child components will be using the `input` and `output` interaction patterns. It is the most maintainable method and also helps enforce reusability.
 
### Advantages

* Decoupled from parent components (**Reusable**)
* Input/Output black box, but functionality lies in the OnChange lifecycle.

### Disadvantages

* Limited to only receiving data synchronously from parent
* Simple, cannot do complex operations
* Nested object changes are known to not work
* Input & Output pattern still valid however harder to document functionality, logic is now nested in the ngChange lifecycle.

## Example

This `VersionChildComponent` detects changes to the `major` and `minor` input properties and composes a log message reporting these changes:

```typescript
import { Component, Input, OnChanges, SimpleChange } from '@angular/core';
@Component({
  selector: 'version-child',
  template: `
    <h3>Version {{major}}.{{minor}}</h3>
    <h4>Change log:</h4>
    <ul>
      <li *ngFor="let change of changeLog">{{change}}</li>
    </ul>
  `
})
export class VersionChildComponent implements OnChanges {
  @Input() public major: number;
  @Input() public minor: number;
  public changeLog: string[] = [];
  public ngOnChanges(changes: {[propKey: string]: SimpleChange}) {
    let log: string[] = [];
    for (let propName in changes) {
      let changedProp = changes[propName];
      let from = JSON.stringify(changedProp.previousValue);
      let to =   JSON.stringify(changedProp.currentValue);
      log.push( `${propName} changed from ${from} to ${to}`);
    }
    this.changeLog.push(log.join(', '));
  }
}
```

The `VersionParentComponent` supplies the `minor` and `major` values and binds buttons to methods that change them.

```typescript
import { Component } from '@angular/core';
@Component({
  selector: 'version-parent',
  template: `
    <h2>Source code version</h2>
    <button (click)="newMinor()">New minor version</button>
    <button (click)="newMajor()">New major version</button>
    <version-child [major]="major" [minor]="minor"></version-child>
  `
})
export class VersionParentComponent {
  public major: number = 1;
  public minor: number = 23;
  
  public newMinor() {
    this.minor++;
  }
  
  public newMajor() {
    this.major++;
    this.minor = 0;
  }
}
```
