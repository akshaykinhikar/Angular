# Interview Questions for UI Angular
##### What is pipe? what is difference in pure and impure pipes?

<details><summary><b>Answer</b></summary>
<p>Suppose we want to perform some operation on array/element and return subset of array/element by performing some operation, in such scenario 
  we use pipe to filter or modify array/element and display corresponsing row/rows in UI.
  
The hero's birthday is {{ birthday | date | uppercase}}</p>

  
  ##### Creating pipes for custom data transformations
```javascript
  import { Pipe, PipeTransform } from '@angular/core';
/*
 * Raise the value exponentially
 * Takes an exponent argument that defaults to 1.
 * Usage:
 *   value | exponentialStrength:exponent
 * Example:
 *   {{ 2 | exponentialStrength:10 }}
 *   formats to: 1024
*/
@Pipe({name: 'exponentialStrength', pure: false})
export class ExponentialStrengthPipe implements PipeTransform {
  transform(value: number, exponent = 1): number {
    return Math.pow(value, exponent);
  }
} 
```
  <p> Angular executes an impure pipe every time it detects a change with every keystroke or mouse movement.</p>
</details>


##### What is interceptor?
<details><summary><b>Answer</b></summary>
<p>Interceptors are a unique type of Angular Service that we can implement. Interceptors allow us to intercept incoming or outgoing HTTP requests using the HttpClient . By intercepting the HTTP request, we can modify or change the value of the request.</p>
  
  ```javascript
  import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpEvent, HttpResponse, HttpRequest, HttpHandler, HttpErrorResponse } from '@angular/common/http';
import { Observable } from 'rxjs';
import { retry } from 'rxjs/operators';

@Injectable()
export class RetryInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(httpRequest).pipe(retry(2));
  }
}
  ```javascript
  <p> In app module, provide interceptor </p>
```
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: MyInterceptor, multi: true }
  ]
```
  <p>source: https://ultimatecourses.com/blog/intro-to-angular-http-interceptors</p>
</details>


##### What are the advantages of using HttpClient?
<details><summary><b>Answer</b></summary>
<ul>
  <li>Included Testability Features.</li>
  <li>Typed Requests and Response Objects.</li>
  <li>Requests and Response Interception.</li>
  <li>Observable APIs and a method of streamlined and efficient error handling.</li>
</ul>
</details>

##### what is promise and observables? 
<details><summary><b>Answer</b></summary>
Observables
  <ul>
  <li>Emit multiple values over a period of time.</li>
  <li>Are lazy</li>
  <li>ewe can unsubscribe()</li>
  <li>Provide the map for forEach, filter, reduce, retry, and retryWhen operators</li>
   <li>Deliver errors to the subscribers.</li>
</ul>
  
  promise
  <ul>
  <li>Emit a single value at a time.</li>
  <li>Are not lazy: execute immediately after creation.</li>
  <li>Are not cancellable.</li>
  <li>Don’t provide any operations.</li>
  <li>Push errors to the child promises.</li>
</ul>
  
</details>

##### Different way of communication between components in angular?
<details><summary><b>Answer</b></summary>
  <p>Pass data from parent to child with input binding</p>
  
  ```javascript 
    import { Component, Input } from '@angular/core';
    import { Hero } from './hero';

    @Component({
    selector: 'app-hero-child',
    template: '
        <h3>{{hero.name}} says:</h3>
        <p>I, {{hero.name}}, am at your service, {{masterName}}.</p>
    '
    })
    export class HeroChildComponent {
    @Input() hero!: Hero;
    @Input('master') masterName = ''; // tslint:disable-line: no-input-rename
    }
  ```
  
  ```javascript
  
  import { Component } from '@angular/core';

import { HEROES } from './hero';

@Component({
  selector: 'app-hero-parent',
  template: `
    <h2>{{master}} controls {{heroes.length}} heroes</h2>

    <app-hero-child
      *ngFor="let hero of heroes"
      [hero]="hero"
      [master]="master">
    </app-hero-child>
  `
})
export class HeroParentComponent {
  heroes = HEROES;
  master = 'Master';
}
  
  ```
<p> Intercept input property changes with a setter </p>
  
  ```javascript
  import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-name-child',
  template: '<h3>"{{name}}"</h3>'
})
export class NameChildComponent {
  @Input()
  get name(): string { return this._name; }
  set name(name: string) {
    this._name = (name && name.trim()) || '<no name set>';
  }
  private _name = '';
}
  
  ```
 ```javascript
  import { Component } from '@angular/core';

@Component({
  selector: 'app-name-parent',
  template: `
    <h2>Master controls {{names.length}} names</h2>

    <app-name-child *ngFor="let name of names" [name]="name"></app-name-child>
  `
})
export class NameParentComponent {
  // Displays 'Dr IQ', '<no name set>', 'Bombasto'
  names = ['Dr IQ', '   ', '  Bombasto  '];
}
  ```

