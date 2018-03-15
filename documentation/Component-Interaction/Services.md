# Service Component Interaction

A parent component and its children share a service whose interface enables bi-directional communication within the family.

The scope of the service instance is the parent component and its children. Components outside this component subtree have no access to the service or their communications.


## When to use

Using a service for component interaction should be your last resort. It can add a lot of overhead in your application and is not always transparent to others.

There are valid reasons to use a service, for instance when persisting data between states.


### Advantages

* Capable of persisting across states/routes

### Disadvantages

* Lack of transparency
* Complexity on how the service is handled
* Difficult to maintain

## Example

A parent component and its children share a service whose interface enables bi-directional communication within the family.

The scope of the service instance is the parent component and its children. Components outside this component subtree have no access to the service or their communications.

This `MissionService` connects the `MissionControlComponent` to multiple `AstronautComponent` children.

mission.service.ts
```typescript
import { Injectable } from '@angular/core';
import { Subject }    from 'rxjs/Subject';
@Injectable()
export class MissionService {
  // Observable string sources
  private _missionAnnouncedSource = new Subject<string>();
  private _missionConfirmedSource = new Subject<string>();
  // Observable string streams
  public missionAnnounced$ = this._missionAnnouncedSource.asObservable();
  public missionConfirmed$ = this._missionConfirmedSource.asObservable();
  // Service message commands
  public announceMission(mission: string) {
    this._missionAnnouncedSource.next(mission);
  }
  confirmMission(astronaut: string) {
    this._missionConfirmedSource.next(astronaut);
  }
}
```

The `MissionControlComponent` both provides the instance of the service that it shares with its children (through the `providers` metadata array) and injects that instance into itself through its constructor:

mission-control.component.ts
```typescript
import { Component }          from '@angular/core';
import { MissionService }     from './mission.service';
@Component({
  selector: 'mission-control',
  template: `
    <h2>Mission Control</h2>
    <button (click)="announce()">Announce mission</button>
    <my-astronaut *ngFor="let astronaut of astronauts"
      [astronaut]="astronaut">
    </my-astronaut>
    <h3>History</h3>
    <ul>
      <li *ngFor="let event of history">{{event}}</li>
    </ul>
  `,
  providers: [MissionService]
})
export class MissionControlComponent {
  public astronauts: string[] = ['Lovell', 'Swigert', 'Haise'];
  public history: string[] = [];
  public missions: string[] = ['Fly to the moon!',
              'Fly to mars!',
              'Fly to Vegas!'];
  public nextMission: number = 0;
  
  constructor(private _missionService: MissionService) {
    _missionService.missionConfirmed$.subscribe(
      astronaut => {
        this.history.push(`${astronaut} confirmed the mission`);
      });
  }
  
  public announce() {
    let mission = this.missions[this.nextMission++];
    this._missionService.announceMission(mission);
    this.history.push(`Mission "${mission}" announced`);
    if (this.nextMission >= this.missions.length) { this.nextMission = 0; }
  }
}
```

The `AstronautComponent` also injects the service in its constructor. Each `AstronautComponent` is a child of the `MissionControlComponent` and therefore receives its parent's service instance:

astronaut.component.ts
```typescript
import { Component, Input, OnDestroy } from '@angular/core';
import { MissionService } from './mission.service';
import { Subscription }   from 'rxjs/Subscription';
@Component({
  selector: 'my-astronaut',
  template: `
    <p>
      {{astronaut}}: <strong>{{mission}}</strong>
      <button
        (click)="confirm()"
        [disabled]="!announced || confirmed">
        Confirm
      </button>
    </p>
  `
})
export class AstronautComponent implements OnDestroy {
  @Input() public astronaut: string;
  public mission = '<no mission announced>';
  public confirmed = false;
  public announced = false;
  public subscription: Subscription;
  constructor(private _missionService: MissionService) {
    this.subscription = _missionService.missionAnnounced$.subscribe(
      mission => {
        this.mission = mission;
        this.announced = true;
        this.confirmed = false;
    });
  }
  
  public ngOnDestroy() {
    // prevent memory leak when component destroyed
    this.subscription.unsubscribe();
  }
  
  public confirm(): void {
    this.confirmed = true;
    this._missionService.confirmMission(this.astronaut);
  }
}
```

```
Notice that we capture the subscription and unsubscribe when the AstronautComponent is destroyed. This is a memory-leak guard step. There is no actual risk in this app because the lifetime of a AstronautComponent is the same as the lifetime of the app itself. That would not always be true in a more complex application.

We do not add this guard to the MissionControlComponent because, as the parent, it controls the lifetime of the MissionService.
```

The History log demonstrates that messages travel in both directions between the parent `MissionControlComponent` and the `AstronautComponent` children, facilitated by the service:
