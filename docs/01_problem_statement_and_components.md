# 1. Problem Statement and Components

PaySmart is a digital wallet and financial transaction platform. Users store money digitally, send money to other users, make payments and view transaction history.

## Basic flow
1. The user registers and logs in.
2. After login the user gets a digital wallet.
3. The user can add money through a supported payment method, check the balance, transfer money and make payments.
4. For every transaction the system checks the balance, validates the details and processes the transaction.
5. The wallet balance is updated and the transaction details are stored.

## Components
| # | Component | Responsibility |
|---|-----------|----------------|
| 1 | User Management | Registration, login, profile |
| 2 | Wallet Management | Add / withdraw money, check balance |
| 3 | Bank Account Management | Link, unlink, verify bank accounts |
| 4 | Money Transfer | Send / receive money between PaySmart users |
| 5 | Payment Management | Pay for products and services from the wallet |
| 6 | Beneficiary Management | Save and manage receiver details |
| 7 | Transaction Management | Amount, date, sender, receiver and status of every transaction |
| 8 | Refund Management | Refund requests for eligible failed/cancelled payments, status tracking |
| 9 | Notification Management | Alerts for payments, failures, transfers, refunds |
| 10 | Admin Management | Manage users, monitor transactions, block suspicious accounts |
