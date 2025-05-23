# IOTA Multisig CLI 🛡️

![logo](/assets/logo.png)

This repository contains tools for managing multisig operations on the IOTA blockchain using a **CLI-first** and **security-by-design** approach.

In the wake of recent incidents like the [SafeWallet frontend compromise](https://x.com/safe/status/1894768522720350673), it's become clear that many multisig platforms expose users to unnecessary risk. Frontends can be tampered with, and users often can't verify what they're signing. This project aims to fix that.

## 🔐 Why CLI-First?

- **No Web Risks:** CLI runs locally with no reliance on a web frontend = no spoofing, injection, or DNS attacks.
- **Full Verifiability:** You can inspect every transaction before it's signed or sent.
- **Local Execution:** Your private keys stay with you. Nothing sensitive leaves your machine.

## 🌐 Web Frontend (Optional UI)

For users who prefer a visual experience, we've also built a **frontend companion tool**:

- 100% client-side - runs in your browser, no backend server.
- Built to work *alongside* the CLI, not replace it.
- Delegates signing to trusted IOTA browser extension wallets (like IOTA wallet).
- Great for preparing, reviewing, and coordinating transactions visually.

👉 **Try it now:** [https://multisig-iota.vercel.app](https://multisig-iota.vercel.app)

---

## Prerequisites

- [IOTA CLI](https://docs.iota.org/developer/getting-started/install-iota) - *Required: Install this [custom fork](https://github.com/arjanjohan/iota/tree/custom-sender-cli)*
- Bash shell environment
- jq (for JSON processing)

__NOTE: To use this tool you must use the [custom fork](https://github.com/arjanjohan/iota/tree/custom-sender-cli) of the IOTA CLI. This custom fork allows to create a transaction with a custom signer (i.e. the multisig wallet). We will create a PR so this feature will be available in the next version of the IOTA CLI.__

## Initial setup

Before you start you must initialze some accounts and a multisig.

## Scripts

### 0. Setup Multisig Wallet

```bash
bash ./scripts/0_setup_multisig.sh
```

Sets up a multisig wallet with multiple signers. The script will guide you through:
- Adding public keys of all signers
- Setting weights for each key
- Setting the threshold for transaction approval

The configuration is saved to a JSON file in the `transactions` directory for future reference.

### 1. Create transaction

```bash
bash ./scripts/1_create_tx.sh [options]
```

Creates a multisig transaction. The script supports four types of transactions:

1. **Publish** - Deploy a new smart contract
   ```bash
   bash ./scripts/1_create_tx.sh -t publish -d <package_directory>
   ```
   Example:
   ```bash
   bash ./scripts/1_create_tx.sh -t publish -d ./my-contract
   ```

2. **Upgrade** - Upgrade an existing smart contract
   ```bash
   bash ./scripts/1_create_tx.sh -t upgrade
   ```

3. **Call** - Call a function on a smart contract
   ```bash
   bash ./scripts/1_create_tx.sh -t call -p <package_address> -m <module_name> -f <function_name> [-a <args>]
   ```
   Example:
   ```bash
   bash ./scripts/1_create_tx.sh -t call -p 0x123... -m counter -f create
   ```

4. **Transfer** - Transfer an object to another account
   ```bash
   bash ./scripts/1_create_tx.sh -t transfer -r <recipient_address> -o <object_id>
   ```
   Example:
   ```bash
   bash ./scripts/1_create_tx.sh -t transfer -r 0x456... -o 0x789...
   ```

If no type is specified, the script will prompt you to select one interactively.

### 2. Approve transaction

```bash
bash ./scripts/2_approve_tx.sh
```

Call this script with the `-- sequence-number <n>` flag to approve/reject a specific transaction. Otherwise it will print an overview of transactions and prompt you to input the sequence number of the one to approve/reject.

### 3. Execute transaction

```bash
bash ./scripts/3_execute_tx.sh
```

Executes the next transaction in the multisig queue, given that is has sufficient approvals.

## Learn more

To learn more about how multisigs work on IOTA check out these resources:
- [IOTA Developer docs](https://docs.iota.org/developer/cryptography/transaction-auth/multisig)

## Next steps
Some further improves will be made after the hackathon:
- *Fix:* Script call fails when a function takes arguments
- *Feature:* Multisig management scripts (change number of signers etc.)
- *PR:* Create a PR for the IOTA CLI, so the custom fork is not needed anymore

