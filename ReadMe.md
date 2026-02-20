# Library Management System

A console-based Library Management System built in C++ as an **Object-Oriented Programming** (OOP) course project (SY-25). It supports two roles — **Admin** and **User** — and persists all data using binary flat files.

---

## Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Class Overview](#class-overview)
- [Prerequisites](#prerequisites)
- [Build & Run](#build--run)
- [Default Credentials](#default-credentials)
- [Usage](#usage)
  - [Admin Menu](#admin-menu)
  - [User Menu](#user-menu)
  - [Hidden Admin Commands](#hidden-admin-commands)
- [Data Storage](#data-storage)
- [Activity Log](#activity-log)
- [Contributing](#contributing)

---

## Features

- **Admin role**
  - Add, search (by ID / name), update, and delete admin accounts
  - Add single or multiple books; search books by ID, name, author, publisher, or genre
  - Update and delete book records
  - Issue books to users and process returns
  - View all issues and all currently un-returned issues
  - Add and search users
  - View the activity log (master admin only)
  - Reset individual databases (master admin only)

- **User role**
  - View own profile and full book-issue history
  - Search users by ID or name
  - Issue books
  - Browse the entire book catalogue and search books

- **Master Admin** — a built-in super-admin account with exclusive access to the log viewer and database-reset commands

- **Activity Logging** — every significant database operation is timestamped and written to `log.txt`

---

## Project Structure

```
LibraryManagementOOPS/
├── login.cpp           # Entry point – login screen and main()
├── admin.h             # admin class (data + getters/setters)
├── admin_database.h    # admin_database class (CRUD + login + logging)
├── adminMenu.h         # admin_menu class (interactive Admin menu)
├── book.h              # book class (data + getters/setters)
├── book_database.h     # book_database class (CRUD + search + modify)
├── user.h              # user class (data + getters/setters)
├── user_database.h     # user_database class (CRUD + search + login)
├── userMenu.h          # user_menu class (interactive User menu)
├── issue.h             # issue class (data + issue/return logic)
├── issue_database.h    # issue_database class (CRUD + return logic)
├── admin.bin           # Admin records (binary, auto-created at runtime)
├── book.bin            # Book records  (binary, auto-created at runtime)
├── user.bin            # User records  (binary, auto-created at runtime)
├── issue.bin           # Issue records (binary, auto-created at runtime)
├── log.txt             # Activity log  (text,   auto-created at runtime)
└── ReadMe.md
```

---

## Class Overview

| Class | File | Responsibility |
|---|---|---|
| `admin` | `admin.h` | Stores admin name, ID, gender, username, password |
| `admin_database` | `admin_database.h` | Upload / download / search / delete admins; login; logging |
| `admin_menu` | `adminMenu.h` | Console menu driving all admin operations |
| `book` | `book.h` | Stores book ID, name, author, genre, publisher, quantity, present quantity |
| `book_database` | `book_database.h` | Upload / download / search / delete / modify books |
| `user` | `user.h` | Stores user name, ID, gender, phone, username, password |
| `user_database` | `user_database.h` | Upload / download / search / delete users; login |
| `user_menu` | `userMenu.h` | Console menu driving all user operations |
| `issue` | `issue.h` | Stores issue ID, book ID, user ID, admin ID, issue date, return date, return status |
| `issue_database` | `issue_database.h` | Upload / download issues; process returns; filter un-returned |

---

## Prerequisites

- A C++ compiler supporting **C++11** or later (e.g. `g++`, MinGW on Windows, MSVC)
- The project uses `<windows.h>` (`Sleep`, `system("cls")`), so it is designed to run on **Windows**. On Linux/macOS you would need to replace those calls with POSIX equivalents.

---

## Build & Run

```bash
g++ login.cpp -o login
./login          # Linux/macOS (after patching Windows-specific calls)
login.exe        # Windows
```

A pre-built Windows executable (`SY25_OOPSCP.exe`) is also included in the repository.

---

## Default Credentials

| Role | Username | Password |
|---|---|---|
| Master Admin | `MASTER@ADMIN` | `ADMIN123` |

> The master admin account is hard-coded and does not need to exist in `admin.bin`. Regular admin accounts are created through the Admin Menu (option 1) and stored in `admin.bin`.

---

## Usage

On launch you are presented with the **LibSys** login screen:

```
==============================
          LibSys
==============================
1. ADMIN
2. USER
------------------------------
Option :
```

Enter `-1` at any menu prompt to exit the application.

### Admin Menu

After a successful admin login the following options are available:

| Option | Action |
|---|---|
| 1 | Add a new admin (master login required) |
| 2 | List all admins |
| 3 | Search admin by ID |
| 4 | Search admin by name |
| 5 | Delete an admin (master login required) |
| 6 | Update an admin (master login required) |
| 7 | Add a single book |
| 8 | Add multiple books |
| 9 | List all books |
| 10 | Search books (by ID / name / author / publisher / genre) |
| 11 | Delete a book |
| 12 | Update a book |
| 13 | Issue a book to a user |
| 14 | List all issue records |
| 15 | Process a book return |
| 16 | List all un-returned issues |
| 17 | Add a user |
| 18 | Add multiple users |
| 19 | List all users |
| 20 | Search user by User ID |
| 21 | Search user by name |
| -1 | Exit |

### User Menu

After a successful user login the following options are available:

| Option | Action |
|---|---|
| 1 | View own profile and issue history |
| 2 | Search user by ID |
| 3 | Search user by name |
| 4 | Delete own account |
| 5 | Update own details |
| 6 | Issue a book |
| 7 | Browse all books |
| 8 | Search books |
| -1 | Exit |

### Hidden Admin Commands

These commands are not printed in the Admin Menu but are accepted at the `Option :` prompt. They require master admin credentials where noted.

| Option | Action |
|---|---|
| 100 | View activity log (master admin credentials required) |
| 101 | Delete `issue.bin` (reset issue database) |
| 102 | Delete `user.bin` (reset user database) |
| 103 | Delete `admin.bin` (reset admin database) |
| 104 | Delete `book.bin` (reset book database) |
| 105 | View issue history for a specific user (master admin required) |

> **Warning:** Options 101–104 permanently delete the corresponding database file. There is no confirmation prompt.

---

## Data Storage

All records are persisted as raw binary data using C++ `fstream`. The files are created automatically in the working directory the first time data is written.

| File | Contents |
|---|---|
| `admin.bin` | Serialised `admin` objects |
| `book.bin` | Serialised `book` objects |
| `user.bin` | Serialised `user` objects |
| `issue.bin` | Serialised `issue` objects |
| `userfolder/<userID>.bin` | Per-user issue history (serialised `issue` objects) |

Book issue dates are set to the current date; **return dates are calculated as 10 days from the issue date**.

---

## Activity Log

Every admin database operation (upload, download, search, delete, login) appends a timestamped entry to `log.txt`. The log can be viewed from the Admin Menu using hidden command **100** (master admin login required).

---

## Contributing

Any addition of a feature should be documented in this README. Please update the relevant section(s) when adding new functionality.
