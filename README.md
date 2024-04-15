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
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1.  Seems like a single model struct will still do well (for now). However, I believe using an interface (or trait in Rust) is still beneficial in BambangShop case. It will allow us to define common methods that all scubscribers should implement, so other parts of the system can interact with subscribers without knowing their specific implementations. This will make the system more flexible and extensible, as new types of subscribers can be added without modifying the subject. The specific implementation of the subscriber can be defined in the concrete subscriber classes that implement the interface.
2.  Using Vec (list) won't be sufficiend. A Vec allows duplicate values, so we need to manually performs check for duplicates when adding to the list. Using DashMap (map/dictionary) will be more efficient and convenient. DashMap ensures the uniqueness of the keys, thus duplicate keys will be overwritten. It also provides efficient lookup and modification of subscribers based on their unique identifiers.
3.  In terms of thread safety, Rust's compiler constraints help enforce safe concurrency. However, in the case of the `SUBSCRIBERS` static variable, using DashMap is still necessary for thread safety. The DashMap library provides a concurrent hashmap implementation that allows multiple threads to access and modify the map simultaneously without requiring external synchronization. This is crucial when dealing with shared data structures accessed by multiple threads. Implementing the Singleton pattern alone would not provide the necessary thread safety guarantees. Therefore, using DashMap in this case is a suitable choice to ensure thread safety when accessing and modifying the list of subscribers.

#### Reflection Publisher-2
1. Seperating "Service" and "Repository" applies seperation of concerns principle. It ensures each component has single responsibility, that is the Model should focus on representing the data, while the Service handles business logic and the Repository handles data storage and retrieval. The separation promotes code modulatiry and reusability, as the service and repository can be used across different models. Lastly, it also makes the codebase easier to maintain and test, for example the Service (business logic) can be tested without doing actual data storage.
2. If we only use Model, the codebase for each model (Program, Susbcriber, and Notification) will be very long as it must handle all the business logic (including the interactions between models) and data storage. We'll need to do a lot of ctrl+F to find the specific code parts we want to modify. Debugging will be harder as well. 
3. Postman is helpful to test the API endpoints. I like the simple & user-friendly interface, it serves the very purpose of testing the API endpoints with just button clicks. We also could specify the params, auth, and headers easily. 

#### Reflection Publisher-3
