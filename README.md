---
description: Initiate transactions while "Unbolted"
---

# Offline 1.0

## **Overview**

When a Bolt P2PE terminal is unable to establish a good internet connection via ethernet, Offline mode can provide a method to capture, and temporarily store transactions until the batch can be pushed back to Bolt for authorization and capture.

When in offline mode, the terminal will be unreachable from the Bolt server, and the POS. However, merchant users will be able to initiate transactions directly within the Bolt Offline terminal interface.

### **How it works and risks**

The Bolt terminal firmware locally stores P2PE encrypted transaction details entered by the merchant. Once an internet connection is reestablished, the terminal forwards its batch \(recent offline orders\) to the CardConnect gateway through Bolt, at which point the authorization\(s\) are submitted.

Upon completion of the batch upload and receipt of authorization response, the terminal will display a batch ID that can be used to reference the offline batch during reconciliation.

{% hint style="danger" %}
It is extremely important for merchants to understand the business risks of accepting transactions in offline mode. Production usage will require merchant/partner agreement to be provided.
{% endhint %}

By delaying the authorization of the cardholder’s payment, merchants are assuming the risk that a future authorization may be declined. CardConnect shall not be held liable in the case of declined authorizations obtained while in Bolt P2PE offline mode.

The following are velocity/throttling safeguards provided through CardConnect’s Terminal Management System \(TMS\). These can be set at the merchant or partner level.

* Offline: On/Off\(Default\)
* Offline Max Transaction Amount \(only applicable if Online = On\)
* Offline Max Transaction Count \(only applicable if Online = On\)

{% hint style="info" %}
Key Points

* Transactions are initiated directly within the Bolt terminal interface by the merchant user. 
  * Cardholders/Consumers should not interact with the terminal while in offline mode.
* The software application \(POS\) will be unable to communicate with the terminal while in offline mode.
{% endhint %}

## **Configuration \(TMS\)**

* Terminals must be running firmware version 1.6.2.11 or higher
* Terminal Merchant Properties: 
  * Offline: On
  * Offline Max Transaction Amount \(optional\)
  * Offline Max Transaction Count \(optional\)
  * Offline PIN \(optional\)
  * Offline Signature \(optional\)

## **User Flow**

If the terminal has been configured for Offline, and it becomes “UNBOLTED,” the user will be guided through the following sequence.

* Prompt to **enter transaction amount**
  * If \(amount &lt;= Offline Max Transaction Amount\)
    * Alert: Amount Exceeds Offline Limit
    * Return to initial transaction amount prompt
* Prompt to **enter a unique order ID**
  * This Order ID will be associated with the authorization record
  * Should be derived from the POS and used during [batch reconciliation](offline.md#batch-reconciliation)
* Prompt for **Card Present or Not**
  * If \(Card Present\)
    * Prompt to Swipe/Insert
  * If \(Card Not Present\)
    * Prompt to manually enter
* Prompt to **capture signature if enabled.**  

### Batch Reconciliation

An endpoint on the CardConnect Gateway will be exposed to identify and retrieve details of any recent offline batch activity. Specific details to be provided very soon.

