# CipherVault – Requirements

## 1. Nonfunctional Requirements

**NFR1 – Security**

* All vault data must be encrypted using **AES-256-GCM**.
* Master passwords must be hashed using **Argon2id** with unique salts.

**NFR2 – Performance**

* Light Version should start within **5 seconds** on Ubuntu 22.04 LTS.
* Full Version should handle **up to 100 concurrent users** without noticeable lag.

**NFR3 – Scalability**

* Full Version must support **PostgreSQL** for multi-user scaling.

**NFR4 – Portability**

* Light Version must run on any system with **Java 17+**.
* Full Version must run in a **Docker container** on Ubuntu Server.

**NFR5 – Maintainability**

* Core functionality must reside in a **shared Java library (CipherVaultCommon)** to reduce code duplication.

**NFR6 – Usability**

* Light Version: **JavaFX GUI** with intuitive navigation.
* Full Version: **Web UI** accessible from modern browsers.

**NFR7 – Reliability**

* Vault writes must be **atomic and durable**.
* Backup and restore functionality must be provided for both Light and Full versions.

---

## 2. Functional Requirements

**FR1 – User Authentication**

* Users must log in using a **master password**.
* Optional key file may be used to enhance security.

**FR2 – Vault Management**

* Users can **add, edit, delete, and view** vault entries.
* Each entry stores **username, password, URL, notes, and folder/category**.

**FR3 – Encryption & Key Management**

* Master password + password salt → Argon2id → session key.
* Session key + encryption key salt → AES-256 key → encrypt/decrypt vault.

**FR4 – Salt Management**

* Password salt and encryption key salt must be **Base64-encoded** and stored in `/config/metadata.json`.

**FR5 – Light Version GUI**

* Provide a **JavaFX desktop GUI** to manage vault entries.
* Support CLI commands for advanced users.

**FR6 – Full Version Web UI**

* Provide a **web interface** for vault management.
* Provide **REST API endpoints** for automation and integration.

**FR7 – Multi-User Support (Full Version)**

* Users can create accounts and log in.
* Support **Role-Based Access Control (RBAC)** for shared vaults.

**FR8 – Audit Logging (Full Version)**

* Track login attempts, vault modifications, and administrative actions.

**FR9 – Database Support**

* Light Version: **SQLite** (single-file database).
* Full Version: **PostgreSQL** (multi-user scalable database).

**FR10 – Backup & Restore**

* Users must be able to **export and import vault data securely**.

**FR11 – Build & Deployment**

* Use **Maven** for building and dependency management.
* Full Version must support **Docker deployment**.

**FR12 – Shared Library (CipherVaultCommon)**

* Core functionality such as encryption, database abstraction, and models must reside in a **shared Java library JAR**.

**FR13 – Project Skeleton**

* The GitHub repository should have a **multi-module Maven structure**:

  * CipherVaultCommon (shared library)
  * CipherVaultLight (Light Version)
  * CipherVaultFull (Full Version)
* Each module contains **stub classes** ready for implementation.

---



I can also **update `wish_list.md`** similarly to reference the modules and stub structure if you want.

Do you want me to do that?

