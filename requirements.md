# CipherVault – Requirements

## 1. Nonfunctional Requirements

**NFR1 – Security**

* All vault data must be encrypted using **AES-256-GCM**.
* Master passwords must be hashed using **Argon2id** with unique salts.

**NFR2 – Performance**

* The Light Version should start within **5 seconds** on a standard Ubuntu 22.04 system.
* The Full Version should handle up to **100 concurrent users** without noticeable performance degradation.

**NFR3 – Scalability**

* Full Version must support PostgreSQL to scale vaults for multiple users.

**NFR4 – Portability**

* Light Version must run on any system with **Java 17+** installed.
* Full Version must run in a **Docker container** on Ubuntu Server.

**NFR5 – Maintainability**

* Core functionality must reside in a **shared Java library** to reduce code duplication and simplify updates.

**NFR6 – Usability**

* Light Version must provide a **JavaFX GUI** with intuitive navigation.
* Full Version must provide a **web UI** accessible from modern browsers.

**NFR7 – Reliability**

* Vault writes to the database must be atomic and durable.
* Backup and restore functionality must be provided for both Light and Full versions.

---

## 2. Functional Requirements

**FR1 – User Authentication**

* Users must be able to log in using a master password.
* Optionally, a key file may be used to enhance security.

**FR2 – Vault Management**

* Users must be able to **add, edit, delete, and view** vault entries.
* Each entry stores **username, password, URL, and notes**.

**FR3 – Encryption & Key Management**

* Master password + password salt → Argon2id → Session key.
* Session key + encryption key salt → AES-256 key → encrypt/decrypt vault.

**FR4 – Salt Management**

* Password salt and encryption key salt must be **Base64-encoded** and stored in `/config/metadata.json`.

**FR5 – Light Version GUI**

* Provide a **JavaFX desktop GUI** to interact with the vault.
* Support CLI commands for advanced users.

**FR6 – Full Version Web UI**

* Provide a **web interface** for vault management.
* Provide **REST API endpoints** for automation and integration.

**FR7 – Multi-User Support (Full Version)**

* Users must be able to create accounts and log in.
* Support **Role-Based Access Control (RBAC)** for shared vaults.

**FR8 – Audit Logging (Full Version)**

* Track login attempts, vault modifications, and administrative actions.

**FR9 – Database Support**

* Light Version: use **SQLite** as single-file database.
* Full Version: use **PostgreSQL** for multi-user scalability.

**FR10 – Backup & Restore**

* Users must be able to export and import vault data securely.

**FR11 – Build & Deployment**

* Use **Maven** for building the project and managing dependencies.
* Full Version must support **Docker deployment**.

**FR12 – Shared Library**

* Core functionality such as encryption, database abstraction, and models must reside in a **shared Java library JAR**.

---



