# Flow of Events: Download Material

## Primary Actor
User (Learner)

## Preconditions
- User is signed in to Libro app.
- Material details page is open.

## Main Flow (Success Scenario)
1. User taps the "Download" button on a selected material.
2. App UI requests the download URL from the Server.
3. Server queries the Database to fetch material metadata and download URL.
4. Database returns material metadata and valid URL to Server.
5. Server responds to App UI with the download URL.
6. App UI initiates file download and saves it to Local Storage.
7. App UI displays download progress to User and notifies upon completion.
8. [Optional] App UI updates the download status on Server.

## Exceptional Flows
- **EF-1: No Network Connection**
  - App UI detects no network before/during request and shows "Network Error, check connection".
- **EF-2: Invalid Download URL / Server Error**
  - Server returns error 404 or 500; App UI displays "File unavailable".
- **EF-3: Insufficient Storage**
  - App UI checks device local storage. If space is insufficient, download halts and alerts User.
- **EF-4: Interrupted Download**
  - Download breaks midway; App UI offers option to "Resume" or "Retry".

## Non-Functional Requirements (NFR Notes)
- **Progress Feedback:** Real-time progress bar displayed during downloading.
- **Data Integrity:** File hash validated after download completes to ensure no corruption.
- **Offline Access:** Saved files accessible via Local Storage without active internet connection.
