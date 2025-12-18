# Role-Based Access Control (RBAC) Test Results

**Project:** <Cybersec-phase3> 
**Tester:** <Markus_Paananen>

---

## Testing Methodology

The following techniques were used to populate this document:

- Browser-based manual testing
- OWASP ZAP (authenticated & unauthenticated)
- Gobuster / wfuzz endpoint discovery

All discovered endpoints (including hidden ones) are documented under the appropriate role.

---

## Guest

### Can do

> Actions a non-authenticated user can perform

- **Can view public resource list — `/`** 
  Observation: Page loads successfully without authentication 
  Spec match: Matches specification.

- **Can access login form — `/login`** 
  Observation: Login form is accessible 
  Spec match: Matches specification.

- **Can view registered reservations without identity — `/` (spec 8)** 
  Observation: Reservations visible, times and dates are visible. 
  Spec match: Matches specification.

---

### Cannot do

> Actions a Guest is correctly blocked from

- **Cannot access reservation page — `/reservation`** 
  Observation: `Process failed. Unauthorized.`
  Spec match: Expected behavior.

- **Cannot create reservation — `POST /api/reservations`** 
  Observation: error; "Not found"
  Spec match: Matches specification.

- **Cannot access admin pages — `/admin/*`** 
  Observation: The process failed. Not found.
  Spec match: Matches specification.

- **Cannot access profile page — `/profile`** 
  Observation: The process failed. Not found.
  Spec match: Matches specification.

- **Cannot access hidden endpoint — `/admin/users` (Gobuster)** 
  Observation: The process failed. Not found. 
  Spec match: Matches specification.

---

## Reserver

### Can do

> Actions an authenticated Reserver can perform

- **Can book a resource — `/reservation` + `POST /api/reservations`** 
  Observation: Reservation successfully created  and api shown.
  Spec match: Matches specification

- **Can view own profile — `/profile`** 
  Observation: The process failed. Not found.
  Spec match: Feature missing or undocumented.

- **Can list resources — `/resources`** 
  Observation: Resource list loads 
  Spec match: Allowed

- **Can view own reservations — `/reservations/my`** 
  Observation: The process failed. Not found.
  Spec match: Feature missing or undocumented.
---

### Cannot do

> Actions a Reserver must not be able to perform

- **Cannot access admin user list — `/admin/users`** 
  Observation: The process failed. Not found.
  Spec match: Matches specification

- **Cannot delete other users — `DELETE /api/admin/users/:id`** 
  Observation: Error: Not found 
  Spec match: Matches specification

- **Cannot modify resources — `/admin/resources/edit/:id`** 
  Observation: Error: Not found 
  Spec match: Matches specification

---

## Administrator

### Can do

> Actions an Administrator can perform

- **Can add a resource — `/admin/resources/new`** 
  Observation: The process failed. Not found.
  Spec match: Feature missing or undocumented.

- **Can delete a reserver — `/admin/users/delete/:id`** 
  Observation: The process failed. Not found. 
  Spec match: Feature missing or undocumented.

- **Can manage all reservations — `/admin/reservations`** 
  Observation: The process failed. Not found.
  Spec match: Feature missing or undocumented.

- **Can view all users — `/admin/users` (spec 4)** 
  Observation: The process failed. Not found.
  Spec match: Feature missing or undocumented.

---

### Cannot do

> Prohibited behaviors or implementation issues

- **Cannot book a resource — `/reservation`** 
  Observation: Booking ui is visible but booking causes the message `new row for relation "booking_reservations" violates check constraint "booking_reservations_check"` 
  Spec match: Matches specification

- **Cannot access audit log — `/admin/audit` (Gobuster)** 
  Observation: The process failed. Not found.
  Spec match: Feature missing or undocumented

---

## Hidden & Discovered Endpoints

> Pages discovered vio zap scan

- `/robots.txt`
- `/sitemap.xml`
- `/static`
- `/login`
- `/register`
- `/reservation`
- `/resources`

Zap discovered additional `/robots.txt`, `static` and `/sitemap.xml`. These do not expose sensitive data

> ZAP did not detect insecure authorization behavior.

---

## Running gobuster

> I could not run the: gobuster dir -u http://localhost:8000 -w /usr/share/wordlists/dirb/common.txt
> because the error:
┌──(hali㉿hali)-[~/Downloads]
└─$ gobuster dir -u http://localhost:8003/ -w common.txt 
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://localhost:8003/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
Progress: 0 / 1 (0.00%)
2025/12/18 12:54:56 the server returns a status code that matches the provided options for non existing urls. http://localhost:8003/76643bee-54b5-434c-b137-19631cdd9138 => 302 (redirect to /status.html?status=failed&message=%3Cstrong%3ENot%20Found%3C%2Fstrong%3E) (Length: 0). Please exclude the response length or the status code or set the wildcard option.. To continue please exclude the status code or the length

> This apparently means that since the application does not return the 404 code, the gobuster cannot differenciate between the existing paths and non existing paths.

> I fixed this by exluding the redirect status.

> gobuster dir -u http://localhost:8003/ \
-w /usr/share/wordlists/dirb/common.txt \
-b 302


> ┌──(hali㉿hali)-[~/Downloads]
└─$ gobuster dir -u http://localhost:8003/ \
-w /usr/share/wordlists/dirb/common.txt \
-b 302

===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://localhost:8003/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   302
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/login                (Status: 200) [Size: 2131]
/register             (Status: 200) [Size: 3098]
/reservation          (Status: 303) [Size: 0] [--> /status.html?status=failed&message=%3Cstrong%3EUnauthorized%3C%2Fstrong%3E] 
/resources            (Status: 200) [Size: 2473]
Progress: 4613 / 4613 (100.00%)
===============================================================
Finished
===============================================================

---
## Other notices

> Regular user should not be able to create administration user.



