# Changes Made to Filebrowser Frontend

This document summarizes the modifications applied to the `filebrowser` frontend as per your requests.

## 1. Removed Hamburger Menu

*   **File:** `frontend/src/views/bars/Default.vue`
*   **Description:** The `<action>` component acting as the hamburger menu button (icon="close_back") in the top-left corner of the header was removed. This eliminates the button used to toggle the sidebar.

## 2. Fixed Header Layout

*   **File:** `frontend/src/views/bars/Default.vue`
*   **Description:** After removing the hamburger menu, the layout of the header shifted. A `<div style="flex: 1;"></div>` spacer was added between the title/search component and the action buttons to push the latter to the right, restoring the intended alignment.

## 3. Replaced "+ File actions" with "Upload" Button

*   **File:** `frontend/src/components/sidebar/General.vue`
*   **Description:**
    *   The original "+ File actions" button and its surrounding `transition` wrapper were removed.
    *   A new "Upload" button was introduced, labeled with `$t("general.upload")` and using the `cloud_upload` icon.
    *   A new method, `showUploadHover`, was added to the component's script to directly trigger the upload prompt (`mutations.showHover("upload")`) when the new button is clicked.
    *   Spacing between the "Upload" icon and text was adjusted by adding a non-breaking space (`&nbsp;`) in the template.

## 4. Removed Quick Toggles

*   **File:** `frontend/src/components/sidebar/General.vue`
*   **Description:** The entire `div` block with `class="quick-toggles"` was removed. This eliminated the "toggle one-click", "light/dark", and "pin" icon buttons from the sidebar.

## 5. Removed Sidebar Links Card

*   **File:** `frontend/src/components/sidebar/General.vue`
*   **Description:** The `<SidebarLinks />` component at the bottom of the `General.vue` template was removed, eliminating the section for selecting sidebar options.

## 6. Removed Settings Cogwheel and Link from Username

*   **File:** `frontend/src/components/sidebar/General.vue`
*   **Description:** ## 7. Robust Permission Handling

*   **File:** `frontend/src/store/getters.js`
*   **Description:**
    *   Updated the `permissions` getter to handle both legacy (`perm`) and standard (`permissions`) field names from the backend.
    *   Added default `false` values for all permission bits to prevent `undefined` checks.
    *   Updated all permission-dependent getters (like `isAdmin`, `fileViewingDisabled`, etc.) to use this unified `permissions()` getter for better consistency.

## 8. Improved Upload Button Visibility

*   **File:** `frontend/src/components/sidebar/General.vue`
*   **Description:**
    *   Added an `isDataLoaded` check to the "Upload" button container. This ensures the button only renders once the user's profile and permissions are fully loaded from the API, preventing it from flickering or disappearing during page load.
    *   Modified `isDataLoaded` to verify the presence of a `username` or `share hash` for more reliable state detection.

## 9. Backend Permission & Default Fixes

*   **File:** `backend/common/settings/settings.go`
*   **Description:**
    *   Updated `AdminPerms()` to explicitly include all permissions (`Create`, `Delete`, `Download`, `Realtime`) by default.
    *   Fixed `ApplyUserDefaults()` to correctly propagate several settings from `config.yaml` to the user object, including `HideSidebarFileActions`, `EditorQuickSave`, and `DeleteWithoutConfirming`.
    *   Ensured that the `hideSidebarFileActions` setting in `config.yaml` is correctly respected by both the backend and frontend.