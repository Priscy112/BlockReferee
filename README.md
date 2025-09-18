# BlockReferee -  Decentralized Peer Review System on Stacks

This Clarity smart contract implements a decentralized, trustless peer review platform for academic papers, leveraging the Stacks blockchain. It enables authors to submit papers and incentivizes reviewers through staking and rewards for quality reviews. The contract includes robust validation and error handling to ensure transparency, fairness, and accountability.

---

## 🚀 Features

* **Paper Submission**

  * Authors can submit papers via an IPFS hash and unique paper ID.
  * Each paper is initialized with metadata including status, author, and timestamp.
  * Prevents duplicate paper submissions and ensures non-empty IPFS hashes.

* **Reviewer Registration**

  * Users can register as reviewers by staking a minimum amount of STX.
  * Each reviewer gets initialized with reputation, stake, and active status.
  * Registration ensures sufficient balance and avoids double registration.

* **On-chain Metadata**

  * Tracks all submitted papers and their review statistics.
  * Maintains a registry of reviewers, including their reputation and status.
  * Keeps a history of reviews tied to both paper ID and reviewer address.

* **Security & Validation**

  * Includes extensive validation and assertions (e.g., empty hash, duplicate paper IDs, insufficient balance).
  * Prevents unauthorized actions using custom error constants.
  * Stores the contract owner for future administrative controls.

---

## 🗃️ Data Structures

### Constants

| Name                   | Description                             |
| ---------------------- | --------------------------------------- |
| `ERR-NOT-AUTHORIZED`   | Unauthorized access error               |
| `ERR-PAPER-NOT-FOUND`  | Paper ID not found                      |
| `ERR-ALREADY-REVIEWED` | Reviewer has already reviewed the paper |
| ...                    | *(See full list in contract code)*      |

### Data Variables

| Variable         | Type        | Description                      |
| ---------------- | ----------- | -------------------------------- |
| `min-stake`      | `uint`      | Minimum STX to become a reviewer |
| `review-reward`  | `uint`      | STX reward per review            |
| `contract-owner` | `principal` | Owner/admin of the contract      |

### Maps

* **Papers**

  * Key: `{ paper-id: uint }`
  * Value: `{ author, ipfs-hash, status, review-count, total-score, timestamp }`

* **Reviews**

  * Key: `{ paper-id, reviewer }`
  * Value: `{ score, comment-hash, timestamp, status }`

* **Reviewers**

  * Key: `{ reviewer }`
  * Value: `{ stake, review-count, reputation, status }`

---

## 🛠️ Functions

### `submit-paper (ipfs-hash, paper-id)`

* Submits a new paper with unique ID and IPFS hash.
* Fails if the hash is empty, ID is invalid, or paper already exists.

### `register-reviewer`

* Allows a user to become a reviewer by staking STX.
* Fails if already registered or balance is insufficient.

### `(is-contract-owner)`

* Private function to verify if `tx-sender` is the contract owner.

---

## ✅ Error Handling

The contract defines constants for all possible error states (e.g., unauthorized access, invalid score, duplicate submission). These make the contract robust and help frontends provide clear user feedback.

---

## 🔐 Security & Design Notes

* All funds are securely transferred using `stx-transfer?` to the contract.
* Validations prevent misuse, re-registration, and double submissions.
* Data structures are optimized for clarity and auditability.

---

## 🧪 Deployment Notes

* Written in Clarity for the Stacks blockchain.
* Ensure `min-stake` and `review-reward` values are set correctly upon deployment.
* Admin functions can be added using the `contract-owner` logic as needed.

---

## 📜 License

MIT or similar open-source license recommended.

---
