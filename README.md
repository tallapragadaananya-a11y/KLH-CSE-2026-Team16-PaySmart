# PaySmart – Digital Wallet and Financial Transaction Platform

**Repository:** `KLH-CSE-2026-27-16-PaySmart`  |  **Course:** Advanced Object-Oriented Programming (AOOP) – PBL  |  **Team ID:** 16

## Team members
| S.No | Name | Roll / ID Number |
|------|------|------------------|
| 1 | T.S.S.S. Ananya | 25200030039 |
| 2 | Devika Kuntur | 2520030329 |
| 3 | Rushvini S | 2520030542 |

**Supervisor / Guide:** K. Sri Rama Murthy  |  **Course Coordinator:** In progress

## Abstract
PaySmart is a Digital Wallet and Financial Transaction Platform designed to provide users with a secure, convenient and efficient way to manage their digital money. The main objective of this project is to simplify financial activities such as adding money, checking wallet balance, transferring money, making payments and viewing transaction history through a single platform.

The system allows users to register, log in and create a digital wallet linked to their account. Users can link and verify bank accounts, add or withdraw money, transfer funds to other PaySmart users and save beneficiary details for future transactions. The platform also maintains complete transaction records and provides notifications about successful payments, failed transactions and refunds. An admin module is included to manage users, monitor transactions and block suspicious accounts.

The system follows object-oriented programming principles by organizing functionalities into different components and identifying their classes, attributes, states and behaviours. It also focuses on maintaining important financial rules, such as preventing negative wallet balances, avoiding duplicate transactions, validating transfers and ensuring accurate refunds. Security mechanisms and transaction validation are used to reduce unauthorized access, fraud and financial inconsistencies. Overall, PaySmart aims to provide a reliable, user-friendly and secure digital financial platform that makes everyday money management and transactions easier and more efficient.

## Repository structure
```
/src        Java source (com.paysmart.*) and tests (src/test)
/docs       Design documents: components, classes, invariants, problems, class diagram
/data       Sample seed data used by the demo / testing
/results    Saved demo output and test results
/reports    Review reports (Review-1, ...)
README.md
```

## Components → Classes
| # | Component | Class | Type |
|---|-----------|-------|------|
| 1 | User Management | `User` | Entity |
| 2 | Wallet Management | `Wallet` | Entity |
| 3 | Bank Account Management | `BankAccount` | Entity / Boundary |
| 4 | Money Transfer | `MoneyTransfer` | Control |
| 5 | Payment Management | `Payment` (+ `PaymentMethodHandler` strategies) | Control |
| 6 | Beneficiary Management | `Beneficiary` | Entity |
| 7 | Transaction Management | `Transaction` | Entity / Control |
| 8 | Refund Management | `Refund` | Control |
| 9 | Notification Management | `Notification` | Boundary |
| 10 | Admin Management | `Admin` | Boundary / Control |

`PaySmartService` is the facade that coordinates all components and enforces the invariants.

## How the six identified problems are solved
| Problem | Solution in code |
|---------|------------------|
| Race conditions / double spending | Per-wallet `ReentrantLock`; `Wallet.transfer` locks both wallets in id order (deadlock-free) |
| Partial failure / inconsistency | Pre-checks while both locks are held, so debit + credit cannot be split; failed attempts are logged as `FAILED` |
| Duplicate transactions | Idempotency key per (user, key) – a retry returns the first result |
| Invalid / duplicate refunds | `validateRefundEligibility` + one active refund per transaction + amount ≤ original |
| Fraud / unauthorized access | 5 failed logins → `BLOCKED`; transfer limit; admin-only blocking/freezing with role checks |
| Tight coupling of payment methods | Strategy pattern (`PaymentMethodHandler`); new method = new class, no `if/else` edits (Open/Closed) |

## Setup and execution
Requires **JDK 11+** (tested on JDK 21). No external libraries.

```bash
# Linux / macOS
./run.sh            # compile + run demo + run tests

# Windows
run.bat
```
Manual:
```bash
mkdir -p out
javac -d out $(find src -name "*.java")
java -cp out com.paysmart.Main            # demo
java -cp out com.paysmart.PaySmartTests   # 30 tests
```

## Current phase status
**Review – I:** Problem analysis, component design, class design, invariants, working core implementation and tests – **completed**.
Next: persistent storage (database), REST/UI layer, additional tests.

## Git tags
`review-1`, `review-2`, `final` (one tag per phase deliverable).
