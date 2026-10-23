graph TD
    Client["Клієнт"]

    subgraph Monolith ["Модульний моноліт"]
        Checkout["Checkout -<br/>координатор"]
        Payment["Payment - модуль і<br/>адаптер провайдера"]
        Orders["Orders<br/>Interface"]
        Inventory["Inventory"]
        Notification["Notification"]
        Reporting["Reporting"]
        NoteNotif["Помилка сповіщення<br/>не скасує замовлення"]
        DB[("Спільна база даних у пам'яті")]

        Checkout --> Orders
        Checkout --> Inventory
        Checkout --> Payment
        Checkout --> Notification
        Checkout --> Reporting

        Payment -- "Дані замовлення через контракт" --> Orders
        Payment --> DB

        Orders --> DB
        Inventory --> DB
        Notification --> DB
        Reporting --> DB

        Notification --> NoteNotif
    end

    FakePay["FakePay<br/>(зовнішній сервіс)"]
    FakeMail["FakeMail<br/>(зовнішній сервіс)"]

    Client --> Checkout
    Payment --> FakePay
    Notification -- "Надсилання сповіщень" --> FakeMail
    