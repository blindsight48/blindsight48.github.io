<?php
/*
Codebase Organization Preface

### **Core Philosophy**
This codebase follows a modular design that separates responsibilities between controllers, models, views, and data files. By adhering to these principles, the system becomes:
- **Easier to maintain**: Each component has a clear, single responsibility.
- **Simpler to debug**: Issues are isolated to specific layers.
- **Scalable**: New features can be added without disrupting existing functionality.

---

### **Controllers (Router Classes)**
- Controllers act as navigators or "routers," directing requests to the appropriate models and views.
- Their responsibilities include:
  1. Handling user input and deciding what to do based on the request.
  2. Invoking methods in models or helpers to process logic or data.
  3. Passing processed data to views for rendering.
  4. Rendering views or returning responses (e.g., JSON for APIs).
- Controllers **must not contain raw code** beyond flow control blocks (`if`, `switch`, `foreach`) or method calls.

Controllers should:
- Inherit a **base controller class** for shared functionality (e.g., rendering views, redirects).
- Avoid duplicating logic by offloading data processing to models.

---

### **Model Classes**
- Models are responsible for handling all business logic, database interactions, and data manipulation.
- Key characteristics:
  1. Operate independently, assuming the conditions for their execution are met.
  2. Focus on positive, actionable statements (e.g., "do this if true").
  3. Can include conditionals and loops necessary for data processing.
- Models should not directly interact with views.

---

### **View Files**
- Views are solely responsible for rendering data and presenting it to the user.
- Key rules:
  1. Do not contain business logic or interact directly with models.
  2. Use only data passed to them by controllers.
  3. Escape all output to ensure security (e.g., `htmlspecialchars`).
  4. May include reusable partials for common UI components (e.g., headers, footers).

For more structure, consider using a templating engine like Twig or Blade.

---

### **Data Files**
- Contain static values, configurations, or settings required across the application.
- Should be placed in a dedicated directory (e.g., `/data`).
- Avoid mixing logic or dynamic code with static data files.

---

### **Separation of Concerns**
By adhering to this structure:
- **Controllers** act as routers, managing application flow and connecting models to views.
- **Models** focus on data and logic.
- **Views** handle presentation.
- **Data files** store configuration and constants.

This separation ensures that each layer is clear in its role, leading to a clean, maintainable, and scalable codebase.

---

### **Example of a PHP Request Execution**
Consider a web application with a feature to display a car's fuel status:

1. A user requests the "Check Fuel" page via a browser.
2. The **router** directs the request to the appropriate controller, e.g., `CarController`.
3. The controller:
   - Calls a model method, e.g., `FuelModel::getFuelStatus()`, to fetch fuel data from the database.
   - Passes the fetched data to a view, e.g., `fuel_status.php`, for rendering.
4. The view receives the data and displays the fuel level to the user.

---

### **Analogy: The Car System**
Think of the application as a car system to understand how the components interact:
- **Data Files (Fuel/Gas)**:
  - Represent the raw material (fuel) required to drive the car. They store static configurations or values.
- **Models (Pedals, Buttons, Engine)**:
  - Handle the actual operation of the car. The pedals and buttons execute logic (e.g., accelerating or braking) based on inputs.
- **Controller (Driver/Person)**:
  - Acts as the decision-maker, deciding when to accelerate, brake, or turn. They interact with the pedals and buttons to control the car.
- **View (Dashboard)**:
  - Displays information to the driver (e.g., speed, fuel level) but doesn't influence the car's operation.

---

### **Code Example: Car Fuel Status**
**Controller**:
```php
class CarController extends Controller {
    public function checkFuel(): void {
        $fuelModel = new FuelModel();
        $fuelStatus = $fuelModel->getFuelStatus(); // Fetch the fuel level from the database.
        
        $this->renderView('fuel_status', ['fuelLevel' => $fuelStatus]);
    }
}
