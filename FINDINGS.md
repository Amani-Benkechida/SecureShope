Finding #1 — Broken Access Control on /api/admin/orders
Status: Confirmed
Phase found: Authentication / Authorization recon
Severity: High
Endpoint: GET /api/admin/orders
Expected behavior: Should require a valid JWT belonging to a user with the admin role, consistent with sibling endpoints (/api/admin/users, /api/admin/messages).
Actual behavior: Returns 200 OK with full order data for all users — including order totals, items, and shipping addresses — with no Authorization header required at all.
Steps to reproduce:
Send an unauthenticated GET request to /api/admin/orders (e.g. via Swagger UI at /docs, or curl, with no Authorization header).
Observe that full order records for every user are returned.
Impact: Any unauthenticated party can enumerate all orders placed in the system, exposing business data and user PII (shipping addresses) at scale.
Root cause (my analysis): The two neighboring admin endpoints each depend on get_current_user + require_admin. This endpoint's handler appears to be missing that same dependency — most likely an oversight when the endpoint was written or copy-pasted from a non-admin route.
Remediation: Add the same auth/role dependency used elsewhere in the admin router (current=Depends(get_current_user) followed by require_admin(current)) before the database query executes.
Finding #2 — 
Status: Investigating
Phase found:
Severity:
Endpoint:
Expected behavior:
Actual behavior:
Steps to reproduce:
Impact:
Remediation:
Notes / leads to revisit
/api/debug/config returns configuration data with no auth required — worth a closer look for information disclosure later; haven't assessed full impact yet.
JWT tampering (editing role claim) was tested and correctly rejected by the server — signature validation appears to be working as intended for this attack path. Common weak-secret guesses did not validate either. Noting as a ruled-out avenue, not a finding.
