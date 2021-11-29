# Personnel interview test

This is an implementation of the following interview test assignment:

```text
Personnel LTD. PHP Test 2020

Create a PHP web application which will contain the features explained below. You can
either use native code or a PHP framework you are familiar with. Our company prefers using the
Laravel PHP framework.

1.) Using the provided CSV file (file.csv), create a screen which will upload the CSV file data
and store it in a database. For this, you will need to:

• Create a database design based on the CSV file;
• Create a screen for uploading CSV files;
• Parse the uploaded CSV file and store the data in the database.

* No authentication (login screen) is required for this task
** You can use any frontend or backend library/plugin as you wish

2.) Using the created database and the imported data from the CSV file, create the following:

2.1.) A table which will display all valid calls*.

2.2) CRUD (Create/Read/Update/Delete) operations for managing calls. The fields
“User”, “Client” and “Type of Call” should be in a dropdown select.

2.3.) A table which will display the users. Every record needs to have a “View” button which
will lead to view 2.4.)

2.4.) A view which will display information (summary) regarding a specific user.

The following should be displayed:
• User’s name and surname
• Average user score (for only valid calls*)
• A table showing the last 5 call interactions for the user (for only valid calls*)

* Valid calls are considered valid if the duration is greater than 10
** No authentication (login screen) is required for this task
*** You can use any frontend library or backend library as you wish

Attached files:
file.csv
```

## Assumptions

To avoid any ambiguities that arise from the test description, following assumptions were made:

- Every row in csv file denotes a `Call` entity.
- Field `User` in csv file containing a user's name uniquely identifies a user entity in the system. Any duplicates in the file refer to the same user entity.
- Field `Client` in csv file containing a client's name uniquely identifies a client entity. Any duplicates in the file refer to the same client entity.
- A single `Client` entity can only ever be of one type denoted by the `Client Type` field in csv file.
- Field `Client Type` is required and always one of `Carer` or `Nurse`.
- Field `Duration` in csv file is required and always >= 0.
- Field `External Call Score` in csv file is required and always >= 0.
- Field `Date` in csv file is required.

## Database schema

After clearing any ambiguities about the data we can define the db schema:

```sql
CREATE TABLE `user` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id_UNIQUE` (`id`),
  UNIQUE KEY `name_UNIQUE` (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE `client` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  `type` varchar(45) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id_UNIQUE` (`id`),
  UNIQUE KEY `name_UNIQUE` (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE `call` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `user_id` int unsigned NOT NULL,
  `client_id` int unsigned NOT NULL,
  `date` datetime NOT NULL,
  `duration` int unsigned    NOT NULL,
  `type` varchar(45) NOT NULL,
  `score` int unsigned NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `id_UNIQUE` (`id`),
  KEY `fk_call_user_id_idx` (`user_id`),
  KEY `fk_call_client_id_idx` (`client_id`),
  CONSTRAINT `fk_call_client_id` FOREIGN KEY (`client_id`) REFERENCES `client` (`id`),
  CONSTRAINT `fk_call_user_id` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
```

## Implementation details

Implementation of this test assignment will include:

- Laravel framework to match company preference.
- Server side rendering with Blade templates to keep it simple.

Sketches defining the layout will all pages required to fulfill the requirements [HERE](./UI-mockups.svg).
