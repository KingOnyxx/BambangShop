# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Observer Pattern and Interface/Trait: In the Observer pattern, the Subscriber is often defined as an interface (or a trait in Rust) to allow for multiple concrete implementations. This provides flexibility as it allows any object that implements the Subscriber interface to be an observer.
2. Unique id and url - Vec vs DashMap: If id in Program and url in Subscriber are intended to be unique, using a DashMap (or a HashMap in general) would be more efficient than a Vec (or a list). This is because lookup, insertion, and deletion operations in a HashMap are generally faster (average time complexity of O(1)) than in a list (average time complexity of O(n)). So, if you frequently need to check for uniqueness or look up subscribers by id or url, DashMap would be a better choice.
3. Thread Safety - DashMap vs Singleton Pattern: In Rust, the DashMap library provides a thread-safe HashMap which is useful when you have multiple threads that might be reading and writing to the SUBSCRIBERS variable concurrently. The Singleton pattern ensures that a class has only one instance and provides a global point of access to it. However, it doesn’t inherently provide thread safety. If you want to ensure both that there’s only one instance of the list of subscribers and that it’s thread-safe, you could use a Singleton pattern to create the list and still use DashMap for the list to ensure thread safety. But if thread safety is not a concern, a regular HashMap could be used.

#### Reflection Publisher-2
1. The separation of “Service” and “Repository” from a Model is based on the principle of Single Responsibility and Separation of Concerns. In a complex application, the Model can become bloated if it’s responsible for both data storage and business logic. The Repository handles the data storage, making it easier to manage data persistence and retrieval, and allowing for potential optimizations related to data access. The Service handles the business logic, making it easier to manage and test the application’s behavior. This improves the modularity and maintainability of the code, as changes in data storage implementation or business logic can be made independently without affecting each other.
2. If we only use the Model without separating “Service” and “Repository”, the interactions between each model (Program, Subscriber, Notification) could significantly increase the code complexity. Each model would need to handle its own data storage and business logic, leading to potential code duplication and making the code harder to maintain and test. It could also make it more difficult to ensure data consistency and integrity across models.
3. Postman is a powerful tool for testing API endpoints. It allows you to send various types of HTTP requests (GET, POST, PUT, DELETE, etc.) to the API and view the responses in real-time, which can greatly speed up the development and debugging process. Some of the features in Postman that could be helpful for Group Project or future software engineering projects because it can group related requests together for better organization and collaboration, define variables that can be used across multiple requests, making it easier to manage common values like API base URLs or authentication tokens. I can write tests for API responses and automate these tests to run on different stages of development workflow. Postman can automatically generate and update API documentation, making it easier to share your API with others.

#### Reflection Publisher-3
