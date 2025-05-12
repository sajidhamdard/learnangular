## **What is Dependency Injection (DI) in Angular?**

**Dependency Injection (DI)** is a design pattern and a core concept in Angular that allows **components, services, and other classes** to receive their dependencies from an external source rather than creating them internally.

In simpler terms, **Dependency Injection** in Angular means **passing objects or services into a class** (like a component or service) instead of the class creating these objects or services on its own.

---

### **How DI Works in Angular:**

In Angular, DI is primarily used to inject services into components, directives, pipes, or other services. Angular's **dependency injection system** makes it easy to manage services and ensures that they are **shared, singleton**, and **reusable** across the application.

---

### **Example of Dependency Injection:**

Consider a scenario where you have a service called `AuthService` that is used in a component called `LoginComponent`.

#### **1. Creating a Service (`AuthService`)**

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root', // Service is provided globally
})
export class AuthService {
  login() {
    console.log('User logged in');
  }
}
```

Here, the `AuthService` is **injectable** and provided globally using `providedIn: 'root'`.

#### **2. Injecting the Service into a Component (`LoginComponent`)**

```ts
import { Component } from '@angular/core';
import { AuthService } from './auth.service'; // Importing the service

@Component({
  selector: 'app-login',
  template: `<button (click)="login()">Login</button>`,
})
export class LoginComponent {
  constructor(private authService: AuthService) {} // DI in action

  login() {
    this.authService.login(); // Using the injected service
  }
}
```

In this example:

* **`AuthService`** is injected into the `LoginComponent` through the **constructor**.
* Angular automatically **instantiates `AuthService`** and provides it to the `LoginComponent`.

---

### **How Dependency Injection Works in Angular:**

1. **Service Registration**:

   * When a service (like `AuthService`) is created, Angular registers it in its **dependency injection container**. You can specify where the service should be provided (root, module, or component).
2. **Service Injection**:

   * When a component or service (like `LoginComponent`) needs the service, Angular's DI system **injects** the instance of the service into the component or service’s **constructor**.
3. **Singleton Instances**:

   * By default, Angular creates a **single instance** of the service and reuses it wherever it’s injected (if the service is provided in the root module or with `providedIn: 'root'`).

---

### **Benefits of Dependency Injection in Angular**

1. **Decouples Components from Services**:

   * **DI decouples the component** from the logic of creating the dependencies. This makes the code **cleaner**, **modular**, and easier to maintain because components don’t need to know how to instantiate services.

2. **Testability**:

   * **DI promotes testing** because it allows you to easily **mock services** while writing unit tests. Instead of having real instances of services, you can inject **mock services** to test components in isolation.

   Example of mocking `AuthService` for testing:

   ```ts
   import { TestBed } from '@angular/core/testing';
   import { LoginComponent } from './login.component';
   import { AuthService } from './auth.service';
   import { of } from 'rxjs';

   class MockAuthService {
     login() {
       return of('mock login');
     }
   }

   describe('LoginComponent', () => {
     let component: LoginComponent;

     beforeEach(() => {
       TestBed.configureTestingModule({
         declarations: [LoginComponent],
         providers: [{ provide: AuthService, useClass: MockAuthService }],
       });

       component = TestBed.createComponent(LoginComponent).componentInstance;
     });

     it('should call login', () => {
       spyOn(component, 'login');
       component.login();
       expect(component.login).toHaveBeenCalled();
     });
   });
   ```

3. **Singleton Services**:

   * With DI, services like `AuthService` can be **singleton** objects, ensuring that there is **only one instance** of the service in the entire application, which helps with **resource management**.

4. **Reusability**:

   * You can create services that are used across multiple components, which improves **code reuse** and avoids duplicating logic across the app.

5. **Ease of Maintenance**:

   * If you need to change a service’s logic, you can do so without needing to update every component that uses the service. You only modify the service itself, and all components using it will automatically benefit from the update.

6. **Flexibility**:

   * DI allows you to **inject multiple implementations** of a service, depending on the context. For example, you can provide different implementations of a service in different modules, depending on the needs of the application.

---

### **How Angular Handles DI:**

1. **Injectors**:

   * Angular uses **injectors** to manage dependencies. When a component or service is created, Angular’s injector checks its constructor for dependencies and injects the required services.

2. **Hierarchical DI**:

   * Angular has a **hierarchical DI system**, meaning that you can provide services at different levels:

     * **Root-level**: Services are available globally throughout the app (using `providedIn: 'root'`).
     * **Module-level**: Services are available only within a specific module (provided in the module's `providers` array).
     * **Component-level**: Services are scoped to a specific component (provided in the component’s `providers` array).

---

### **Key Terms in DI:**

1. **Provider**:

   * A provider is a **configuration** that tells Angular how to create a service or a dependency. You can configure providers in an `NgModule` or `Component` via the `providers` array.

2. **Injector**:

   * An injector is an object that is responsible for creating instances of services and providing them to the components or services that need them.

3. **Token**:

   * A token is used to uniquely identify a service or dependency in Angular's DI system. The token can be a **class**, **string**, or **`InjectionToken`**.

---

### **Example of Hierarchical DI System:**

You can provide a service at the **module level** so it is only available to components within that module, or provide it at the **component level** so it's available only to that component and its descendants.

```ts
@NgModule({
  providers: [SomeService],  // Service is available within this module only
})
export class SomeModule {}
```

If you provide a service **inside a component**, it will be available only within that component and its child components.

```ts
@Component({
  selector: 'app-my-component',
  providers: [SomeService],  // Service is available only within this component and its children
})
export class MyComponent {}
```

---

### **Summary:**

* **Dependency Injection (DI)** is a design pattern in Angular where services or dependencies are **injected** into classes (like components or services) rather than being created by those classes.
* DI helps in **decoupling** components from services, making code **modular**, **testable**, and **maintainable**.
* Angular provides a **hierarchical DI system**, allowing services to be scoped to different levels (root, module, component).
* DI promotes **code reuse**, **testability**, and **flexibility** in how services are provided and managed within Angular applications.

---

### **What is `providedIn: 'root'`?**

`providedIn: 'root'` is a **configuration option** used in Angular’s **dependency injection** system. It specifies that a service should be available **globally** as a singleton and will be injected wherever it’s needed across the application.

---

### **How Does It Work?**

When you use **`providedIn: 'root'`** in a service, Angular will create **one instance** of that service for the entire application. This instance is then shared across all components, directives, and other services that inject it.

Here’s the syntax:

```ts
@Injectable({
  providedIn: 'root'  // Service is available app-wide as a singleton
})
export class MyService {
  constructor() { }
}
```

In this case:

* **`MyService`** will be created once (when it's needed for the first time).
* Angular will manage the instance of **`MyService`**, ensuring there is only **one instance** shared across the app.

---

### **Why Use `providedIn: 'root'`?**

1. **Singleton Pattern**:

   * **`providedIn: 'root'`** ensures that **only one instance** of the service is created and shared across the entire application.
   * This makes the service **stateful** and available globally.

2. **Tree-shakable**:

   * Angular is **tree-shakable**, which means that if a service is not used in your app, it will be **removed from the final bundle** during the build process. This reduces the size of the app, improving performance.
   * **`providedIn: 'root'`** is **tree-shakable**: if you don’t use the service anywhere, Angular will automatically exclude it from the final build.

3. **No Need to Add to `providers` Array**:

   * If you use `providedIn: 'root'`, you don’t need to manually add the service to the `providers` array in any module (like `AppModule` or other feature modules). Angular will automatically take care of providing the service globally.

---

### **Alternative Scoping with `providedIn`**

In addition to **`root`**, you can also scope a service to a **specific module** using `providedIn` with the module name:

1. **`providedIn: 'any'`**:

   * When you use **`providedIn: 'any'`**, Angular will provide a new instance of the service for each lazy-loaded module. This is useful if you want **module-scoped** services that are created per lazy-loaded module.

2. **Module-level `providedIn`**:

   * You can also use `providedIn` at the module level (in an `NgModule`’s `providers` array) if you need the service to be available only in specific modules. However, using **`providedIn: 'root'`** is the preferred approach for most use cases.

---

### **Example: Using `providedIn: 'root'`**

Here’s an example with `providedIn: 'root'`:

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // This service will be a singleton throughout the app
})
export class AuthService {
  constructor() { }

  login() {
    console.log('User logged in');
  }
}
```

