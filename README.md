# Libro UML Diagrams — Implementation Guide

## 1. Project Overview

Libro is a library app where users can find, view, and download learning materials (courses, books, articles, research papers). This guide defines the exact steps to produce the UML use‑case and sequence diagrams for Libro. It covers the full scope, the team split, and the conventions everyone must follow. All diagrams are created with **Software Ideas Modeler** and committed as source files plus exported images.

---

## 2. Actors and System Boundary

- **System boundary name:** `Libro`
- **Primary actor:** `User` (Learner)
- **Secondary actor in use‑case diagrams:** `Server` — only where it genuinely supports a use case (e.g., sign‑in, search, download).
- **Database:** never drawn as an actor in use‑case diagrams. It appears only as a lifeline in sequence diagrams.
- **In sequence diagrams:** the User actor appears as a lifeline named `user : User`.

---

## 3. Full List of Use Cases

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

## 4. Team Work Split and File Ownership

Work is divided into four feature areas. Each member owns one use‑case diagram, one sequence diagram, two flow‑of‑events documents, and one traceability file. No member edits another member’s source files.

| Member | Feature area | Use‑case diagram (source + image) | Sequence diagram (source + image) | Flow‑of‑events files | Traceability file |
|--------|--------------|-----------------------------------|-----------------------------------|----------------------|-------------------|
| **M1** | Authentication & onboarding | `diagrams/use-cases/M1-auth-usecase.*` | `diagrams/sequences/M1-login-sequence.*` | `flows/M1-login-flow.md`<br>`flows/M1-signup-summary.md` | `traceability/M1-trace.md` |
| **M2** | Discovery & viewing | `diagrams/use-cases/M2-discovery-usecase.*` | `diagrams/sequences/M2-search-sequence.*` | `flows/M2-search-flow.md`<br>`flows/M2-bookmark-summary.md` | `traceability/M2-trace.md` |
| **M3** | Download & material actions | `diagrams/use-cases/M3-download-usecase.*` | `diagrams/sequences/M3-download-sequence.*` | `flows/M3-download-flow.md`<br>`flows/M3-delete-download-summary.md` | `traceability/M3-trace.md` |
| **M4** | Profile & accounts | `diagrams/use-cases/M4-profile-usecase.*` | `diagrams/sequences/M4-switch-account-sequence.*` | `flows/M4-switch-account-flow.md`<br>`flows/M4-add-account-summary.md` | `traceability/M4-trace.md` |

**Integration lead:** M1. M1 assembles the final report after all branches are merged. M1 does not rewrite other members’ diagram source files.

---

## 5. No‑Conflict Rules

Follow these rules to avoid merge conflicts and lost work:

1. **One owner per file.** Only the assigned member edits their use‑case source, sequence source, exported image, and flow‑of‑events files.
2. **Work on separate branches:**
   - `m1-auth`
   - `m2-discovery`
   - `m3-download`
   - `m4-profile`
3. **Never edit another member’s diagram source.** Request changes through pull‑request comments.
4. **Shared files** (`README.md`, `common-conventions.md`, final report index) are edited only by M1 after review.
5. **Export images** with the same base filename as the source (e.g., `M1-auth-usecase.png`).
6. **Link to files** in the final report instead of copy‑pasting diagram content — one source of truth.
7. **Traceability is split by member.** Each member writes their own traceability file; M1 merges the rows.

---

## 6. Shared Conventions for All Diagrams

### 6.1 Use‑Case Diagram Rules

- Use cases are named **verb + noun** (e.g., `Sign In`, `Search Material`).
- Optional namespace: `Libro::Sign In`.
- Actors are placed outside the system boundary.
- Use cases are placed inside the system boundary.
- Use the correct relationship direction:

| Relationship | Notation | Direction |
|--------------|----------|-----------|
| Association | Solid line | Actor ↔ use case |
| Include | Dashed arrow labeled `<<include>>` | Base use case → included use case |
| Extend | Dashed arrow labeled `<<extend>>` | Extending use case → base use case |
| Generalization | Solid line with hollow triangle arrowhead | Specialized → general |

- No Database actor.

### 6.2 Sequence Diagram Rules

Each sequence diagram must contain:

- Actor and lifelines named `instance : Class` (e.g., `ui : AppUI`, `server : Server`, `db : Database`, `storage : LocalStorage`).
- Activation bars where a lifeline is processing.
- Numbered messages in chronological order.
- Synchronous messages (solid arrowhead), reply messages (dotted line, open arrowhead), and self‑messages where appropriate.
- Guards such as `[credentials valid]`.
- An `alt` frame for main/exceptional or success/failure branches.
- An `opt` frame for optional steps.
- Create/delete/lost/found messages only when they add real meaning.

---

## 7. Step‑by‑Step Implementation per Member

### Member 1 — Authentication & Onboarding

#### Step 1: Create the use‑case diagram `M1-auth-usecase`

Draw the system boundary `Libro` and include these use cases:

- `Sign In`
- `Sign Up`
- `Select Preferred Topics`

