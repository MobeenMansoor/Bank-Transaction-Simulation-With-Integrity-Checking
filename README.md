# 🏦 Banking Transaction Simulation

A secure, full-stack banking simulation built with **Python Flask** that demonstrates core **Data Structures & Algorithms** concepts — including **Merkle Trees**, **SHA-256 hashing**, and **blockchain-style ledger chaining** — applied to a realistic financial transaction system.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Data Structures & Algorithms](#data-structures--algorithms)
- [Installation](#installation)
- [Usage](#usage)
- [Application Routes](#application-routes)
- [Security Model](#security-model)
- [Screenshots](#screenshots)
- [Contributors](#contributors)
- [License](#license)

---

## Overview

This project simulates a banking environment where users can send money, view transaction history, manage account limits, and download transcripts — all while the system maintains **cryptographic integrity** of every transaction using Merkle Trees and hash chains. An admin panel provides full oversight with the ability to approve, reject, or modify transactions before they are finalized.

The system is designed as a university-level project that bridges **Information Security**, **Software Engineering**, and **Data Structures & Algorithms** by applying theoretical concepts to a practical, user-facing application.

---

## Features

### 👤 User Features
| Feature | Description |
|---|---|
| **Secure Login** | Account ID + PIN authentication with SHA-256 hashed credentials |
| **Dashboard** | Real-time balance display with latest transaction details |
| **Send Money** | Transfer funds to other accounts with two modes: *Fast* and *Standard* |
| **Transaction History** | View all past transactions with filtering and search |
| **Transaction Limits** | Configure Online, ATM, International, and POS withdrawal limits |
| **Personal Details** | View and update profile information (username, email, phone, address) |
| **Integrity Verification** | Verify transaction integrity using Merkle Root comparison |
| **Transcript Download** | Generate and download account statements in **PDF** or **TXT** format |

### 🔐 Admin Features
| Feature | Description |
|---|---|
| **Transaction Queue** | View and manage all pending transactions |
| **Approve / Reject** | Manually approve or reject standard-mode transactions |
| **Amount Modification** | Adjust the final amount during approval (with theft/subsidy tracking) |
| **Account Management** | Lock/unlock user accounts |
| **Ledger View** | Full transaction ledger with Merkle Root display |
| **Auto-Processing** | Fast-mode transactions are auto-approved after 30 seconds |

### 🛡️ Security Features
- **SHA-256 PIN Hashing** — PINs are never stored in plaintext
- **Account Locking** — Accounts auto-lock after 3 failed login attempts
- **Integrity Hash Sealing** — Standard-mode transactions are sealed with a hash at creation time
- **Blockchain-style Hash Chain** — Each transaction references the hash of the previous one
- **Merkle Tree Verification** — Global Merkle Root validates the integrity of the entire ledger
- **Tamper Detection** — Integrity hash mismatches automatically roll back fraudulent transactions

---

## Technology Stack

| Layer | Technology |
|---|---|
| **Backend** | Python 3, Flask, Flask-Login |
| **Frontend** | HTML5, CSS3, Vanilla JavaScript |
| **Data Storage** | JSON files (`user.json`, `transaction.json`, `snapshots.json`) |
| **Cryptography** | SHA-256 (via `hashlib`) |
| **Data Structures** | Merkle Tree (custom implementation) |
| **PDF Generation** | FPDF (optional) |
| **Authentication** | Flask-Login with `UserMixin` |

---

## Project Structure

```
DSA_IS_SE_Project/
│
├── main.py                        # Flask application — all routes and business logic
├── markle_tree.py                 # Merkle Tree data structure implementation
├── requirements.txt               # Python dependencies
│
├── data/                          # Persistent JSON data store
│   ├── user.json                  # User accounts, credentials, and limits
│   ├── transaction.json           # Finalized transaction ledger (hash-chained)
│   └── snapshots.json             # Pending transaction queue
│
├── templates/                     # Jinja2 HTML templates
│   ├── login.html                 # Login page
│   ├── dashboard.html             # User dashboard
│   ├── admin_dashboard.html       # Admin control panel
│   ├── send_money.html            # Fund transfer page
│   ├── history.html               # Transaction history
│   ├── limit.html                 # Transaction limit management
│   ├── personal_details.html      # Profile settings
│   ├── verify_integrity.html      # Merkle Tree integrity checker
│   └── download_transcript.html   # Statement/transcript generator
│
└── static/                        # CSS and JavaScript assets
    ├── login.css / login.js
    ├── dashboard.css / dashboard.js
    ├── send_money.css / send_money.js
    ├── history.css / history.js
    ├── limit.css / limit.js
    ├── personal_details.css / personal_details.js
    ├── verify_integrity.css / verify_integrity.js
    ├── download_transcript.css / download_transcript.js
    ├── notifications.css / notifications.js
    └── admin_updates.js
```

---

## Data Structures & Algorithms

### 🌳 Merkle Tree (`markle_tree.py`)

The Merkle Tree is the core data structure used for **transaction integrity verification**. It is a binary tree where:

- **Leaf nodes** store SHA-256 hashes of individual transaction amounts
- **Internal nodes** store the hash of the concatenation of their two children
- The **Merkle Root** is a single hash that represents the integrity of all transactions

```
         [Root Hash]
        /            \
   [Hash(A+B)]    [Hash(C+D)]
   /       \       /       \
 [H(tx1)] [H(tx2)] [H(tx3)] [H(tx4)]
```

**Key Operations:**

| Method | Description | Complexity |
|---|---|---|
| `makeTreeFromArray(arr)` | Build a complete binary tree from transaction strings | O(n) |
| `calculateMerkleRoot()` | Compute the root hash via recursive hashing | O(n) |
| `getMerkleRoot()` | Return the cached Merkle Root | O(1) |
| `verifyUtil(arr)` | Re-build a tree from new data and compare roots | O(n) |

### 🔗 Hash Chain (Blockchain-style Ledger)

Each finalized transaction stores:
- `previous_hash` — the hash of the preceding transaction
- `hash` — `SHA-256(transaction_data + previous_hash)`

This creates an **immutable chain** where altering any past transaction would invalidate all subsequent hashes.

### 🔐 SHA-256 Hashing

Used for:
1. **PIN storage** — User PINs are hashed before storage
2. **Integrity sealing** — Standard-mode transactions are sealed with a hash at creation
3. **Ledger chaining** — Each transaction hash depends on the previous transaction
4. **Merkle Tree nodes** — All tree nodes store SHA-256 digests

---

## Installation

### Prerequisites
- **Python 3.8+** installed on your system
- **pip** package manager

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/KiritoTempest175/Banking-Transaction-Simulation.git
   cd Banking-Transaction-Simulation
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv venv

   # Windows
   venv\Scripts\activate

   # macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the application**
   ```bash
   python main.py
   ```

5. **Open in browser**
   ```
   http://127.0.0.1:5000
   ```

---

## Usage

### Transaction Modes

The system supports two transaction processing modes:

| Mode | Processing | Integrity Check | Admin Intervention |
|---|---|---|---|
| **Fast** | Auto-approved after 30 seconds | No integrity hash | No admin review needed |
| **Standard** | Requires manual admin approval | SHA-256 integrity seal | Admin can approve/reject/modify |

### Workflow

```
User sends money ──► Transaction enters Snapshot Queue (PENDING)
                              │
                    ┌─────────┴─────────┐
                    │                   │
               [Fast Mode]        [Standard Mode]
                    │                   │
           Auto-approved          Admin reviews
           after 30 sec          (approve/reject)
                    │                   │
                    └─────────┬─────────┘
                              │
                   Added to Transaction Ledger
                   (hash-chained + Merkle Tree)
```

---

## Application Routes

### Authentication
| Route | Method | Description |
|---|---|---|
| `/` | GET | Redirects to login |
| `/login` | GET, POST | Login page with account ID + PIN |
| `/logout` | GET | Logout and redirect to login |

### User Routes
| Route | Method | Description |
|---|---|---|
| `/dashboard` | GET | User dashboard with balance and recent activity |
| `/send_money` | GET | Money transfer form |
| `/perform_transaction` | POST | Submit a new transaction |
| `/history` | GET | View all past transactions |
| `/limit` | GET, POST | View and update transaction limits |
| `/personal_details` | GET | View personal profile |
| `/update_personal_details` | POST | Update profile information |
| `/verify_integrity` | GET | Verify transaction integrity via Merkle Tree |
| `/download_transcript` | GET | Transcript download page |
| `/generate_transcript` | POST | Generate and download PDF/TXT transcript |
| `/api/check_updates` | GET | API endpoint for real-time balance polling |

### Admin Routes
| Route | Method | Description |
|---|---|---|
| `/admin` | GET | Admin dashboard (queue, accounts, or ledger view) |
| `/admin/process` | POST | Approve or reject a pending transaction |
| `/admin/toggle_lock/<id>` | POST | Lock or unlock a user account |
| `/api/admin/queue` | GET | API endpoint for transaction queue data |

---

## Security Model

### Authentication Flow

```
User enters Account ID + PIN
        │
        ▼
PIN is SHA-256 hashed
        │
        ▼
Hash compared against stored pin_hash
        │
   ┌────┴────┐
   │         │
 Match    Mismatch
   │         │
 Login    Increment failed_attempts
   │         │
   │    ≥ 3 attempts? ──► Lock Account
   │
   ▼
Role check ──► Admin? → Admin Dashboard
             └► User?  → User Dashboard
```

### Transaction Integrity (Standard Mode)

1. **At creation**: `integrity_hash = SHA-256(transaction_amount)`
2. **At approval**: Admin's modified amount is re-hashed and compared
3. **Mismatch** → Transaction is automatically **rolled back** with a security alert

### Merkle Tree Verification

1. All finalized transaction amounts are collected
2. A Merkle Tree is constructed with SHA-256 hashes as leaves
3. The Merkle Root is computed and displayed to users
4. Any tampering with even a single transaction changes the root hash

---

## Screenshots

> *Add screenshots of your application here to showcase the UI.*
>
> Suggested screenshots:
> - Login page
> - User dashboard
> - Send money interface
> - Transaction history
> - Integrity verification page
> - Admin dashboard (queue, accounts, ledger views)

---

## Contributors

| Name | Role |
|---|---|
| Muhammad Mobeen | Developer |
| Muhammad Usman | Developer |
| Muhammad Huzaifa Zaman | Developer / Admin |

---

## License

This project was developed as a university course project for **Data Structures & Algorithms / Information Security / Software Engineering**.

---