* This means **`AuthService`** will be created only once and shared throughout the application.
* You can inject `AuthService` into any component, service, or module, and it will always refer to the same instance.

```ts
import { Component } from '@angular/core';
import { AuthService } from './auth.service';

@Component({
  selector: 'app-root',
  template: `<button (click)="login()">Login</button>`
})
export class AppComponent {
  constructor(private authService: AuthService) {}

  login() {
    this.authService.login();  // Calls the singleton instance
  }
}
```

---

### **Benefits of Using `providedIn: 'root'`**

1. **Global Availability**:
   Services provided in `root` are available globally across the app, without needing to explicitly add them to the `providers` array of each module.

2. **Singleton Service**:
   The service is **instantiated only once** and shared across the entire application.

3. **Optimized Build**:
   Angular can **tree-shake** unused services, meaning the service won’t be bundled if it’s never used, reducing the size of your app.

4. **Simpler Code**:
   No need to manually add services to the `providers` array in the `@NgModule` decorator for the **root module** or any other module.

---

### **When Should You Use `providedIn: 'root'`?**

* **Use it for global services**: Services that are meant to be shared app-wide (like authentication, logging, data caching, etc.).
* **For singleton services**: If you want a **single instance** of the service used throughout the app, `providedIn: 'root'` is ideal.
* **For performance optimization**: Since it's tree-shakable, it will help reduce the final bundle size if the service isn't used anywhere.

---

### **Summary**

* **`providedIn: 'root'`** makes the service available globally as a singleton and eliminates the need to add it to the `providers` array in modules.
* It is **tree-shakable**, ensuring unused services are excluded from the final build.
* Use it for services that should be available throughout the application, like authentication or shared data services.

---
