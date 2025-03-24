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
-   [✓] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✓] Commit: `Create Subscriber model struct.`
    -   [✓] Commit: `Create Notification model struct.`
    -   [✓] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✓] Commit: `Implement add function in Subscriber repository.`
    -   [✓] Commit: `Implement list_all function in Subscriber repository.`
    -   [✓] Commit: `Implement delete function in Subscriber repository.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✓] Commit: `Create Notification service struct skeleton.`
    -   [✓] Commit: `Implement subscribe function in Notification service.`
    -   [✓] Commit: `Implement subscribe function in Notification controller.`
    -   [✓] Commit: `Implement unsubscribe function in Notification service.`
    -   [✓] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. **In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or `trait` in Rust) in this BambangShop case, or a single Model `struct` is enough?**<br><br>
    Karena kondisi BambangShop saat ini masih sederhana dengan _class_ Subscriber berperan sebagai satu-satunya _observer_, maka satu model `struct` masih cukup. Penggunaan `trait` akan diperlukan jika nantinya Suscriber sebagai _observer_ memiliki jenis atau perilaku yang berbeda-beda. Hal ini akan mendukung penerapan _Open-Closed Principle_.<br><br>
2. **`id` in `Program` and `url` in `Subscriber` is intended to be unique. Explain based on your understanding, is using `Vec` (list) sufficient or using `DashMap` (map/dictionary) like we currently use is necessary for this case?**<br><br>
    Menggunakan `DashMap` lebih menguntungkan daripada menggunakan `Vec`. Keuntungannya terdapat pada penyimpanan data `id` dan `url` yang bisa dihubungkan langsung dengan `Program` dan `Subscriber` yang bersangkutan dengan sistem _key-value_. `DashMap` juga dapat berjalan dengan efisien ketika menghadapi data atau _request_ yang besar. <br><br>
3. **When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (`SUBSCRIBERS`) static variable, we used the `DashMap` external library for `thread safe HashMap`. Explain based on your understanding of design patterns, do we still need `DashMap` or we can implement Singleton pattern instead?**<br><br>
    `DashMap` tetap diperlukan karena selain mendukung _multithreading_ pada pemrosesan Map `Subscriber`, `DashMap` menyediakan fasilitas _thread-safe_ secara _built-in_ untuk operasi pada Map. Jadi, mengimplementasikan Singleton _pattern_ saja belum cukup jika tidak dilengkapi dengan prosedur _thread-safe_.

#### Reflection Publisher-2
1. **In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?**<br><br>
    Dengan memisahkan Service dan Repository dari Model, kita telah menerapkan _Single Responsibility Principle_. Repository bertanggung jawab pada akses dan penyimpanan data ke _database_, sementara Service bertugas untuk menangani logika pengolahan datanya. Pemisahan ini memungkinkan kita menguji masing-masing komponen secara terpisah dan mengembangkan atau memodifikasi fungsi masing-masing tanpa mengganggu Model yang sudah dibuat.<br><br>
2. **What happens if we only use the Model? Explain your imagination on how the interactions between each model (`Program`, `Subscriber`, `Notification`) affect the code complexity for each model?**<br><br>
    Jika kita hanya menggunakan Model tanpa memisahkan Service dan Repository, Model tersebut harus menangani banyak hal, mulai dari penyimpanan data hingga logika fungsinya. Jika dilakukan perubahan pada satu fungsi, fungsi lain juga harus ikut berubah. Akibatnya dapat meningkatkan resiko terjadinya _bug_, kesulitan _maintainability_, dan kode menjadi lebih kompleks dan sulit dibaca. <br><br>
3. **Have you explored more about `Postman`? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.**<br><br>
    `Postman` sangat berguna untuk menguji _endpoint_ dan API dari program yang kita buat. Kita bisa mengecek _behavior_ program dengan mengirim berbagai jenis _request_ (GET, POST, PUT, DELETE, dll.) ke server melalui Postman. Tentu _response_-nya juga ditampilkan Postman untuk verifikasi apakah _endpoint_ berjalan sesuai yang diharapkan.

#### Reflection Publisher-3
