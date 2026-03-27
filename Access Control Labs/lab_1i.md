# Lab Name

Unprotected Admin Functionality

# Vulnerability

Missing access control on admin functionality

The application exposes an admin panel without proper authentication or authorization checks, allowing any user to access administrative actions.

# Objective

Delete the user carlos using the unprotected admin interface.

# Methodology / Steps
1. Identifying reachable paths

A random file path was added to the URL:

/file.txt


Response returned:

404 Not Found


Observation:
The application does not restrict arbitrary path access, indicating that sensitive endpoints may be accessible without authentication.

2. Enumerating admin panel paths

Common admin endpoints were tested manually:

/admin
/administrator
/admin-panel
/administration-panel


Valid admin panel discovered at:

/administration-panel


Root Cause:
The admin panel is exposed and lacks authentication checks.

3. Exploiting unprotected admin functionality

Accessed the admin panel directly without logging in.

Used the available functionality to delete the user carlos.

🔹 Impact

Any unauthenticated user can perform administrative actions.

Leads to account takeover, data loss, and full application compromise.

🔹 Root Cause Analysis

Missing server-side authorization checks.

Admin routes are not protected by authentication middleware.

🔹 Recommended Mitigation

Restrict admin endpoints using strong authentication.

Implement role-based access control (RBAC).

Enforce authorization checks on every sensitive backend route.