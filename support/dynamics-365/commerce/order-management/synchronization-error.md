---
title: Asynchronous order synchronization issues in Dynamics 365 Commerce
description: Describes common reasons for asynchronous order creation failure in Microsoft Dynamics 365 Commerce and provides troubleshooting steps to help system users and partners understand what went wrong.
author: josaw1 
ms.author: josaw
ms.reviewer: shajain, brstor
ms.date: 05/22/2025
ms.custom: sap:Order management\Issues with order creation in point of sale (POS)
---
# Asynchronous order synchronization issues

This article describes common reasons for asynchronous order creation failure in Microsoft Dynamics 365 Commerce, and provides troubleshooting steps to help system users and partners understand what went wrong.

## Symptoms

Asynchronous orders that are created from Dynamics 365 Commerce e-commerce or point of sale (POS) aren't reflected in Commerce headquarters.

## Troubleshooting steps

Order creation can fail in headquarters for different reasons, depending on the stage at which the order creation process fails. The following troubleshooting steps go through the possible root causes.

### Validate that the transaction related to the asynchronous order has reached headquarters

For e-commerce orders, in headquarters, go to **Retail and Commerce** > **Inquiries and reports** > **Online store transactions**. If you have a confirmation number for the order, filter the transactions by entering the confirmation number in the **Channel reference ID** field. If you don't have the confirmation number, filter the transactions by entering the customer account number.

For POS orders, open the **Store transactions** page, and filter the records by the receipt number or the customer account number. If the transaction isn't found, run the **P-0001** channel transactions job, which syncs transactions from the channels to headquarters. If the **P-0001** job fails, open a support ticket for the job failure. If the **P-0001** job succeeds, but the transactions still don't appear in headquarters, open a support ticket that includes the relevant information.

### Check the synchronization status if the transaction is present in headquarters but isn't linked with a sales order

If the transaction is present in headquarters, but the sales order hasn't been created, open the **Online store transactions** page, and select the **Synchronization status** FastTab. If the **Synchronize orders** job tried to sync this transaction, the **Pending order status** field should show a status of **Succeeded** or **Failed**. If the status is **Succeeded**, the sales order field must be present on this transaction. If the status is **Failed**, you can view the error details in the **Order error details** field on the **Synchronization status** FastTab. If neither of these statuses is shown, no attempt has been made to process the transaction. In this case, you can select **Synchronize order** at the top of the page to run the synchronization job.

Ensure that the **Synchronize orders** job is scheduled to run periodically, so that asynchronous transactions can be created as orders in headquarters.

The following sections provide information about some common errors and their proposed fixes.

#### The "Order error details" field shows the "Number sequence has been exceeded" error message

Number sequences are used to create sales orders in headquarters. If all the numbers that are allowed for a number sequence are exhausted, the system generates this error message. The number sequence that's used to create sales orders can be found at **Account receivables parameters** > **Number sequences** > **Sales order**. To fix this error, fix the existing number sequence, or replace it with a new number sequence.

#### The "Order error details" field shows the "There must be a default payment service to process credit card transactions" error message

To fix this error, confirm that a default payment is defined in headquarters. If no default payment is defined, you must define one. Go to **Accounts receivable** > **Payment setup** > **Payment services**, and ensure that the **Default processor for new credit cards** option is set to **Yes** for one payment service.

#### The "Order error details" field shows an account structure error message

The text of the account structure error message can vary, as the following examples show. However, the errors share a common root cause that's related to the account structure configuration.

- > Posting results for journal batch number 0009656328 Voucher ARP-000959899 1.00 for voucher ARP-000959899 in company usrt will be posted as an overpayment or underpayment

- > Posting results for journal batch number 0009656328 Voucher ARP-000959899 Voucher ARP-000959901 Account structure, for the combination 618160, isn't valid for ledger Corporate Main Account Shared

- > Posting results for journal batch number 0009656328 Voucher ARP-000959899 Voucher ARP-000959901 Reported from company accounts usrt

- > Posting results for journal batch number 0009656328 Voucher ARP-000959899 Posting has been canceled

To fix these errors, review the account structures for accuracy. For more information, see [Configure account structures](/dynamics365/finance/general-ledger/configure-account-structures).

After the error is fixed, select the failed transaction, and then select **Synchronize order** at the top of the page to run the synchronization job.

#### Other types of errors that might require the transaction data to be fixed

To fix other types of errors that might require the transaction data to be fixed, you can [edit and audit transactions](/dynamics365/commerce/edit-order-trans).
