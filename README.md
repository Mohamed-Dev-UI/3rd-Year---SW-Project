## 1. Introduction

Libro is a library app where users can find, view, and download learning materials (courses, books, articles, research papers).

---

## 2. Actors

- **User** (Learner): the main person using the app.
- **Server**: handles authentication, search, and download requests. (Can be treated as a secondary actor or as part of the system.)
- **Database**: stores user accounts and material data. (Usually part of the system, but can be shown as a lifeline in sequence diagrams.)

---

## 3. Use Cases

### Authentication & Onboarding
- Sign In
- Sign Up
- Select Preferred Topics

### Finding & Using Materials
- Search Material
- Filter Material
- View Material
- Download Material
- Bookmark Material
- Rate Material

### Managing Downloads
- View Downloads
- Delete Download

### Profile Management
- Edit Profile
- Switch Account
- Add Account
- Remove Account

---

## 4. Sequence Diagrams

### 4.1 User Login

**Lifelines:** User, App UI, Server, Database

1. User → App UI: enter email and password
2. App UI → Server: send login credentials
3. Server → Database: query user by email
4. Database → Server: return user record
5. Server → Server: validate password
6. Server → App UI: return success/failure
7. App UI → User: show success screen or error message

---

### 4.2 Search for Material

**Lifelines:** User, App UI, Server, Database

1. User → App UI: enter search query or select filter
2. App UI → Server: send search request
3. Server → Database: query materials matching input
4. Database → Server: return matching materials
5. Server → App UI: return results (or empty list)
6. App UI → User: display results (downloaded items pinned at top)

---

### 4.3 Download Material

**Lifelines:** User, App UI, Server, Database, Local Storage

1. User → App UI: tap download on a material
2. App UI → Server: request download URL
3. Server → Database: fetch material metadata
4. Database → Server: return metadata and URL
5. Server → App UI: send download URL
6. App UI → Local Storage: save file
7. App UI → User: show download progress and completion
8. App UI → Server: update download status (optional)