Add actors and relationships:

- Actor `User` associated with all three use cases.
- Secondary actor `Server` associated with `Sign In` and `Sign Up`.
- `Sign In` **includes** `Validate Credentials` (`<<include>>`).
- `Display Login Error` **extends** `Sign In` (`<<extend>>`).
- `Sign Up` **includes** `Create Account`.
- `Select Preferred Topics` shown as an optional onboarding step after sign‑up (use `<<extend>>` only if it is truly optional).

Export as `M1-auth-usecase.png`.

#### Step 2: Create the sequence diagram `M1-login-sequence`

Use the following lifelines: `user : User`, `ui : AppUI`, `server : Server`, `db : Database`.

Draw these messages in order:

1. User → App UI: enter email and password.
2. App UI → Server: send login credentials.
3. Server → Database: query user by email.
4. Database → Server: return user record.
5. Server → Server: validate password (self‑message).
6. Server → App UI: return success/failure.
7. App UI → User: show success screen or error message.

Add an `alt` frame:

- `[credentials valid]` → success screen
- `[else]` → error message

Add guard `[password matches]` on the self‑message.

Export as `M1-login-sequence.png`.

#### Step 3: Write flow‑of‑events documents

Create `flows/M1-login-flow.md` using the template in Section 9. Include:

- Use case name: `Libro::Sign In`
- Primary actor: User; Secondary actor: Server
- Main flow
- Exceptional flow: wrong password, unknown email, server unavailable
- NFR notes: login response under 2 seconds, credentials encrypted in transit

Create `flows/M1-signup-summary.md` with main and exceptional flow for `Sign Up`.

#### Step 4: Fill traceability file `traceability/M1-trace.md`

Add rows for `Sign In`, `Sign Up`, `Select Preferred Topics` using the columns in Section 10.

---

### Member 2 — Discovery & Viewing

#### Step 1: Create the use‑case diagram `M2-discovery-usecase`

Draw the system boundary `Libro` and include these use cases:

- `Search Material`
- `Filter Material`
- `View Material`
- `Bookmark Material`

Add actors and relationships:

- Actor `User` associated with all four use cases.
- `Filter Material` **extends** `Search Material` (`<<extend>>`) — filtering is optional after searching.
- `Bookmark Material` **extends** `View Material` (`<<extend>>`) — bookmarking is optional from viewing.
- Optional: `Server` as secondary actor for `Search Material`.

Export as `M2-discovery-usecase.png`.

#### Step 2: Create the sequence diagram `M2-search-sequence`

Use lifelines: `user : User`, `ui : AppUI`, `server : Server`, `db : Database`.

Draw these messages in order:

1. User → App UI: enter search query or select filter.
2. App UI → Server: send search request.
3. Server → Database: query materials matching input.
4. Database → Server: return matching materials.
5. Server → App UI: return results (or empty list).
6. App UI → User: display results (downloaded items pinned at top).

Add an `alt` frame:

- `[results found]` → display results
- `[else]` → show “No materials found”

Export as `M2-search-sequence.png`.

#### Step 3: Write flow‑of‑events documents

Create `flows/M2-search-flow.md` using the template. Include:

- Main flow
- Exceptional flow: empty result, invalid filter, server timeout
- NFR notes: fast search response, results ordered with downloads pinned

Create `flows/M2-bookmark-summary.md` for `Bookmark Material`.

#### Step 4: Fill traceability file `traceability/M2-trace.md`

Add rows for `Search Material`, `Filter Material`, `View Material`, `Bookmark Material`.

---

### Member 3 — Download & Material Actions

#### Step 1: Create the use‑case diagram `M3-download-usecase`

Draw the system boundary `Libro` and include these use cases:

- `Download Material`
- `Rate Material`
- `View Downloads`
- `Delete Download`

Add actors and relationships:

- Actor `User` associated with all four use cases.
- Secondary actor `Server` associated with `Download Material`.
- `Download Material` **includes** `Request Download URL` (`<<include>>`).
- `Delete Download` **extends** `View Downloads` (`<<extend>>`) — deletion is optional from the downloads list.
- `Rate Material` associated with `View Material` or shown as a separate user action.

Export as `M3-download-usecase.png`.

#### Step 2: Create the sequence diagram `M3-download-sequence`

Use lifelines: `user : User`, `ui : AppUI`, `server : Server`, `db : Database`, `storage : LocalStorage`.

Draw these messages in order:

1. User → App UI: tap download on a material.
2. App UI → Server: request download URL.
3. Server → Database: fetch material metadata.
4. Database → Server: return metadata and URL.
5. Server → App UI: send download URL.
6. App UI → Local Storage: save file.
7. App UI → User: show download progress and completion.
8. App UI → Server: update download status (optional).

Add:

- `alt` frame for `[download succeeds]` / `[download fails]`
- `opt` frame for `[update download status]`

Export as `M3-download-sequence.png`.

#### Step 3: Write flow‑of‑events documents

