# Interview Questions for UI Angular
##### What is pipe? what is difference in pure and impure pipes?

<details><summary><b>Answer</b></summary>
<p>The hero's birthday is {{ birthday | date | uppercase}}</p>

  
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
  
  
</details>
  
  

##### 
<details><summary><b>Answer</b></summary>
<p></p>
</details>


##### 
<details><summary><b>Answer</b></summary>
<p></p>
</details>
  

##### 
<details><summary><b>Answer</b></summary>
<p></p>
</details>
  

##### 
<details><summary><b>Answer</b></summary>
<p></p>
</details>
