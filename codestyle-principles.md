# Codestyle principles


### 1. [Use models and interfaces everywhere](#style-1)
#### Interfaces
in TypeScript are used to define the structure of an object.\
They are purely for type-checking during development. \
Interfaces can describe the shape of objects, including properties, methods, and their types.

#### Models
in TypeScript, typically refer to classes that represent data structures and include both properties and methods.\
Unlike interfaces, models can have actual implementations, including method logic, default values, and other functionality.\
Models are used at runtime and can be instantiated.

```!!!Simple words: in case you need to create objects of some type - use classes, otherwise - interfaces!!!```

### 2. [Keep your files short](#style-1)
The file length should be not more than `400` lines.
In case it's longer - split the functionality into smaller components, use services and utils.


### 3. [Use Lazy Loading for deature modules](#style-1)
It reduces the initial load time of the app and improves the overall UX.

### 4. [Manage subsriptions](#style-1)
- make sure you don't leave subscription longer than you need it. Subscriptions are clogging the RAM, impacting on the performance- unsubscribe once it's not needed to listen the observable
- unsubscribe from all observables in `ngOnDestroy` hood
- use `take(1)` pipe to get only one value from observable
- use `async` pipe in templates to handle the subscription automatically by Angular
- use subscription management libraries. E.x. [Until Destroy](https://www.npmjs.com/package/@ngneat/until-destroy)


### 5. [Use Change Detection Strategy OnPush](#style-1)
By default, Angular uses a strategy called `Default` change detection, which can be resource-intensive.\
The `OnPush` strategy detects changes only when the input properties of a component change or when an event is triggered.

### 6. [Use Reactive Forms](#style-1)
Reactive Forms allow you to handle form data in a reactive way and provide features such as validation, error handling, and dynamic form controls.


### 7. [Use Smart and Dumb Components](#style-1)
A `smart` component (container),is a component that has the responsibility of managing the state of the application and orchestrating the interactions between the components.\
A smart component is typically connected to a service or a store that manages the state of the application.\
It contains the business logic of the application and handles data fetching, transformation, and manipulation.

A `dumb` component (presentation), is a component that has no knowledge of the application state and only communicates with parent component via input data and firing events. \
Dumb components are designed to be reusable and encapsulate the presentation logic of the application. They have a simple API and are easy to test and maintain.


### 8. [Do not inject HttpClient directly](#style-1)
Create global BaseApiService

```typescript
@Injectable({
providedIn: 'root',
})
export class RestBaseService {
    protected apiUrl: string;
    constructor(protected http: HttpClient) {
      this.apiUrl = environment.Backend_Url;
    }
}
```
Use this service as a base for other API services

```typescript
export class AuthService extends RestBaseService {
  constructor(
    http: HttpClient,
  ) {
    super(http);
  }
}
```

Never write a logic to communicate with server directly in a component! 

### 9. [Use constand and enums for readonly values](#style-1)

### 10. [Use proper names for boolean variables](#style-1)
Use `is, are, should` for boolean variables: isLoading, areItemsShown, shouldReloadPage


### 11. [Write styles in style files only](#style-1)
Never write direct styles in `html` file. Use `scss` files only.

### 12. [Use appropriate function names](#style-1)
Function names should clearly describe what the function does:
- Use action verbs to describe what the function does, such as `calculateTotalPrice, fetchUserData, or validateForm`.
- Avoid using abbreviations or acronyms that might not be immediately clear to others who read your code
- Use `on` prefix to define the function which should call on some action `onButtonClick`
- User `get`, `set` prefixes: `getTotalPrice`, `setFormValue`
