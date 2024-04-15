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
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Penggunaan *trait* atau *interface* pada *Observer Pattern* bergantung pada banyaknya tipe *Observer*. Jika *Observer* memiliki banyak tipe, penggunaan *trait* lebih diutamakan karena membuat desain kode yang lebih fleksibel dan mendukung *Open/Closed Principle* jika terdapat penambahan tipe *Observer* di kemudian hari. Namun, pada kasus BambangShop dimana *Observer* hanya terdiri dari satu jenis dan memiliki fungsionalitas yang sama, yaitu hanya Subscriber saja, penggunaan trait tidak diperlukan dan sudah cukup dengan menggunakan satu model *struct* saja.

2. Penggunaan DashMap memiliki keuntungan dari segi efisiensi dibandingkan dengan Vec ketika berhubungan dengan data yang cukup besar, yaitu dengan mempercepat proses operasi yang berhubungan dengan pencarian data karena `id` atau `url` yang unik yang dapat menjadi *key* pada DashMap sehingga memiliki akses yang cepat untuk mendapatkan suatu *value* tertentu. Berbeda dengan Vec yang perlu mengiterasi setiap isinya untuk mencari suatu value tertentu sehingga penggunaan DashMap lebih efisien.

3. Dalam konteks *multithreading*, penggabungan antara penggunaan DashMap dan *Singleton Pattern* perlu untuk dilakukan untuk memastikan keamanan operasi *thread* secara *concurrent*. Penggunaan *Singleton Pattern* memberikan kepastian bahwa hanya terdapat satu objek SUBSCRIBER saja di program sehingga sumber data terpusat di satu variabel saja, tetapi belum memastikan keamanan pengaksesan data secara *concurrent* pada operasi di *multithreading*. Oleh karena itu, penggunaan DashMap memberikan solusi untuk permasalahan tersebut karena DashMap memiliki *built-in* untuk keamanan *thread* sehingga dapat memastikan keamanan pengaksesan data secara *concurrent* di lingkungan *multithreaded*.

#### Reflection Publisher-2
1. Pemisahan antara **Repository** dan **Service** dari **Model** perlu dilakukan agar memenuhi prinsip *Single Responsibility Principle (SRP)* dan *Separation of Concerns*. Dengan menerapkan SRP, setiap komponen dari program kita memiliki tugasnya tersendiri secara spesifik sehingga membuat program kita menjadi lebih modular dan mudah untuk di-*maintain* di kemudian hari. Selain itu, dengan menerapkan prinsip *Separation of Concerns* akan menyebabkan setiap komponen tidak memiliki ketergantungan yang kuat sehingga perubahan di salah satu komponen tidak akan berpengaruh terhadap keseluruhan kode program. Oleh karena itu, dengan memisahkan **Repository** dan **Service** dari **Model** akan membuat program kita lebih fleksibel, mengurangi kompleksitas, dan memudahkan pemeliharaan kode.

2. Dengan menggabungkan **Repository** dan **Service** di **Model**, hal tersebut akan menyebabkan berkurangnya fleksibilitas kode karena setiap komponen pada program kita akan memiliki ketergantungan yang kuat antar satu sama lain. Dengan begitu, ketika kita ingin melakukan perubahan pada suatu komponen, hal tersebut dapat berakibat pada fungsionalitas komponen lain sehingga susah untuk beradaptasi dan melakukan pembaruan kode di kemudian hari.

3. Menggunakan **Postman** membantu saya dalam menguji *endpoint* aplikasi dengan lebih mudah, seperti *request* GET, POST, PUT, DELETE, dan lain-lain. Dengan begitu, saya bisa menjadi lebih yakin untuk memastikan apakah program sudah memberikan fungsionalitas yang benar atau belum. Fitur-fitur lain pada **Postman** yang mungkin akan saya gunakan nantinya adalah:
- **Automation**, **Postman** dapat memberikan fasilitas untuk menjalankan serangkaian *test* secara otomatis dan memonitoring *endpoint* API.
- *Environment Variables*, **Postman** dapat membuat suatu *variable* yang dapat digunakan diberbagai *request* berbeda dan memudahkan ketika berada di *environments* yang berbeda, seperti *development, staging*, dan *production*.

#### Reflection Publisher-3
1. Pada tutorial ini, kita menggunakan variasi **Push model** pada *Observer Pattern*. Hal ini ditandai dengan digunakannya method `notify` setiap kali dilakukan `create`, `delete`, dan `publish` Product. Dengan begitu, setiap kali ada perubahan data, maka Publisher akan *push* pembaruan tersebut langsung ke *Observer* atau dalam kasus ini adalah Subscriber.

2. Jika menggunakan varian lainnya, yaitu **Pull model**, kita akan mendapatkan kelebihan dan kekurangannya, yaitu:
- Kelebihan<br>
Subscriber dapat memiliki kontrol penuh terhadap pembaruan data dari Publisher. Subscriber dapat memperoleh data terbaru kapan pun mereka mau. Selain itu, **Pull model** juga bisa membantu menghindari adanya transfer data dan notifikasi yang tidak diperlukan oleh Subscriber.
- Kekurangan<br>
Karena perbaruan data dari Publisher hanya dapat diketahui ketika Subscriber yang melakukan *pull* data dari Publisher, hal tersebut mengakibatkan Subscriber perlu melakukan *pull* secara berkala untuk mendapatkan *update* terbaru. Selain itu, Subscriber juga bisa tertinggal dengan *update* terbaru karena mereka tidak tahu kapan *update* tersebut sudah ada. Tidak hanya itu, penggunaan **Pull model** juga dapat meningkatkan keterkaitan antara Publisher dengan Observer sehingga akan cukup sulit untuk di-*maintain* dan meningkatkan kompleksitas *codebase* di sisi Observer karena perlu mengatasi masalah sinkronisasi dan strategi pengambilan data terbaru.

3. Jika kita tidak menggunakan konsep *multi-threading* dalam hal notifikasi, maka hal tersebut akan menyebabkan terjadinya *bottleneck* dalam sistem yang akan mengakibatkan proses pengiriman notifikasi ke setiap Subscriber menjadi sangat lambat. Hal tersebut terjadi karena pengiriman notifikasi perlu dilakukan satu per satu atau hanya bisa melalui satu *thread* sehingga akan menghambat proses pengiriman notifikasi.