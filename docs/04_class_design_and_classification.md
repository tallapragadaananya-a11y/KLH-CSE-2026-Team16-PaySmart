# 4. Class Design (State and Behaviour) and Classification

| Class | Variables (State) | Methods (Behaviour) |
|-------|-------------------|---------------------|
| User | userId, name, email, phoneNumber, passwordHash, accountStatus {ACTIVE, INACTIVE, BLOCKED} | register(), login(), logout(), updateProfile() |
| Wallet | walletId, userId, balance, currency, walletStatus {ACTIVE, FROZEN, CLOSED} | addMoney(amount, sourceId), withdrawMoney(amount, bankAccountId), checkBalance() |
| BankAccount | accountId, userId, accountNumber, bankName, ifscCode, verificationStatus | linkAccount(), unlinkAccount(), verifyAccount() |
| MoneyTransfer | transferId, senderId, receiverId, amount, transferDate, transferStatus | validateTransfer(), sendMoney(), receiveMoney() |
| Payment | paymentId, userId, merchantId, paymentAmount, paymentMethod {WALLET, UPI, CARD, NET_BANKING}, paymentDate, paymentStatus | validatePayment(), makePayment(), processPayment() |
| Beneficiary | beneficiaryId, ownerUserId, name, phoneNumber, bankAccountNumber, beneficiaryStatus | addBeneficiary(), updateBeneficiary(), deleteBeneficiary() |
| Transaction | transactionId, senderWalletId, receiverWalletId, amount, transactionType {DEPOSIT, WITHDRAWAL, TRANSFER, MERCHANT_PAYMENT, REFUND}, transactionDate, transactionStatus | createTransaction(), processTransaction(), viewHistory() |
| Refund | refundId, transactionId, refundAmount, reason, refundDate, refundStatus | requestRefund(), validateRefundEligibility(), processRefund(), approveRefund() |
| Notification | notificationId, userId, message, sentDate, readStatus | sendNotification(), markAsRead() |
| Admin | adminId, name, email, role {SUPER_ADMIN, COMPLIANCE_OFFICER, SUPPORT_ADMIN}, adminStatus | manageUsers(), blockUser(), viewTransactions(), generateFinancialReport() |

## Classification (Entity / Control / Boundary)
| Component | Class | Classification | Justification |
|-----------|-------|----------------|---------------|
| User Management | User | Entity | Stores persistent identity, credentials and account status |
| Wallet Management | Wallet | Entity | Persistent balance and currency of a user |
| Bank Account Mgmt | BankAccount | Entity / Boundary | Stores bank details; interfaces with external banking APIs for verification |
| Money Transfer | MoneyTransfer | Control | Coordinates validation, sender debit and receiver credit |
| Payment Management | Payment | Control | Merchant validation, payment-method routing, execution |
| Beneficiary Mgmt | Beneficiary | Entity | Saved recipient profiles and verification status |
| Transaction Mgmt | Transaction | Entity / Control | Immutable financial records (Entity); ACID rules (Control) |
| Refund Management | Refund | Control | Multi-step approval and reversal workflow |
| Notification Mgmt | Notification | Boundary | Outbound communication (SMS, push, e-mail) |
| Admin Management | Admin | Boundary / Control | Administrative interface (Boundary); enforces blocks (Control) |

## Design decisions beyond the first design sheet
- `BankAccount.Status` keeps the four values from the design sheet; a new account starts as LINKED and `verifyAccount()` moves it to VERIFIED.
- `Notification.Status` creates a message as SENT; `markAsRead()` sets READ.
- `Transaction` additionally stores `payerUserId` so history and refunds can be traced even when the money source is external (UPI/Card/NetBanking).
- Merchants have a settlement wallet created on first payment, so merchant payments obey the same conservation-of-money rule.
