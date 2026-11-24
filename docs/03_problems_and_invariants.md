# 3. Problems and Sentence Invariants

## Problems that may arise
1. **Race conditions & double spending (Wallet, Money Transfer)** – two simultaneous transfers read the same balance and drive it negative.
2. **Partial transaction failures (Money Transfer, Bank Account)** – sender debited, receiver not credited because of a timeout.
3. **Duplicate transactions / idempotency (Payment, Transfer)** – double click on "Pay" processes the payment twice.
4. **Invalid or duplicate refunds (Refund)** – multiple refunds for one transaction, or refund above the original amount.
5. **Fraud & unauthorized access (User, Admin)** – account takeover, brute-force login, rapid high-value transfers to unverified beneficiaries.
6. **Tight coupling of payment methods (Payment)** – hard-coded if/else for UPI/Card/NetBanking violates the Open/Closed Principle.

## Sentence invariants
- **User:** unique userId, email and phoneNumber; a Blocked user cannot start any financial transaction.
- **Wallet:** balance >= 0 always; every active wallet belongs to exactly one valid User.
- **Bank Account:** cannot be used to add/withdraw funds unless Verified.
- **Money Transfer:** senderId != receiverId and amount > 0.
- **Payment:** becomes Successful only if the wallet balance (or linked source) >= paymentAmount.
- **Beneficiary:** has valid routing details (phone or bank account) and is not Blocked during a transfer.
- **Transaction (conservation of money):** for a successful wallet-to-wallet transaction, amount debited == amount credited; every transaction leaves an immutable log.
- **Refund:** only for a Successful (or disputed) transaction; refundAmount <= original amount.
- **Notification:** has a valid userId, a non-empty message and a valid timestamp.
- **Admin:** only an Active admin with valid role privileges can Block a user or freeze a wallet.

## Invariant / problem → code → test
| Rule | Enforced in | Test (src/test/.../PaySmartTests.java) |
|------|-------------|---------------------------------------|
| Unique email/phone | `PaySmartService.register` | User: duplicate e-mail / phone rejected |
| Blocked user cannot transact | `PaySmartService.activeUser` | User: blocked user cannot transact |
| balance >= 0 | `Wallet.assertCanSpend/debit` | Wallet: overdraft rejected |
| Verified bank only | `Wallet.addMoney/withdrawMoney` | Bank: unverified account cannot add money |
| sender != receiver, amount > 0 | `MoneyTransfer.validateTransfer` | Transfer: self-transfer rejected |
| Conservation of money | `Wallet.transfer` | Conservation: sender -400, receiver +400 |
| Immutable log | `Transaction.moveFromPending` | Transaction: finished transaction cannot change state |
| Refund rules | `validateRefundEligibility` | Refund: amount above original rejected |
| Admin privileges | `Admin.assertCanBlock` | Admin: support role cannot block |
| Problem 1 | `Wallet` locks | 50 concurrent transfers → exactly 10 succeed |
| Problem 3 | `idempotent()` | same idempotency key returns same transaction |
| Problem 4 | `activeRefundByTxn` | second refund for same transaction rejected |
| Problem 5 | `login()` lock-out | 5 failed logins block the account |
| Problem 6 | `PaymentMethodHandler` | handler swapped without editing service code |
