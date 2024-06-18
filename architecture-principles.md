# Architecture Principles

This part explains the basic architecture principles: 
1. Splitting into modules
2. Folder structure
3. Naming conventions
4. Usage of shared resources


### [Splitting into modules](#style-1)

Modules - it's a cohesive blocks of functionality.\
Use modular architecture.

Types:

1. **Core Module**: contains stuff needed for communication with server:
    - Singleton API services 
    - Singleton services which will be used throw the whole application (local storage service, responsive design service)
    - Interceptors related to Authentication logic (jwt, global error handling)
    - Guards related to Authentication logic (auth, roles)
    
2. **Feature Modules**: represent distinct features or sections of the application. In most cases requires Lazy Loading usage.
3. **Shared Module**: contains common components, directives, and pipes used across multiple modules. This helps in avoiding code duplication.


### [Folder structure](#style-1)

For easy maintenance and understanding we should follow the same folder structure for all modules. 


**Principles**:
1. Use tree-based folder structure: follow the same folder structure for all modules

2. Keep files of the same purpose in related folder: all pipes in `pipes` folder, all directives in `directives` folder etc.

4. Put common functions which doesn't require separate service in `utils` folder with `util` postfix: `calculate-tital-price.util`

5. Global styles: split global styles into separate files by it\'s purpose: `variables.scss`, `mixins.scss`, `responsive.scss`, `form-elements.scss`, `buttons.scss`

6. Keep global style files under `assets/style` folder. Do not import them into component style files!!!. Global styles should be imported globally via `style.scss` file


Example of file structure:
```
src/
  app/
    core/
      services/
      guards/
      interceptors/
    modules(features,pages)/
      feature1/
        components/
        enums/
        services/
        utils/
        feature1.module.ts
        feature1-routing.module.ts
      feature2/
        components/
        consts/
        services/
        feature2.module.ts
        feature1-routing.module.ts
    shared/
      components/
      directives/
      enums/
      consts/
      pipes/
      models/
      interfaces/
      utils/
      shared.module.ts
    app-routing.module.ts
    app.component.ts
    app.module.ts
  assets/
    style/
    images/
        icons/
        feature1/
  environments/
  styles/
  index.html
  main.ts
  polyfills.ts
  styles.css
```

### [Naming Conventions](#style-1)
- **Components**: Use the feature or purpose followed by Component. For example, UserProfileComponent, LoginComponent.
- **Services**: Use the feature or purpose followed by Service. For example, AuthService, UserService.
- **Directives**: Use the feature or behavior followed by Directive. For example, HighlightDirective, ScrollDirective.
- **Pipes**: Use the transformation followed by Pipe. For example, DateFormatPipe, CurrencyPipe.
- File names should follow a kebab-case convention and include the type of the Angular artifact they represent: `date.pipe.ts`, `auth.interceptor.ts`, `user-role.enum.ts`, `default-user-type.const.ts` `user.model.ts`, `card.interface.ts` (read about difference under models and interfaces below)
- Give clear names based on the file purpose. Don't use too short names to avoid duplication. Don't use too long names to not overload the project. Keep the middle ground :)


**Bad** ❌:
```
info.component (What info? What is it's purpose?)
global-user-information-card-for-dashboard-module.component (global-user-info is enaught)
 
```

**Good** ✅:
```
account-header.component
completed-trips-list.component
```

### [Difference between interface and model](#style-1)

#### Interfaces
in TypeScript are used to define the structure of an object.\
They are purely for type-checking during development. \
Interfaces can describe the shape of objects, including properties, methods, and their types.

#### Models 
in TypeScript, typically refer to classes that represent data structures and include both properties and methods.\
Unlike interfaces, models can have actual implementations, including method logic, default values, and other functionality.\
Models are used at runtime and can be instantiated.

```!!!Simple words: in case you need to create objects of some type - use classes, otherwise - interfaces!!!```

### [Usage of shared resources](#style-1)
- Keep common resources which are used into different places under `SharedModule`
- Avoid importing `SharedModule` into `AppModule` to prevent loading shared resources eagerly and unnecessarily.
- If a large feature is used in multiple places (e.g., the "Edit User" feature is used in two different modules), it's better to create a separate module for it. This way, you don't have to include it in the SharedModule and import it everywhere, making your application more efficient.
- Create shared components for the common elements:
  - buttons
  - form elements
  - cards
  - text elements: headers, articles
  - badges
  - avatars
  - alerts
  - error messages
  
- Create global services for handling:
  - interaction with Local Storage
  - interaction with Session Storage
  - global error handling mechanism
  - service to interact with alerts/toasts

