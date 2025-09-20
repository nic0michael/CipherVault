# CipherVault – Specification (spec.md)

## 1. Project Name

**CipherVault** – A secure, self-hosted password manager designed as an alternative to KeePass.

---

## 2. Programming Language and GUI

| Variant                     | Language | GUI / Interface                               |
| --------------------------- | -------- | --------------------------------------------- |
| Light Version (Standalone)  | Java     | **JavaFX** desktop GUI, CLI optional, no HTML |
| Full Version (Web / Docker) | Java     | Web UI via HTML/REST API, runs in browser     |

---

## 3. Build Automation Tool

* **Tool**: **Maven**
* **Purpose**: Automates compilation, testing, packaging, and dependency management for all variants.
* **Usage**:

  * Compile Java source code
  * Package variant-specific builds into JAR/WAR files
  * Manage dependencies, including the shared library JAR

---

## 4. Versions of the Solution

### 4.1 Light Version – Standalone

* **Database**: SQLite (single-file vault)
* **Access**: CLI or JavaFX desktop GUI; **does not run from HTML**
* **Target Users**: Homelab enthusiasts, personal use
* **Key Points**:

  * Easy installation
  * Fully offline and secure
  * Portable vault file for easy backups

### 4.2 Full Version – Web / Self-Hosted

* **Database**: PostgreSQL
* **Access**: Web UI via HTML; REST API for automation
* **Target Users**: Teams, enterprises, or advanced homelab users
* **Key Points**:

  * Supports multi-user environments with RBAC
  * Centralized audit logging
  * Runs securely in Docker container with HTTPS/TLS

---

## 5. Shared Java Library JAR

* A **Java library JAR** will be provided for code that is common to both Light and Full versions.
* **Included functionality**:

  * AES-256 encryption/decryption
  * Argon2id password hashing and salt handling
  * Vault data models and metadata handling
  * Database abstraction layer (adapters for SQLite and PostgreSQL)
  * Utility functions (logging, config, validation)
* **Benefits**:

  * Single source of truth for cryptography and core logic
  * Easier maintenance and bug fixes
  * Reduces code duplication between versions

---

## 6. Database

* **Primary**: PostgreSQL (Full Version)
* **Alternative**: SQLite (Light Version)

---

## 7. Operating System

* Target: **Ubuntu Server 22.04 LTS or later**

---

## 8. Encryption

* **Data Encryption**: AES-256-GCM
* **Password Hashing**: Argon2id
* **Key Management**:

  * Master password + password salt → Argon2id → Session key
  * Session key + encryption key salt → AES-256 key → Encrypts/decrypts vault
* **Salts**:

  * Both **Base64-encoded**, embedded in `/config/metadata.json`
  * **Password Salt**: hashes the master password
  * **Encryption Key Salt**: used for AES-256 key derivation for database data

### Example Metadata File

```json
{
  "version": "1.0",
  "kdf": "argon2id",
  "password_salt": "mZL9z8H2pJ8+T2D6Kg==",
  "encryption_salt": "O7JfZk3aL8x4JrA5Qw=="
}
```

---

## 9. GitHub Project & Folder Structure

* The project will be maintained as a **single GitHub repository** with three separate folders/modules:

```
/CipherVault/
├── /CipherVaultCommon/     # Shared library (Java core)
│   └── pom.xml
├── /CipherVaultLight/      # Standalone JavaFX app
│   └── pom.xml
├── /CipherVaultFull/       # Web/Docker version
│   └── pom.xml
├── README.md
└── .gitignore
```

* **Each folder is a separate Maven module**.
* Parent repository may include a **multi-module Maven POM** to build all three together.
* Benefits:

  * Clear separation of Light, Full, and Common code
  * Independent evolution of GUI and Web logic
  * Shared library ensures consistent encryption, key management, and models

---

## 10. Features

* Multi-user support (Full Version only)
* Secure vaults with sharing (Full Version)
* Optional key file for extra protection
* Audit logging (Full Version only)
* CLI and JavaFX GUI (Light Version)
* Web UI / REST API (Full Version)

---

## 11. Encryption Flow

1. **Password Salt**

   * Protects against precomputed dictionary/rainbow table attacks
   * Ensures unique master password hashes per installation

2. **Encryption Key Salt**

   * Ensures AES-256 encryption keys are unique, even for identical master passwords
   * Used by Argon2id to derive the AES-256 key for encrypting/decrypting database data

3. **Process Flow**

   * Master password + password salt → Argon2id → Session key
   * Session key + encryption key salt → Argon2id → AES-256 key
   * AES-256 key → Encrypts/decrypts vault (PostgreSQL or SQLite)

---

✅ **Summary**
CipherVault provides a **secure, self-hosted password manager** with two distinct versions, a **shared Java library** for core functionality, strong cryptography, and a clean, maintainable repository structure. The Light Version is a standalone JavaFX desktop app, and the Full Version is a web-enabled, self-hosted solution suitable for Docker deployment. The **multi-module GitHub repository** ensures clear separation, modularity, and easy maintainability.

---


