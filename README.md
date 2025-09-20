# CipherVault

**CipherVault** is a secure, self-hosted password manager designed as an alternative to KeePass. It offers two variants: a **Light Version** (standalone desktop app) and a **Full Version** (web-enabled, self-hosted, Docker-ready). Core cryptography and data models are shared via a **Java library**.

---

## Table of Contents

1. [Features](#1-features)
2. [Project Structure](#2-project-structure)
3. [Requirements](#3-requirements)
4. [Setup & Installation](#4-setup--installation)
5. [Usage](#5-usage)
6. [Encryption & Security](#6-encryption--security)
7. [Contributing](#7-contributing)
8. [License](#8-license)

---

## 1. Features

* AES-256-GCM encryption of vault data
* Argon2id password hashing with per-installation salts
* CLI and JavaFX GUI (Light Version)
* Web UI and REST API (Full Version)
* Multi-user support with RBAC (Full Version)
* Optional key file for extra protection
* Audit logging (Full Version)
* Shared Java library for core functionality (encryption, models, database abstraction)

---

## 2. Project Structure

```
/CipherVault/
├── /CipherVaultCommon/     # Shared library (Java core)
├── /CipherVaultLight/      # Standalone JavaFX app
├── /CipherVaultFull/       # Web/Docker version
├── README.md
└── .gitignore
```

* Each folder is a **separate Maven module**.
* The parent repository may include a **multi-module Maven POM**.

---

## 3. Requirements

* **OS**: Ubuntu Server 22.04 LTS or later
* **Java**: 17+
* **Build Tool**: Maven
* **Databases**:

  * SQLite (Light Version)
  * PostgreSQL (Full Version)
* **Docker**: For Full Version deployment (optional)

---

## 4. Setup & Installation

### 4.1 Light Version – Standalone

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/CipherVault.git
   ```
2. Navigate to the Light Version folder:

   ```bash
   cd CipherVault/CipherVaultLight
   ```
3. Build with Maven:

   ```bash
   mvn clean package
   ```
4. Run the JavaFX desktop app:

   ```bash
   java -jar target/CipherVaultLight.jar
   ```

### 4.2 Full Version – Web / Docker

1. Navigate to the Full Version folder:

   ```bash
   cd CipherVault/CipherVaultFull
   ```
2. Build the project with Maven:

   ```bash
   mvn clean package
   ```
3. Run in Docker:

   ```bash
   docker build -t ciphervault-full .
   docker run -d -p 8080:8080 ciphervault-full
   ```
4. Access the Web UI at `http://localhost:8080`

---

## 5. Usage

* **Light Version**: Use the JavaFX GUI or CLI to manage vault entries, import/export, and back up the vault file.
* **Full Version**: Use the Web UI for multi-user access, audit logging, and REST API integration.

---

## 6. Encryption & Security

* **Master Password**: hashed with Argon2id + password salt
* **AES-256-GCM**: encrypts all vault data
* **Encryption Key Salt**: ensures unique AES key per installation
* **Metadata File**: stores Base64-encoded salts and crypto parameters in `/config/metadata.json`

---

## 7. Contributing

* Fork the repository and create a feature branch:

  ```bash
  git checkout -b feature/my-feature
  ```
* Commit your changes and push:

  ```bash
  git commit -am "Add feature"
  git push origin feature/my-feature
  ```
* Submit a pull request for review.

---

## 8. License

**CipherVault** is released under the **MIT License**.
See [LICENSE](LICENSE) for details.

---