<p>Intercept input property changes with ngOnChanges()</p>

  ```javascript
  import { Component, Input, OnChanges, SimpleChanges } from '@angular/core';

@Component({
  selector: 'app-version-child',
  template: `
    <h3>Version {{major}}.{{minor}}</h3>
    <h4>Change log:</h4>
    <ul>
      <li *ngFor="let change of changeLog">{{change}}</li>
    </ul>
  `
})
export class VersionChildComponent implements OnChanges {
  @Input() major = 0;
  @Input() minor = 0;
  changeLog: string[] = [];

  ngOnChanges(changes: SimpleChanges) {
    const log: string[] = [];
    for (const propName in changes) {
      const changedProp = changes[propName];
      const to = JSON.stringify(changedProp.currentValue);
      if (changedProp.isFirstChange()) {
        log.push(`Initial value of ${propName} set to ${to}`);
      } else {
        const from = JSON.stringify(changedProp.previousValue);
        log.push(`${propName} changed from ${from} to ${to}`);
      }
    }
    this.changeLog.push(log.join(', '));
  }
}
  ```
  ```javascript
  import { Component } from '@angular/core';

@Component({
  selector: 'app-version-parent',
  template: `
    <h2>Source code version</h2>
    <button (click)="newMinor()">New minor version</button>
    <button (click)="newMajor()">New major version</button>
    <app-version-child [major]="major" [minor]="minor"></app-version-child>
  `
})
export class VersionParentComponent {
  major = 1;
  minor = 23;

  newMinor() {
    this.minor++;
  }

  newMajor() {
    this.major++;
    this.minor = 0;
  }
}
  ```
  
<p>Parent listens for child event</p>
  
  ```javascript
  import { Component, EventEmitter, Input, Output } from '@angular/core';

@Component({
  selector: 'app-voter',
  template: `
    <h4>{{name}}</h4>
    <button (click)="vote(true)"  [disabled]="didVote">Agree</button>
    <button (click)="vote(false)" [disabled]="didVote">Disagree</button>
  `
})
export class VoterComponent {
  @Input()  name = '';
  @Output() voted = new EventEmitter<boolean>();
  didVote = false;

  vote(agreed: boolean) {
    this.voted.emit(agreed);
    this.didVote = true;
  }
}
  ```
  ```javascript
  import { Component } from '@angular/core';

@Component({
  selector: 'app-vote-taker',
  template: `
    <h2>Should mankind colonize the Universe?</h2>
    <h3>Agree: {{agreed}}, Disagree: {{disagreed}}</h3>

    <app-voter
      *ngFor="let voter of voters"
      [name]="voter"
      (voted)="onVoted($event)">
    </app-voter>
  `
})
export class VoteTakerComponent {
  agreed = 0;
  disagreed = 0;
  voters = ['Narco', 'Celeritas', 'Bombasto'];

  onVoted(agreed: boolean) {
    agreed ? this.agreed++ : this.disagreed++;
  }
}
  ```
  
<p>Parent interacts with child using local variable</p>

  ```javascript
  import { Component } from '@angular/core';
import { CountdownTimerComponent } from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-lv',
  template: `
    <h3>Countdown to Liftoff (via local variable)</h3>
    <button (click)="timer.start()">Start</button>
    <button (click)="timer.stop()">Stop</button>
    <div class="seconds">{{timer.seconds}}</div>
    <app-countdown-timer #timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownLocalVarParentComponent { }
  ```
  <p>Place a local variable, #timer, on the tag <countdown-timer> representing the child component. That gives you a reference to the child component and the ability to access any of its properties or methods from within the parent template. </p>
    
    <p>Parent calls an @ViewChild()</p>
  
  ```javascript
  import { AfterViewInit, ViewChild } from '@angular/core';
import { Component } from '@angular/core';
import { CountdownTimerComponent } from './countdown-timer.component';

@Component({
  selector: 'app-countdown-parent-vc',
  template: `
    <h3>Countdown to Liftoff (via ViewChild)</h3>
    <button (click)="start()">Start</button>
    <button (click)="stop()">Stop</button>
    <div class="seconds">{{ seconds() }}</div>
    <app-countdown-timer></app-countdown-timer>
  `,
  styleUrls: ['../assets/demo.css']
})
export class CountdownViewChildParentComponent implements AfterViewInit {

  @ViewChild(CountdownTimerComponent)
  private timerComponent!: CountdownTimerComponent;

  seconds() { return 0; }

  ngAfterViewInit() {
    // Redefine `seconds()` to get from the `CountdownTimerComponent.seconds` ...
    // but wait a tick first to avoid one-time devMode
    // unidirectional-data-flow-violation error
    setTimeout(() => this.seconds = () => this.timerComponent.seconds, 0);
  }

  start() { this.timerComponent.start(); }
  stop() { this.timerComponent.stop(); }
}
  ```
 
<p>Parent and children communicate using a service</p>
  
 ```javascript
  
  ```
    
</details>
  
  

#####  What is subject and its types?
<details><summary><b>Answer</b></summary>
<p></p>
</details>


##### Difference between Angularjs and Angular 2+?
<details><summary><b>Answer</b></summary>
<ul>
  <li>removed $rootscope</li>
  <li>better compilation</li>
  <li>cli added</li>
  <li>component based structure</li>
  <li>es5/6 introduced to support Object Oriented approach</li>
</ul>
</details>
  

##### How to many ways are to create decorators?
<details><summary><b>Answer</b></summary>
<ol>
  <li><b>Class decorator</b> @component @module </li>
  <li><b>Property</b> decorator @input @output </li>
  <li><b>Method</b> decorator @hostlistener </li>
  <li><b>Parameter</b> @inject </li>
<ol>
</details>
  

##### How will your application run fast based on performance?
<details><summary><b>Answer</b></summary>
<ul>
  <li> remove change detection</li>
  <li> AOT </li>
  <li> lazy loading </li>
  <li> server side rendering </li>
  <li> web services to catching data </li>
</details>
