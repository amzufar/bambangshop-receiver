# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. In this tutorial, we used `RwLock<>` to synchronise the use of `Vec` of `Notification`s. Explain why it is necessary for this case, and explain why we do not use `Mutex<>` instead?

The code used `RwLock<Vec>` to coordinate access to the shared `Vec` of `Notifications` within the `NotificationRepository`. This guarantees that:
 * Multiple threads can concurrently read from the data structure or permit a single thread to modify it at any given time. 
 * Prevents race condition and ensuring thread safety. 
 * `RwLock` is favored over `Mutex` here because it facilitates concurrent reading of the notifications list by multiple threads, thus enhancing concurrency and performance, particularly in scenarios with frequent read operations. 
 * `Mutex` restricts access to a single thread and may cause unnecessary blocking and reduced concurrency
 * `RwLock` enables effective coordination of both read and write access to maximize concurrency while maintaining thread safety. 
 
 Hence why `RwLock` is the optimal choice when efficient coordination of read and write operations is necessary to achieve maximum concurrency and ensure thread safety.

2. In this tutorial, we used `lazy_static` external library to define `Vec` and `DashMap` as a `static` variable via a `static` function, why did not Rust allow us to do so?

In Rust, static variables are designed to be immutable by default for safety reasons. This design choice aligns with Rust's emphasis on memory safety and preventing data races. This ensures that they cannot be modified concurrently by multiple threads, which helps ensures thread safety.

Allowing mutable static variables, especially to be modified by static functions, would introduce potential issues related to thread safety and data races. In concurrent environments, mutable static variables could lead to race conditions, where multiple threads attempt to modify the variable's state simultaneously, resulting in unpredictable behavior.

By disallowing mutable static variables by default, Rust encourages developers to use safer concurrency patterns, such as using synchronization primitives like Mutex or RwLock to control access to shared state. While this may require more explicit handling of mutability, it ultimately leads to more robust and predictable concurrent programs.
#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: `src/lib.rs`? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

`lib.rs` file sets up configurations and utilities that are commonly used in Rocket applications. These configurations include handling environment variables (`dotenv`), defining custom error types and responses (`Result`, `Error`, `ErrorResponse`, `compose_error_response`), and initializing global variables such as an HTTP client (`REQWEST_CLIENT`) and application configurations (`APP_CONFIG`). By leveraging the Rocket framework, us as developers can utilize the functionalities provided by Rocket along with the configurations and utilities defined in the Rust file to build robust and scalable web applications in Rust. Therefore, this file serves as a crucial foundational component for building a Rust web application with Rocket.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

The Observer pattern makes it easy to add more subscribers to the notification system. Each subscriber listens for notifications from the main part of the program, so it's simple to customize and add new features. If we use this pattern, adding new subscribers just means making sure they follow the subscriber rules and telling the main program about them. Also, if we want to run multiple copies of the main program, that's no problem because each one can keep track of its own subscribers. This way, the notification system can handle more work efficiently and be ready for any new features we want to add later.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be you tutorial work or your Group Project)

Using tools like Postman to create tests helps developers check API endpoints and responses automatically. This makes sure the application works correctly and stays stable by running tests to catch any issues, and it checks if the responses match what's expected. Also, when we add extra details like descriptions and examples in Postman, it makes it easier for everyone on the team to understand how to use the APIs. This means new developers can learn how everything works faster, and everyone follows the same rules for using the APIs. Overall, using Postman like this can really help make development smoother, improve the quality of the software, and make it easier for everyone on the team to work together.
