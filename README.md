# SSIS — Student Services Information System (YuloTech)

Self-service portal for students plus Registrar, Cashier, Department (clearance) and Administrator back offices.
Built from the SSIS Figma prototype and the SAD document: **React + Vite** front end, **PHP 8** REST API, **MySQL/MariaDB** database (XAMPP).

## Run it (XAMPP)

1. Start **Apache** and **MySQL** in the XAMPP Control Panel.
2. Create and seed the database (once, and any time you want to reset the demo data):
   ```
   C:\xampp\php\php.exe database\seed.php
   ```
   This creates the `yulotech_ssis` database from `database/schema.sql` and loads demo data.
   DB credentials live in `api/config.php` (default: `root`, no password).
3. Open the built site: <http://localhost/YULOTECH/dist/>
   (rebuild after front-end changes with `npm run build`).

### Development with hot reload
```
npm install
npm run dev        # http://localhost:5173, /api is proxied to http://localhost/YULOTECH
```
No Apache? Run `C:\xampp\php\php.exe -S 127.0.0.1:8000` in this folder and put `SSIS_API=http://127.0.0.1:8000` in `.env.local`.

## Demo accounts
All demo accounts use the password `password123`.

| Portal | Tab | Username |
|---|---|---|
| Student | Student | `carlos` (also `juan`, `sarah`, …) |
| Registrar | Staff | `jobert` |
| Cashier | Staff | `maria` |
| Department (Laboratory office) | Staff | `msantos` |
| Department (other offices) | Staff | `library`, `depthead`, `finance`, `affairs`, `registry` |
| Administrator | Staff | `admin` |

## What's implemented (per the SAD scope)
- **Student**: dashboard, read-only courses, grades, profile + profile-edit request, document requests (Certificate of Enrollment / Grades / Good Moral) with live fee estimate, Pending → Processing → Ready / Rejected tracking with registrar remarks, and clearance progress ("2 of 6 offices cleared"). Requests are blocked while the student has pending library books or an unpaid balance.
- **Registrar**: student records, grade encoding, review of document requests (approve / mark processing / reject — a remark is required to reject) with timeline, and approval of profile-edit requests.
- **Cashier**: student search, balance summary, record Cash / GCash / Bank payments (reference required for GCash/Bank), official receipts (preview / print), daily collections dashboard. No online payment (out of scope).
- **Department**: sequential digital clearance — Library → Laboratory → Department → Finance/Tuition → Student Affairs → Registrar. An office stays *Locked* until the previous office clears the student; Finance cannot clear a student with an unpaid balance.
- **Admin**: create / edit / deactivate accounts, assign roles, reset passwords (temporary password, forced change at next login), password-reset requests, recent activity.
- **Security**: role-based access enforced in the API, password hashing (`password_hash`), session cookies, prepared statements, activity log of sensitive actions.

## Layout
```
api/          PHP REST API (index.php + routes_*.php, lib.php, config.php)
database/     schema.sql (3NF tables) and seed.php (demo data)
src/          React app (pages/ per portal, ui.jsx shared components)
dist/         production build served by Apache
```