Create `flows/M3-download-flow.md` using the template. Include:

- Main flow
- Exceptional flow: no network, invalid URL, insufficient storage, interrupted download
- NFR notes: progress feedback, saved file integrity, optional offline access

Create `flows/M3-delete-download-summary.md` for `Delete Download`.

#### Step 4: Fill traceability file `traceability/M3-trace.md`

Add rows for `Download Material`, `Rate Material`, `View Downloads`, `Delete Download`.

---

### Member 4 — Profile & Accounts

#### Step 1: Create the use‑case diagram `M4-profile-usecase`

Draw the system boundary `Libro` and include these use cases:

- `Manage Account` (general use case)
- `Edit Profile`
- `Switch Account`
- `Add Account`
- `Remove Account`

Add actors and relationships:

- Actor `User` associated with the account use cases.
- **Generalization** arrows from `Edit Profile`, `Switch Account`, `Add Account`, `Remove Account` to `Manage Account`. This diagram is the team’s required example of generalization.
- Secondary actor `Server` for account operations where account data is validated.

Export as `M4-profile-usecase.png`.

#### Step 2: Create the sequence diagram `M4-switch-account-sequence`

Use lifelines: `user : User`, `ui : AppUI`, `server : Server`, `db : Database`, `storage : LocalStorage`.

Draw these messages in order:

1. User → App UI: select “Switch Account”.
2. App UI → Local Storage: read saved accounts.
3. Local Storage → App UI: return account list.
4. App UI → User: show account list.
5. User → App UI: select an account.
6. App UI → Server: request account/session validation.
7. Server → Database: query account record.
8. Database → Server: return account record.
9. Server → App UI: return success/failure.
10. App UI → Local Storage: set active account.
11. App UI → User: show selected profile.

Add:

- `alt` frame: `[account valid]` / `[session expired or account missing]`
- `opt` frame: `[add new account]`

Export as `M4-switch-account-sequence.png`.

#### Step 3: Write flow‑of‑events documents

Create `flows/M4-switch-account-flow.md` using the template. Include:

- Main flow
- Exceptional flow: expired session, removed account, invalid credentials
- NFR notes: account data protected, fast switch, local account list consistency

Create `flows/M4-add-account-summary.md` for `Add Account`.

#### Step 4: Fill traceability file `traceability/M4-trace.md`

Add rows for `Edit Profile`, `Switch Account`, `Add Account`, `Remove Account`.

---

## 8. Checklists

### 8.1 Use‑Case Diagram Checklist

Every use‑case diagram must have:

- [ ] System boundary named `Libro`
- [ ] Actors outside the boundary
- [ ] Use cases inside the boundary
- [ ] Verb + noun names
- [ ] Association lines actor ↔ use case
- [ ] At least one `<<include>>` where required shared behavior exists
- [ ] At least one `<<extend>>` where optional/exceptional behavior exists
- [ ] Generalization only where “is‑a” is true
- [ ] No Database actor
- [ ] Readable layout and exported image

### 8.2 Sequence Diagram Checklist

Every sequence diagram must have:

- [ ] Title matching one use case
- [ ] Actor/lifelines named `instance : Class`
- [ ] Activation bars
- [ ] Numbered messages in time order
- [ ] Synchronous request messages
- [ ] Reply messages (dotted line, open arrowhead)
- [ ] Self‑message where internal validation occurs
- [ ] Guard `[condition]` where flow depends on a condition
- [ ] `alt` frame for main/exceptional flow
- [ ] `opt` frame for optional steps
- [ ] Create/delete/lost/found only if meaningful

---

## 9. Flow‑of‑Events Template

Each flow file must use this structure:

```markdown
# Use Case: Libro::<Use Case Name>

## Actors
- Primary:
- Secondary:

## Preconditions
- ...

## Main Flow
1. ...
2. ...

## Exceptional Flow
- ...

## Postconditions
- ...

## NFR Notes
- ...
```

---

## 10. Traceability Matrix

Each member fills their own file (`traceability/M1-trace.md`, etc.) using these columns:

| README use case | Owner | Use‑case diagram | Flow file | Sequence diagram | Notes |
|-----------------|-------|------------------|-----------|------------------|-------|

M1 merges these rows into the final report.

---

## 11. Review and Definition of Done

Each member’s work is done when:

- [ ] Use‑case diagram source and exported image are committed
- [ ] Sequence diagram source and exported image are committed
- [ ] Flow‑of‑events files are complete
- [ ] Traceability rows are complete
- [ ] No shared file was edited outside ownership
- [ ] Review partner approved the pull request

**Review rotation:**

- M1 reviews M2
- M2 reviews M3
- M3 reviews M4
- M4 reviews M1

**Final integration (M1):**

- Create a final report index linking all diagrams and flow files.
- Merge traceability rows into a single matrix.
- Do not rewrite other members’ diagram source files.
- Submit the complete package: 4 use‑case diagrams, 4 sequence diagrams, 8 flow‑of‑events documents, and the merged traceability matrix.