---
tags:
- Xperience extension
- xpll
---

Sending and receiving assets with digital wallets is a keystone of any blockchain’s decentralized infrastructure. Follow the steps below to send and receive tokens with your ParallelChain account.

!!! note

    Before you can perform the following steps, you need to have created and logged in to your ParallelChain account. If you are not sure how to do so, refer to [Xperience Browser Extension Tutorials: Creating and Managing Your Account](./create_account.md). 

## Sending Tokens
---
![check transfer](../../img/tokens/1_Send%20Tokens.svg){ width=40%  style="display: block; margin: 0 auto" } 

### Step 1: Fill in Transaction Details
1. Click **SEND**.
2. Select the token to send by clicking the down arrow in the **Token** field.
3. Enter the address that you want to send tokens to in the **To Address** field.

    !!! note 
        If you are transferring tokens between your own accounts, you can access your different accounts by clicking the down arrow next to your account name and address. You can copy the address of the receiving account by clicking the copy icon beside its address. 
4. Enter the number of tokens you would like to send in the **Send** field. The amount must not be a negative number, and it cannot be more than the number of tokens you currently have.
5. Click **NEXT**.

### Step 2: Confirm Your Transaction
1. You will see the following additional fields:
    - **Nonce** - will be automatically filled in for you.
    - **Max Base Fee per Gas** - will be automatically filled in for you. The minimum fee is 8 XPLL.
    - **Priority Fee per Gas** - determines the priority of your transaction. The minimum fee is 0 XPLL.
    - **Gas Limit** - will be automatically filled in for you.
    - **Estimated Gas Fee** - the approximate gas fee that will be charged for your transaction.
2. If you are satisfied with the fields, click **NEXT**.

3. Preview the summary of the transaction. Click **CONFIRM** to confirm or **CANCEL** to make edits.

4. The transaction status will show **PENDING**. Click **CLOSE**.

    ![check transfer](../../img/tokens/2_Check%20Status%20of%20Transaction.svg){ width=40%  style="display: block; margin: 0 auto" } 

5. You can check the status of your transaction in the **Transactions** tab. It is labeled **TRANSFER**. When the transaction is validated by the network, the transaction status will show **SUCCESS**. Your tokens have now been sent.  

## Receiving XPLL Tokens

### Step 1: Share Your Wallet Address
1. Click the copy icon next to your wallet address to copy it.
2. Share your wallet address with the sender through a secure text messaging application.
3. The sender should use your wallet address and follow the steps in **Sending Tokens** to send you the tokens.

### Step 2: Receive Your Tokens
1. Wait for the sender to confirm the transaction. Be patient as the transaction takes time to be validated by the network.
2. Once the transaction is confirmed, your wallet balance will be automatically updated.

## FAQ
### What tokens can I send or receive?
You can send or receive XPLL using either **Xperience Browser Extension** or **ParallelChain Explorer**. However, you can only send or receive **PRFC1 tokens** using **Xperience Browser Extension** for now.

### I have not received my tokens. Where can I check the transaction status?
To check the transaction status of the token transfer, you can use [ParallelChain Explorer](https://explorer.parallelchain.io/explorer?network=Mainnet).

Click on your wallet address in **Xperience Browser Extension**. This will open **ParallelChain Explorer**. Enter your wallet address in the search field. Under the **Transactions** section, **RECEIVED** tab, you can see your transactions for receiving tokens. 

### Can I cancel or refund transactions? 

No, once a transaction request has been made, it cannot be canceled. 

### Where can I seek support or report bugs? 

You can visit ParallelChain’s [Discord](https://discord.gg/parallelchainofficial) for community help. If you cannot resolve your issue there, you can write to [walletsupport@parallelchain.io](mailto:walletsupport@parallelchain.io). 