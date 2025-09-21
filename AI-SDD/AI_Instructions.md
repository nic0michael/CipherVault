## **Revised AI Prompt – CipherVault Single-Project Full Version**

**Project Name:** CipherVault – Full Version (Web / Docker)
**Language / Framework:** Java 17, Spring Boot
**Database:** PostgreSQL
**Build Tool:** Maven (single project, single `pom.xml`)
**Security:** AES-256-GCM encryption, Argon2id hashing, master password + salts

**Goal:** Generate a **single Spring Boot microservice project** that manages a secure password vault. All code lives in this project (no shared library). Include **REST API, service layer, entities, request/response DTOs**, and placeholders for PostgreSQL integration.

---

### **Project Structure**

```
src/main/java/com/ciphervault/fullservice/ciphervault/
├── controller/
│   └── VaultController.java
├── entity/
│   └── VaultEntity.java
├── dao/
│   └── VaultDAO.java
├── request/
│   └── VaultRequest.java
├── response/
│   ├── VaultResponse.java
│   ├── VaultListResponse.java
│   ├── IntializeProjectResponse.java
│   └── LogResponse.java
├── service/
│   ├── VaultService.java
│   └── VaultServiceImpl.java
CiphervaultApplication.java
```

---

### **VaultController Endpoints**

* `POST /api/vault/initialize` → `initializeProject(String masterPassword)`
  Initializes the project, generates salts, returns info for user to save.
* `POST /api/vault/login` → `login(String masterPassword)`
  Authenticates user, returns session token or login info.
* `POST /api/vault/entries` → `createEntry(VaultRequest entry)`
  Creates a vault entry in the database.
* `GET /api/vault/entries/{entryName}` → `getEntry(String entryName)`
  Retrieves a single vault entry by name.
* `GET /api/vault/entries` → `getEntryList()`
  Returns all vault entries.
* `GET /api/vault/entries/search?entryName=xyz` → `searchEntryList(String entryName)`
  Returns a list of vault entries matching the search string.

---

### **Service Layer**

* **VaultService (interface)** – Defines methods matching VaultController endpoints.
* **VaultServiceImpl** – Implements VaultService with placeholder logic.
  * TODO comments for PostgreSQL CRUD operations.
  * Uses `VaultEntity`, `VaultDAO`, `VaultRequest`, `VaultResponse`, `VaultListResponse`, `IntializeProjectResponse`, `LogResponse`.

---

### **Entities / DTOs**

* **VaultEntity** – JPA entity representing vault entry.
* **VaultDAO** – Data Access Object for internal service use.
* **VaultRequest** – Request payload for creating/updating a vault entry.
* **VaultResponse** – Response payload for a single vault entry.
* **VaultListResponse** – Response payload for multiple vault entries.
* **IntializeProjectResponse** – Response for project initialization (salts, metadata).
* **LogResponse** – Response for login (session token, message).

---

### **Core Features**

* AES-256-GCM encrypted vaults
* Master password with optional key file
* Multi-user and RBAC support (Full Version)
* Audit logging, backup/restore
* CLI and JavaFX GUI support

---

### **Wishlist / Aspirational Features**

* **WISH1:** Master password + optional key file
* **WISH2:** Easy-to-use GUI / navigation
* **WISH3:** Secure folders / categories
* **WISH4:** Quick add/edit/delete vault entries
* **WISH5:** Search & filter
* **WISH6:** Export/import vault securely
* **WISH7:** Cross-platform support (Windows, Mac)
* **WISH8:** Two-Factor Authentication (Full Version)
* **WISH9:** Built-in password generator
* **WISH10:** Notifications for failed logins or vault changes

---

### **Requirements**

1. Use Spring REST annotations (`@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`).
2. Use `@Autowired` to inject VaultService into VaultController.
3. Include placeholder responses that compile and return dummy data.
4. Include JSON examples in comments for request/response objects.
5. All classes ready for AES-256 / Argon2id integration in the future.
6. Single Maven project with one `pom.xml`; no separate shared library.

---

### **Expected Output**

* VaultController.java
* VaultService.java
* VaultServiceImpl.java
* VaultEntity.java
* VaultDAO.java
* VaultRequest.java
* VaultResponse.java
* VaultListResponse.java
* IntializeProjectResponse.java
* LogResponse.java

---
