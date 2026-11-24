# 2. Classes, Values, States and Behaviours

| Component | Class | Values | States | Behaviours |
|-----------|-------|--------|--------|------------|
| User | `User` | userId, name, email, phoneNumber, password | Active, Inactive, Blocked | register(), login(), logout(), updateProfile() |
| Wallet | `Wallet` | walletId, balance, currency, userId | Active, Frozen, Closed | addMoney(), withdrawMoney(), checkBalance() |
| Bank Account | `BankAccount` | accountId, accountNumber, bankName, IFSCCode | Linked, Unlinked, Verified, Unverified | linkAccount(), unlinkAccount(), verifyAccount() |
| Money Transfer | `MoneyTransfer` | transferId, senderId, receiverId, amount, date | Pending, Processing, Successful, Failed | sendMoney(), receiveMoney(), validateTransfer() |
| Payment | `Payment` | paymentId, paymentAmount, paymentMethod, paymentDate, merchantId | Pending, Successful, Failed | makePayment(), validatePayment(), processPayment() |
| Beneficiary | `Beneficiary` | beneficiaryId, name, phoneNumber, bankAccount | Added, Verified, Blocked | addBeneficiary(), updateBeneficiary(), deleteBeneficiary() |
| Transaction | `Transaction` | transactionId, amount, transactionType, transactionDate | Pending, Successful, Failed, Cancelled | createTransaction(), processTransaction(), viewHistory() |
| Refund | `Refund` | refundId, refundAmount, refundDate, transactionId | Requested, Processing, Completed, Rejected | requestRefund(), processRefund(), approveRefund() |
| Notification | `Notification` | notificationId, message, date, userId | Sent, Read, Unread | sendNotification(), markAsRead() |
| Admin | `Admin` | adminId, name, email, role | Active, Inactive, Suspended | manageUsers(), blockUser(), viewTransactions() |

## Where each behaviour lives in code
Entity classes hold their own state and state-local rules (e.g. `Wallet.addMoney`, `BankAccount.verifyAccount`, `Refund.moveTo`).
Behaviours that coordinate several objects (register/login, sendMoney, makePayment, requestRefund/approveRefund, blockUser, viewHistory, sendNotification, generateFinancialReport) are in `PaySmartService`, which keeps entities independent of each other (low coupling).
