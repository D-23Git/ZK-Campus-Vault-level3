# Product Proposal: ZK Campus Vault
**Academic Eligibility Gate & Confidential GPA Verification**

---

## 💡 Problem Statement
Academic record verification for college admissions, scholarship applications, and job recruitment currently requires students to submit their raw transcript payloads. This process leaks sensitive personal data:
* It discloses the exact GPA (e.g., revealing a 3.85 GPA when only a 3.50 threshold was required).
* It exposes unique student roll IDs and university registration codes.
* It raises privacy and GDPR compliance issues by storing raw PII on third-party recruitment systems.

## 🛡️ Zero-Knowledge Solution: "Proven Without Being Shown"
ZK Campus Vault uses Midnight Network's native Compact smart contracts to verify academic credentials selectively and privately.
By executing local **Groth16 ZK proof compilation** directly inside the user's browser, a student can prove to recruiters that:
1. Their GPA meets kiva exceeds the required threshold limit (e.g. `GPA >= 3.50`).
2. They are active, enrolled students at a recognized university registrar.

The actual GPA value and student Roll ID remain completely private in local client RAM. The Midnight ledger only records the on-chain commitment hash and the validity boolean of the proof.

---

## ⚙️ Architecture & Data Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    PRIVATE (never on-chain)                  │
│       actualGPA | Roll ID | Secret Salt Blinding Factor      │
└──────────────────────────┬──────────────────────────────────┘
                           │ Local ZK Witness
                    ┌──────▼──────┐
                    │  ZK Circuit  │  ← campus_vault.compact
                    │  (Groth16)  │  asserts: (actualGPA >= limit)
                    └──────┬──────┘
                           │ Cryptographic Proof JSON
┌──────────────────────────▼──────────────────────────────────┐
│                    PUBLIC (Preprod Ledger State)             │
│   commitmentHash | verifiedResult: passed (true/false)     │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗺️ Future Roadmap & Scope
1. **Multi-Criteria Split Eligibility:** Extend the circuit logic to prove GPA range limits (e.g., `3.50 <= GPA <= 4.00`) for custom tiered Split scholarship splits.
2. **Decentralized Issuer DAO:** Governance smart contracts enabling accredited university registrars to register and revoke credential signatures cooperatively.
3. **Mainnet Transition:** Migration of compact circuits from Preprod testnet to Midnight Mainnet after secure third-party audit validation.

---
*Built as a submission for Level 3 — First Quarter Moon (INTO the Midnight Bootcamp, 2026).*
