# CipherVault – Developer-Ready Blueprint

## 1. Project Overview

**CipherVault** is a secure, self-hosted password manager offering two variants:

* **Light Version** – standalone JavaFX desktop app using SQLite
* **Full Version** – web-enabled Java app using PostgreSQL, deployable in Docker

Core functionality is shared via **CipherVaultCommon**, a Java library JAR.

---

## 2. Architecture

### 2.1 Folder / GitHub Structure

```
/CipherVault/                  # Parent repository
├── pom.xml                    # Parent Maven POM (multi-module)
├── /CipherVaultCommon/        # Shared library module
│   └── pom.xml
├── /CipherVaultLight/         # Light Version module
│   └── pom.xml
├── /CipherVaultFull/          # Full Version module
│   └── pom.xml
├── README.md
└── LICENSE
```

* **Each module has its own Maven POM** and stub classes ready for implementation.
* **Parent POM** builds all modules together.

---

## 3. Module Responsibilities

| Module            | Responsibilities                                                                               | Related FRs                         |
| ----------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------- |
| CipherVaultCommon | Core encryption, Argon2id, AES-256, vault/data models, metadata handling, database abstraction | FR3, FR4, FR12                      |
| CipherVaultLight  | JavaFX GUI, CLI support, SQLite adapter, vault operations                                      | FR2, FR5, FR10                      |
| CipherVaultFull   | Web UI, REST API, PostgreSQL adapter, multi-user management, audit logging                     | FR2, FR6, FR7, FR8, FR9, FR10, FR11 |

---

## 4. Class / Module Layout (Shared Library)

**CipherVaultCommon**

* `EncryptionManager` – AES-256-GCM encrypt/decrypt, Argon2id key derivation
* `VaultEntry` – Single vault record (ID, title, username, password, URL, notes, folder)
* `User` – User object (ID, username, password hash, role, salt)
* `MetadataManager` – Reads/writes `/config/metadata.json` containing Base64-encoded salts
* `DatabaseAdapter` (abstract) – Interface for `SQLiteAdapter` and `PostgresAdapter`
* `Utils` – Logging, configuration, validation

**CipherVaultLight**

* `MainApp` – JavaFX entry point
* `VaultController` – GUI logic for vault entries
* `SettingsController` – Master password, backup, import/export

**CipherVaultFull**

* `WebServer` – Embedded server (e.g., Jetty, Spring Boot)
* `VaultController` – REST endpoints for CRUD operations
* `UserController` – Registration, login, role management
* `AuditLogger` – Tracks user actions

---

## 5. Data Models

### Vault Entry

| Field    | Type   | Description        |
| -------- | ------ | ------------------ |
| id       | UUID   | Unique identifier  |
| title    | String | Entry title        |
| username | String | Username           |
| password | String | Encrypted password |
| url      | String | Optional URL       |
| notes    | String | Optional notes     |
| folder   | String | Folder/category    |

### User (Full Version)

| Field        | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| id           | UUID   | Unique identifier            |
| username     | String | Login name                   |
| passwordHash | String | Argon2id hashed password     |
| salt         | String | Base64-encoded password salt |
| role         | String | RBAC role                    |

### Metadata

```json
{
  "version": "1.0",
  "kdf": "argon2id",
  "password_salt": "Base64EncodedValue",
  "encryption_salt": "Base64EncodedValue"
}
```

---

## 6. Encryption & Security Flow

1. User enters **master password**.
2. `EncryptionManager` derives **session key** using Argon2id and password salt.
3. Session key + encryption key salt → AES-256 key for vault encryption/decryption.
4. Metadata file `/config/metadata.json` stores Base64-encoded salts and crypto parameters.

---

## 7. REST API (Full Version)

| Endpoint                  | Method | Input                          | Output                |
| ------------------------- | ------ | ------------------------------ | --------------------- |
| `/api/login`              | POST   | username, password             | session token / error |
| `/api/vault/entries`      | GET    | session token                  | List of vault entries |
| `/api/vault/entries`      | POST   | session token, VaultEntry JSON | Created entry / error |
| `/api/vault/entries/{id}` | PUT    | session token, VaultEntry JSON | Updated entry / error |
| `/api/vault/entries/{id}` | DELETE | session token                  | Success / error       |
| `/api/users`              | POST   | username, password, role       | Created user / error  |

---

## 8. GUI Mockups (Light Version)

* **Login Screen** – Master password, optional key file
* **Vault Dashboard** – List entries, folders/categories, search bar
* **Add/Edit Entry** – Fields: title, username, password, URL, notes, folder
* **Settings** – Change master password, backup, import/export

---

## 9. Build & Deployment

* **Build Tool**: Maven
* **Light Version**:

  ```bash
  cd CipherVaultLight
  mvn clean package
  java -jar target/CipherVaultLight.jar
  ```
* **Full Version (Docker)**:

  ```bash
  cd CipherVaultFull
  mvn clean package
  docker build -t ciphervault-full .
  docker run -d -p 8080:8080 ciphervault-full
  ```

---

## 10. Features & Wishlist

**Core Features**

* AES-256-GCM encrypted vaults
* Argon2id password hashing
* Master password with optional key file
* Multi-user and RBAC support (Full Version)
* Audit logging, backup/restore, CLI and JavaFX GUI

**Wishlist / Aspirational Features**

* WISH1: Master password + optional key file
* WISH2: Easy-to-use GUI / navigation
* WISH3: Secure folders / categories
* WISH4: Quick add/edit/delete vault entries
* WISH5: Search & filter
* WISH6: Export/import vault securely
* WISH7: Cross-platform support (Windows, Mac)
* WISH8: Two-Factor Authentication (Full Version)
* WISH9: Built-in password generator
* WISH10: Notifications for failed logins or vault changes

---

✅ **Summary**

* CipherVault is a **secure, self-hosted password manager** with a **multi-module Maven structure**, **shared library**, and **Light + Full versions**.
* The blueprint includes **data models, encryption flow, REST API, GUI stubs, and Maven skeleton**, ready for AI or developer implementation.

---



