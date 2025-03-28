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

<!-- You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman -->

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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Do I still need a trait for Subscriber?
    for this project, a single model struct is enough because im not dealing ith multiple behaviours from the subscriber struct. 

2. Is Vec enough or should I use DashMap?
    Vec is enough because the subscriber struct is not dealing with multiple behaviours but it requires additional logic for the uniqueness problem. but dashmap is necessary and may be better because i need unique subscribers indentifier because of the 'url'.

3. Do I need DashMap or can I use Singleton?
    dashmap, because i need it for concurrent access and thread safety. implementing this project with a singleton would need more work put into because of the manual sync

#### Reflection Publisher-2
1. Why separate “Service” and “Repository” from a Model?
    In MVC, separating the "Service" and "Repository" layers from the Model is about adhering to the Single Responsibility Principle. The Model should focus on representing data, while the Repository handles data access, and the Service encapsulates business logic. This separation improves modularity, reusability, and testability. It keeps the Model focused on what it represents, allowing easier changes to business logic or data storage without affecting the core model.

2. What happens if i only use the Model?
    If i only use the Model, the code can become monolithic and harder to maintain. The Model would need to handle both data persistence and business logic, leading to tight coupling. This would increase complexity and make it difficult to scale or modify parts of the system (like switching databases or changing business rules) without affecting everything else.

3. Interactions between Models (Program, Subscriber, Notification)
    If interactions between the Program, Subscriber, and Notification models are handled solely in one place, the complexity increases. Each model would need to know about the others, leading to poor separation of concerns. For example, changes in the logic of one model (like modifying how a notification is sent) could impact the entire codebase. By separating them into services and repositories, each model can evolve independently, reducing code complexity and improving maintainability.

4. How Postman helps in testing
    I’ve explored Postman, and it helps a lot in testing API requests. With Postman, I can easily send HTTP requests (GET, POST, PUT, DELETE) and check responses. It allows me to test my RESTful APIs by setting up different endpoints, request bodies, and headers.
#### Reflection Publisher-3
1. Which variation of Observer Pattern is used in this tutorial case?
    In this tutorial case, i am using the Push model of the Observer Pattern. The publisher (NotificationService) pushes data (the notification) to the subscribers via HTTP requests.

2.  Advantages and disadvantages of using the other variation (Pull model)?
    If i used the Pull model, the subscribers would need to actively request notifications from the publisher.

    Advantages of Pull model:
    - Less load on the publisher: The publisher only sends notifications when subscribers request them.

    - More control for subscribers: Subscribers can decide when they want to receive updates.

    Disadvantages of Pull model:
    - Increased complexity: Subscribers need to constantly poll for updates, which could be inefficient and lead to unnecessary resource usage.

    - Delayed notifications: Since subscribers have to pull the data, there may be a delay in receiving updates.

3. What will happen if i decide to not use multi-threading in the notification process?
    the notification process would become sequential. This means that the program would send notifications one by one to each subscriber, which could significantly slow down the system if there are many subscribers. The application might become unresponsive or take longer to complete the process, especially under heavy load, as each notification is processed in turn rather than concurrently.