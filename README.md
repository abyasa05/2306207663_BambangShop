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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. Penggunaan trait dalam kasus ini bisa berguna jika misalnya akan dibuat beberapa jenis Subscriber dengan behaviour yang berbeda-beda terhadap notifikasi. Namun, karena tampaknya semua struct Subscriber hanya memuat nama dan URL saja, maka penggunaan trait belum begitu diperlukan untuk kasus ini dan penggunaan struct yang ada sudah mencukupi.

2. Karena penggunaan URL harus bersifat unik, maka penggunaan DashMap merupakan pilihan yang lebih tepat mengingat sifatnya yang tidak memperbolehkan adanya nilai duplikat. Selain itu, waktu pengecekan pada DashMap juga relatif lebih efisien dibandingkan dengan Vec. Hal ini dikarenakan Vec mengecek nilai secara linear sehingga memiliki kompleksitas O(n) sementara DashMap memiliki kompleksitas O(1). Karena itu, DashMap lebih cocok untuk digunakan untuk menyimpan data (Subscriber) dalam jumlah besar.

3. DashMap merupakan jenis HashMap yang dibuat untuk menangani operasi _multi-thread_. Dengan itu, tiap thread bisa mengakses DashMap secara _concurrent_ tanpa perlu melakukan locking pada keseluruhan Map. Sementara itu, Singleton pattern memastikan bahwa hanya bisa terdapat satu instansiasi struct pada satu waktu dengan bantuan Mutex. Penggunaan Singleton pattern akan melakukan locking pada DashMap sehingga hanya terdapat satu thread yang bisa mengakses dalam satu waktu. Hal ini bisa berdampak pada penurunan performa, terutama jika terdapat banyak operasi concurrency dalam satu waktu. Maka dari itu, penggunaan DashMap sebagai _List of Subscribers_ sudah cukup tepat.

#### Reflection Publisher-2
1. Pemisahan _repository_ dan _service_ dari _business logic_ merupakan implementasi dari salah satu design principle yaitu Single Responsibility Principle (SRP), di mana tiap bagian memiliki fungsionalitas yang jelas. Pemisahan ini juga berguna untuk mengurangi dependensi _business logic_ dengan _database_, atau dalam kata lain, perubahan pada database tidak akan mempengaruhi business logic. Selain itu, pemisahan ini juga mempermudah pembuatan dan penjalanan unit testing. Dengan ini, unit test tidak memerlukan koneksi langsung dengan database, melainkan bisa menggunakan _mock repository_ saja.

2. Jika service dan repository digabungkan ke dalam model, maka program akan cenderung menjadi lebih kompleks dan sulit untuk di-maintain. Misalnya, logika bisnis sekarang harus diduplikasi pada tiap Model sehingga jika ada perubahan atau penambahan fitur pada logika bisnis, tiap model harus dimodifikasi juga. Selain itu, unit testing juga menjadi kurang praktis karena melibatkan operasi langsung dengan database.

3. Postman berguna untuk mengecek respons dari _http request_ yang dikirimkan. Dengan ini, Postman bisa membantu saya untuk memastikan bahwa program bisa mengeluarkan _output_ yang tepat untuk tiap fitur dan _request_-nya.

#### Reflection Publisher-3
