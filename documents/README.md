# CipherVault

**CipherVault** is a secure, self-hosted password manager designed as an alternative to KeePass. It offers two variants:

* **Light Version** – Standalone JavaFX desktop app using SQLite
* **Full Version** – Web-enabled Java app using PostgreSQL, deployable in Docker

Core functionality is shared via a **Java library JAR** (`CipherVaultCommon`).

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

* AES-256-GCM encrypted vaults
* Argon2id password hashing with salts
* Multi-user support with RBAC (Full Version)
* Audit logging (Full Version)
* CLI and JavaFX GUI (Light Version)
* Web UI and REST API (Full Version)
* Shared Java library for core functionality (encryption, data models, database abstraction)

---

## 2. Project Structure

CipherVault is a **Maven multi-module project**:

```
/CipherVault/                  # Parent repository
├── pom.xml                    # Parent POM
├── /CipherVaultCommon/        # Shared library module
│   └── pom.xml
├── /CipherVaultLight/         # Light Version module
│   └── pom.xml
├── /CipherVaultFull/          # Full Version module
│   └── pom.xml
├── README.md
├── LICENSE
├── spec.md
├── requirements.md
└── developer-ready-blueprint.md
```

* Each module has stub classes ready for implementation.
* Parent POM builds all modules together.

---

## 3. Requirements

For full details, see [requirements.md](requirements.md) and [spec.md](spec.md).

**High-level requirements:**

* Java 17+
* Ubuntu 22.04+
* Maven build tool
* SQLite (Light Version) / PostgreSQL (Full Version)
* Docker support (Full Version)

---

## 4. Setup & Installation

### 4.1 Light Version

```bash
cd CipherVaultLight
mvn clean package
java -jar target/CipherVaultLight.jar
```

### 4.2 Full Version (Web / Docker)

```bash
cd CipherVaultFull
mvn clean package
docker build -t ciphervault-full .
docker run -d -p 8080:8080 ciphervault-full
```

Access Web UI at `http://localhost:8080`.

---

## 5. Usage

* **Light Version**: JavaFX GUI or CLI for adding, editing, and viewing vault entries.
* **Full Version**: Web UI and REST API for multi-user management, vault CRUD operations, and audit logging.

---

## 6. Encryption & Security

* **Master Password** → Argon2id + password salt → session key
* **Encryption Key Salt** → Argon2id → AES-256 key → encrypt/decrypt vault data
* Salts are **Base64-encoded** and stored in `/config/metadata.json`

---

## 7. Contributing

* Fork repository, create a feature branch:

```bash
git checkout -b feature/my-feature
```

* Commit and push:

```bash
git commit -am "Add feature"
git push origin feature/my-feature
```

* Submit a pull request for review.

---

## 8. License

**CipherVault** is released under the **MIT License**. See [LICENSE](LICENSE) for details.

---

## 9. Developer References

* [spec.md](spec.md) – Full project specification
* [requirements.md](requirements.md) – Functional & nonfunctional requirements
* [developer-ready-blueprint.md](developer-ready-blueprint.md) – Architecture, class layout, API, and Maven skeleton

---





