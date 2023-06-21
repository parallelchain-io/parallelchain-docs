---
tags:
  - pchain-client
  - Staking
  - Network Transaction
---

The staking process consists of several Staking Commands. Each command usually works with each other to serve a particular purpose. It is recommended to read paragraph [How does staking work](../concepts/staking/how_does_staking_work.md#how-does-staking-work) before trying out the commands which are described on this page.

This page guides you to create staking transactions by using the CLI. The CLI creates a JSON file which can be used to submit the staking transaction in the same way as [submitting transfer transaction](./transfer.md#submitting-transaction).

## Creating Deposit
---

First, before you stake, you have to lock up (stake) some balance tied to an operator to prepare for the stake. This step is done by the CLI subcommand `deposit create`, which reduces the balance of your account.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deposit create \
    --operator <OPERATOR_ADDRESS> \
    --balance <BALANCE> \
    --auto-stake-reward
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    deposit create \
    --operator <OPERATOR_ADDRESS> \
    --balance <BALANCE> \
    --auto-stake-reward
    ```
The last flag for `auto-stake-reward` is optional. By default, it is false. It indicates whether your rewards should be staked to the Pool in each epoch.

After you create a deposit, you can do follows:

- Top-up the amount of the deposit
- Update deposit settings (i.e. toggling `auto-stake-reward`)
- Stake deposit to the pool
- Withdraw the deposit

!!! Important Notes
    Now you have just created the Deposit to a Pool, but you still have no stake in the pool. At this stage, you will **NOT** have the rewards, because your reward amount is calculated by the amount that you staked, not the Deposit you created.

### Topping up Deposit

After creating a Deposit in a Pool, use the CLI subcommands `deposit top-up` if you want to put more in the same Pool. 

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deposit top-up \
    --operator <OPERATOR_ADDRESS> \
    --amount <AMOUNT>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    deposit top-up \
    --operator <OPERATOR_ADDRESS> \
    --amount <AMOUNT>
    ```

### Toggling `auto-stake-reward`
---

You specified the flag `auto-stake-reward` when you created the Deposit. You can change the value of this flag by submitting another transaction using the CLI subcommand `deposit update-settings`.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deposit update-settings \
    --operator <OPERATOR_ADDRESS> \
    --auto-stake-reward
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    deposit update-settings \
    --operator <OPERATOR_ADDRESS> \
    --auto-stake-reward
    ```

## Staking and Unstaking
---

After you create a Deposit to a Pool, you should now stake some amount of it to the pool with the CLI subcommand `stake stake`.  

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    stake stake \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    stake stake \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```

The `max-amount` specifies the maximum amount of Deposits that are going to stake. After submitting the transaction, the resulting staked amount is not necessary to be as same as the `max-amount`.

You can also unstake your stake on with the CLI subcommand `stake unstake`.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    stake unstake \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    stake unstake \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```

### Creating Transaction with Multiple Staking Commands

Staking in a pool requires two steps: `Create Deposit` and `Stake Deposit`. But it does not mean that there must be two corresponding transactions to be made. You can create a single transaction with two staking commands. For example, you can do both `Create Deposit` and `Stake Deposit` in a transaction by the CLI command `append`.

Suppose you created the transaction file (i.e. `tx.json`) from the step [Creating Deposit](#creating-deposit).

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction append \
    --file <PATH_TO_TRANSACTION_FILE> \
    stake stake \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction append \
    --file <PATH_TO_TRANSACTION_FILE> \
    stake stake \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```

The transaction file will then be modified. Then, you can submit it as a single transaction.

## Withdrawing Deposit
---

Your Deposit can be increased due to reward distribution in each epoch. If you want to pull out the Deposit from the Pool to the balance of your account, make use of the CLI subcommand `deposit withdraw`.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deposit withdraw \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    deposit withdraw \
    --operator <OPERATOR_ADDRESS> \
    --max-amount <MAX_AMOUNT>
    ```

The balance in your account will be increased for the Deposit that you withdrew from the Pool.

!!! Tips
    If you withdraw the Deposit without unstaking (assuming you already staked), you may not be able to withdraw all deposited tokens because you cannot withdraw the staked Deposit in the current Pool. In this case, you have to unstake first and wait for the next two epochs before you withdraw the Deposit.
