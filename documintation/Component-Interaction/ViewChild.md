# View Child

The `view child` interaction pattern is similar to the `local variable` pattern, however **under no circumstances** should you use inline local variables on your views. Using inline local values is dangerous as it is completely hidden from teams and difficult to debug.

## When to use

Using the `ViewChild` decorator should only be used in rare circumstances when child components are so complex you require an instance of the class to be available.

If `Input/Ouput` and `ngOnChanges` are overbearing to maintain then using `ViewChild` can help control the complexity of the child component.

### Advantages

* Provide parents more control 

### Disadvantages

* Couples child component to parent, the component is no longer reusable
* Poor design, difficult to maintain
* If using *ngFor or showing multiple child components then use ViewChildren

## Example

Using `ViewChild` we can `inject` the child component into the parent.

**Child Component**
```typescript
import { Component, OnDestroy, OnInit } from '@angular/core';
@Component({
  selector: 'countdown-timer',
  template: '<p>{{message}}</p>'
})
export class CountdownTimerComponent implements OnInit, OnDestroy {
  public intervalId: number;
  public message: string;
  public seconds: number;
  
  constructor() {
    this.intervalId = 0;
    this.message = '';
    this.seconds = 11;
  }
  
  public ngOnInit() {
    this.start();
  }
  
  public ngOnDestroy() {
    this.clearTimer();
  }
  
  public clearTimer() {
     clearInterval(this.intervalId);
  }
  
  public start() {
    this.countDown();
  }
  
  public stop()  {
    this.clearTimer();
    this.message = `Holding at T-${this.seconds} seconds`;
  }
  
  private countDown() {
    this.clearTimer();
    this.intervalId = window.setInterval(() => {
      this.seconds -= 1;
      if (this.seconds === 0) {
        this.message = 'Blast off!';
      } else {
        if (this.seconds < 0) { this.seconds = 10; } // reset
        this.message = `T-${this.seconds} seconds and counting`;
      }
    }, 1000);
  }
}
```

**Parent Component**
```typescript
import { AfterViewInit, ViewChild } from '@angular/core';
import { Component }                from '@angular/core';
import { CountdownTimerComponent }  from './countdown-timer.component';
@Component({
  selector: 'countdown-parent-vc',
  template: `
  <h3>Countdown to Liftoff (via ViewChild)</h3>
  <button (click)="start()">Start</button>
  <button (click)="stop()">Stop</button>
  <div class="seconds">{{ seconds() }}</div>
  <countdown-timer></countdown-timer>
  `,
  styleUrls: ['./views/demo.css']
})
export class CountdownViewChildParentComponent implements AfterViewInit {
  @ViewChild(CountdownTimerComponent) private _timerComponent: CountdownTimerComponent;
  
  constructor() {
    
  }
  
  public ngAfterViewInit() {
    // Redefine `seconds()` to get from the `CountdownTimerComponent.seconds` ...
    // but wait a tick first to avoid one-time devMode
    // unidirectional-data-flow-violation error
    setTimeout(() => this.seconds = () => this._timerComponent.seconds, 0);
  }
  
  public seconds(): number {
    return 0;
  }
  
  public start(): void {
    this._timerComponent.start();
  }
  
  public stop(): void {
    this._timerComponent.stop();
  }
}
```
