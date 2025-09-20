# CipherVault – Developer Blueprint

## 1. Project Overview

**CipherVault** is a secure, self-hosted password manager with two versions:

* **Light Version** – standalone JavaFX desktop app with SQLite
* **Full Version** – web-enabled Java app with PostgreSQL, deployable in Docker

Core functionality is shared via a **Java library JAR**.

---

## 2. Architecture

### 2.1 Folder / GitHub Structure

```
/CipherVault/
├── /CipherVaultCommon/     # Shared library (Java core)
├── /CipherVaultLight/      # Standalone JavaFX app
├── /CipherVaultFull/       # Web/Docker version
├── README.md
├── LICENSE
└── .gitignore
```

### 2.2 Module Overview

| Module            | Responsibility                                              |
| ----------------- | ----------------------------------------------------------- |
| CipherVaultCommon | Encryption, database abstraction, models, utility functions |
| CipherVaultLight  | JavaFX GUI, CLI, SQLite adapter, vault interaction          |
| CipherVaultFull   | Web UI, REST API, PostgreSQL adapter, multi-user management |

---

## 3. Class / Module Layout (Shared Library)

**CipherVaultCommon**

* `EncryptionManager` – AES-256-GCM encrypt/decrypt, key derivation using Argon2id
* `VaultEntry` – Represents a single vault record (ID, Title, Username, Password, URL, Notes, Folder)
* `User` – Represents a user (ID, Username, PasswordHash, Role, Salt)
* `MetadataManager` – Reads/writes `/config/metadata.json` containing Base64-encoded salts
* `DatabaseAdapter` (abstract) – Interface for `SQLiteAdapter` and `PostgresAdapter`
* `Utils` – Logging, configuration, validation

**CipherVaultLight**

* `MainApp` – JavaFX entry point
* `VaultController` – Handles GUI interactions for vault entries
* `SettingsController` – Handles configuration, master password setup

**CipherVaultFull**

* `WebServer` – Embedded server setup (e.g., Jetty, Spring Boot)
* `VaultController` – REST endpoints for CRUD operations
* `UserController` – User registration, login, role management
* `AuditLogger` – Tracks user actions

---

## 4. Data Models

### 4.1 Vault Entry

| Field    | Type   | Description        |
| -------- | ------ | ------------------ |
| id       | UUID   | Unique identifier  |
| title    | String | Entry title        |
| username | String | Username           |
| password | String | Encrypted password |
| url      | String | Optional URL       |
| notes    | String | Optional notes     |
| folder   | String | Folder/category    |

### 4.2 User (Full Version)

| Field        | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| id           | UUID   | Unique identifier            |
| username     | String | Login name                   |
| passwordHash | String | Argon2id hashed password     |
| salt         | String | Base64-encoded password salt |
| role         | String | RBAC role                    |

### 4.3 Metadata

```json
{
  "version": "1.0",
  "kdf": "argon2id",
  "password_salt": "Base64EncodedValue",
  "encryption_salt": "Base64EncodedValue"
}
```

---

## 5. Encryption & Security Flow

1. User enters **master password**.
2. `EncryptionManager` uses **password salt + master password → Argon2id → session key**.
3. Session key + **encryption salt → Argon2id → AES-256 key**.
4. AES-256 key encrypts/decrypts vault entries.
5. Metadata file stores salts and crypto parameters in Base64.

---

## 6. REST API (Full Version)

| Endpoint                  | Method | Input                          | Output                |
| ------------------------- | ------ | ------------------------------ | --------------------- |
| `/api/login`              | POST   | username, password             | session token / error |
| `/api/vault/entries`      | GET    | session token                  | List of vault entries |
| `/api/vault/entries`      | POST   | session token, VaultEntry JSON | Created entry / error |
| `/api/vault/entries/{id}` | PUT    | session token, VaultEntry JSON | Updated entry / error |
| `/api/vault/entries/{id}` | DELETE | session token                  | Success / error       |
| `/api/users`              | POST   | username, password, role       | Created user / error  |

---

## 7. GUI Mockups (Light Version)

* **Login Screen**: Master password field, optional key file
* **Vault Dashboard**: List of entries, folders/categories, search bar
* **Add/Edit Entry**: Fields for Title, Username, Password, URL, Notes, Folder
* **Settings**: Change master password, backup, import/export vault

---

## 8. Build & Deployment

* **Build Tool**: Maven
* **Light Version**:

  ```bash
  cd CipherVaultLight
  mvn clean package
  java -jar target/CipherVaultLight.jar
  ```
* **Full Version** (Docker):

  ```bash
  cd CipherVaultFull
  mvn clean package
  docker build -t ciphervault-full .
  docker run -d -p 8080:8080 ciphervault-full
  ```

---

## 9. Wishlist / Aspirational Features

* WISH1: Master password with optional key file
* WISH2: Easy-to-use UI / intuitive navigation
* WISH3: Secure folders / categories
* WISH4: Quick add/edit/delete vault entries
* WISH5: Search & filter entries
* WISH6: Export / import vault data
* WISH7: Cross-platform support (Windows, Mac)
* WISH8: Two-Factor Authentication (Full Version)
* WISH9: Built-in password generator
* WISH10: Notifications for failed logins or vault changes

---

This **blueprint document** contains everything an AI or developer needs to:

* Generate project skeletons
* Implement shared library
* Build Light and Full versions
* Implement encryption, database access, and REST API
* Create GUIs and workflows

---



