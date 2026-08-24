# Report Improvement Plan
### Project: *Privacy-Preserving Secure Cloud Storage Using Hybrid Encryption and Attribute-Based Access Control*
Prepared in response to the mentor's review feedback.

---

## 0. Quick assessment of the current report
The report is well-structured (Problem → Literature → Objectives → Scope → Novelty → Feasibility) and the technical direction (hybrid AES-256-GCM + RSA-OAEP, CP-ABE key wrapping, ABAC enforcement, versioned revocation) is sound. The main weaknesses the mentor is pointing at are:
- References are **all foundational/old** (2005–2013). There is nothing showing awareness of the *current* research frontier.
- No **architecture diagram** — a reviewer cannot see how the pieces connect.
- The literature survey is a bare table of *contributions* only — no critical reading (achievements **and** drawbacks).
- A few sentences are dense/run-on.
- The **Operational & Schedule Feasibility** section is just a prose list of week numbers with no real planning content.
- No **reference list / links** at the end.

The six items below map one-to-one to the mentor's points.

---

## 1. Add latest research papers (2021 onwards)
Keep the 8 foundational works (Sahai–Waters, Goyal, BSW07, Waters, Lewko–Waters, NIST 800-162, Green, Akinyele) as the *historical basis*, but add a **"Recent Advances (2021–present)"** subsection so the research gap is framed against current work. Suggested, directly relevant, recent references:

| # | Reference (year) | Why cite it |
|---|---|---|
| R1 | *Flexible revocation in CP-ABE with verifiable ciphertext delegation*, Multimedia Tools & Appl., 2022 — [link](https://link.springer.com/article/10.1007/s11042-022-13537-0) | Directly supports your **revocation** gap (uses a user binary tree). |
| R2 | *Puncturable CP-ABE for efficient and flexible user revocation*, Science China Inf. Sci., 2023 — [link](https://link.springer.com/article/10.1007/s11432-022-3585-9) | Modern alternative to your attribute-versioning revocation; proxy-assisted decryption. |
| R3 | *A cloud-based enhanced CP-ABE framework for efficient user and attribute-level revocation*, IJCA, 2023 — [link](https://www.tandfonline.com/doi/full/10.1080/1206212X.2023.2250149) | Recent survey/scheme on user- and attribute-level revocation. |
| R4 | *A revocable ABE EHR sharing scheme with multiple authorities in blockchain (MA-RABE)*, Peer-to-Peer Netw. Appl., 2022 — [link](https://link.springer.com/doi/10.1007/s12083-022-01387-4) | Multi-authority + revocation + healthcare — matches your out-of-scope future work. |
| R5 | *Blockchain-Based Multiple Authorities ABE for EHR Access Control*, Applied Sciences (MDPI), 2022 — [link](https://www.mdpi.com/2076-3417/12/21/10812) | Policy-hiding + outsourced-decryption verification. |
| R6 | *Access control based on blockchain and attribute-based searchable encryption in cloud*, J. Cloud Computing, 2023 — [link](https://link.springer.com/article/10.1186/s13677-023-00444-4) | Directly supports your **searchable-encryption** gap. |
| R7 | *A structured review of lattice-based ABE methods for post-quantum security*, Springer, 2026 — [link](https://link.springer.com/10.1007/s10791-026-09965-3) | Anchors your **post-quantum** gap (LWE / Ring-LWE ABE). |
| R8 | *Blockchain-Enabled Lattice-Based Attribute-Based Searchable Encryption with Instant Revocation (BL-ABSE)*, Electronics (MDPI), 2025 — [link](https://www.mdpi.com/2079-9292/15/11/2471) | Combines *all four* of your themes (PQ + ABE + revocation + audit) — cite as the closest state-of-the-art and position your work against it. |
| R9 | *Performance & Storage Analysis of CRYSTALS-Kyber (ML-KEM) vs RSA/ECC*, arXiv, 2025 — [link](https://arxiv.org/html/2508.01694) | Data to justify ML-KEM as the post-quantum path. (ML-KEM standardized as NIST FIPS 203, 2024.) |
| R10 | *RSA and AES Based Hybrid Encryption Technique for Cloud*, 2023 — [link](https://www.researchgate.net/publication/375145532) | Recent comparator for your hybrid AES+RSA core. |
| R11 | *A privacy-preserving cloud storage framework with hybrid encryption, homomorphic keyword search, and blockchain integrity*, Scientific Reports (Nature), 2026 — [link](https://www.nature.com/articles/s41598-026-48705-x) | Very close in scope — use it to sharpen your novelty statement. |
| R12 | *A hybrid ECC-AES encryption framework for secure cloud data protection*, Scientific Reports (Nature), 2025 — [link](https://www.nature.com/articles/s41598-025-01315-5) | Performance baseline for hybrid-encryption latency. |

> Content from the sources above was rephrased for compliance with licensing restrictions. Verify each DOI/venue before final submission.

**Action:** add ~6–10 of these, and rewrite the *Identified Research Gaps* section so each gap cites a 2021+ paper that leaves it open.

---

## 2. Rewrite unclear sentences
Concrete before → after examples from the current draft:

**Core Problem Statement (I.B)**
- *Before:* "organisations struggle to have strong, flexible access control, efficient key management, quick revocation, post-quantum security, and searchable encrypted data all at the same time without compromising security, performance, or ease of use."
- *After:* "No existing cloud-storage system simultaneously provides fine-grained access control, efficient key management, near-instant revocation, post-quantum resistance, and search over encrypted data without sacrificing performance or usability. This project targets that combined gap."

**Novelty — defence-in-depth (V.A)**
- *Before:* "ABAC gives instant revocation that can, in principle, be bypassed; ABE gives revocation that cannot be bypassed but is cryptographically expensive."
- *After:* "ABAC enforces revocation instantly at the access layer but relies on that layer being trusted; CP-ABE enforces revocation cryptographically (it cannot be bypassed even by the server) but is computationally expensive. We combine the two so that ABAC handles the common fast path while ABE guarantees confidentiality if the enforcement layer is compromised."

**Objective O7**
- *Before:* "demonstrate at least one genuinely novel angle that moves one of the documented research gaps forward."
- *After:* "produce at least one measurable contribution — the three-tier key-wrapping model and its revocation cost model — that advances the real-time-revocation gap identified in Section II."

**Action:** do one editing pass for run-on sentences; prefer one idea per sentence. I can do this pass across the whole document on request.

---

## 3. Add the proposed model architecture
Insert a new **Section (e.g., "VII. Proposed System Architecture")** with a figure plus a numbered data-flow. Suggested layered architecture:

```
                    ┌─────────────────────────────────────────────┐
                    │              CLIENT (data owner / user)       │
                    │  • AES-256-GCM file encryption                │
                    │  • DEK generation                             │
                    │  • RSA-OAEP device key  • RSA-PSS signing     │
                    │  • Final CP-ABE decryption step               │
                    └───────────────┬───────────────▲──────────────┘
                        upload/DEK   │               │ transformed CT
                                     ▼               │
   ┌──────────────────┐   ┌───────────────────────────────────────┐
   │ Attribute        │   │        BACKEND (Flask REST API)         │
   │ Authority (AA)   │──▶│  • JWT auth                             │
   │ • CP-ABE Setup   │   │  • ABAC engine: PDP · PEP · PIP · PAP   │
   │ • Attribute keys │   │  • CP-ABE key-wrap / outsourced decrypt │
   │ • Key versioning │   │  • Revocation: attr-versioning + re-wrap│
   └──────────────────┘   └──────┬───────────────────┬─────────────┘
                                  │                   │
                        metadata  ▼                   ▼  ciphertext objects
                        ┌──────────────────┐  ┌───────────────────────┐
                        │ MongoDB          │  │ Object store           │
                        │ (policies, keys, │  │ (AWS S3 / Azure Blob)  │
                        │  versions, audit)│  │  presigned URLs        │
                        └──────────────────┘  └───────────────────────┘
```

**Upload flow (owner):** ① generate random DEK → ② AES-256-GCM encrypt file → ③ wrap DEK under a CP-ABE access policy → ④ optionally wrap under recipient RSA public key (device binding) → ⑤ RSA-PSS sign + SHA-256 hash → ⑥ store ciphertext in S3/Azure, metadata + wrapped keys + policy + version in MongoDB.

**Download flow (user):** ① authenticate (JWT) → ② ABAC PDP evaluates subject/object/action/environment attributes → ③ if allowed, server performs outsourced CP-ABE transformation using the user's transform key → ④ client completes the light final decryption to recover the DEK → ⑤ AES-GCM decrypt + verify tag/signature.

**Revocation flow:** bump the attribute/key version → lazily re-wrap only the 32-byte DEK on next access (no file re-encryption) → ABAC denies instantly in parallel.

Draw this in draw.io / PowerPoint and export as PNG for the Word doc. **Add a second diagram** showing the three-tier key hierarchy: `File → AES DEK → CP-ABE wrap → per-user RSA wrap`.

---

## 4. Literature survey: add 4–5 line paragraphs (achievements + drawbacks)
Replace the "contribution-only" table with a short critical paragraph per work (keep the table as a summary if you like). Template:

> *[Authors, venue, year]* proposed **[what]**. Its main achievement is **[strength]**, which established/enabled **[impact]**. However, it is limited by **[drawback 1]** and **[drawback 2]**, and it does not address **[the gap your project targets]**.

Two worked examples:

- **Bethencourt, Sahai & Waters (CP-ABE, IEEE S&P 2007).** They introduced ciphertext-policy ABE, letting a data owner embed the access policy directly in the ciphertext so any user whose attributes satisfy it can decrypt. Its achievement is expressive, owner-controlled, fine-grained access without knowing recipients in advance — the model this project builds on. Its drawbacks are a security proof only in the generic-group model, decryption cost that grows with policy size, and no built-in revocation, which is precisely the operational gap we target.

- **Lewko & Waters (Multi-Authority ABE, EUROCRYPT 2011).** They removed the single-authority bottleneck by letting independent authorities issue attribute keys without a central coordinator. This improves trust decentralization and resilience for federated settings such as multi-hospital consortia. However, it adds setup and communication overhead, complicates revocation across authorities, and assumes classical (non-post-quantum) hardness — motivating both our decentralization and post-quantum future-work directions.

**Action:** write one such paragraph for each surveyed work, ending each with the drawback that leads into your research gap. Add the same treatment for 3–4 of the new 2021+ papers.

---

## 5. Fix the "Operational and Schedule Feasibility" section
The mentor is right — it currently reads as a single paragraph of week ranges and conveys no planning judgement. Rework it into an actual feasibility argument:

1. **Milestone/Gantt table** with columns: *Phase | Weeks | Tasks | Deliverable | Dependency | Owner*.
2. **Critical path & dependencies** — e.g., "CP-ABE (Charm/Docker) must complete before the hybrid envelope module; benchmarking depends on the API."
3. **Team allocation** — map the four members (Vansh, Kushagra, Geetanshi, Himanshi) to workstreams so "operational feasibility" actually means *do we have the people/skills/time*.
4. **Risk buffer** — note slack weeks and the Charm-Crypto fallback already mentioned.

Example table:

| Phase | Weeks | Key tasks | Deliverable | Depends on |
|---|---|---|---|---|
| P1 | 1–3 | Lit review; AES-256-GCM + RSA-OAEP modules | Crypto primitives + report §II | — |
| P2 | 4–6 | CP-ABE (BSW07) via Charm in Docker; hybrid envelope | Working key-wrap module | P1 |
| P3 | 7–9 | Flask API, JWT, ABAC PDP/PEP/PIP/PAP, S3/Azure adapters | REST backend | P2 |
| P4 | 10–11 | Sharing, versioned revocation, AA console | Revocation prototype | P3 |
| P5 | 12–15 | Minimal UI, benchmarking, security analysis, paper | Evaluation + final report | P3, P4 |

Then a one-line verdict: with this dependency structure and the four-person split, the plan fits one semester with a two-week buffer.

---

## 6. Add a References / links section
Add a numbered **References** section (IEEE style, since the report uses "IEEE S&P"-style venue naming) listing *every* foundational work already named **and** the new 2021+ papers from Section 1, each with a DOI/URL. In the running text, cite with bracketed numbers `[n]`. Also add inline links/footnotes wherever an idea was taken (e.g., outsourced decryption → Green et al.; ABAC model → NIST SP 800-162).

---

## Suggested order of work before the mentor meeting
1. Draft the **References** list (fast, unblocks citations). → Point 6 + 1
2. Rewrite **Literature Survey** into critical paragraphs with recent papers. → Points 1, 4
3. Draw and insert the **architecture + key-hierarchy diagrams**. → Point 3
4. Rebuild **Operational & Schedule Feasibility** as a Gantt/dependency table. → Point 5
5. Final **language editing pass** for unclear sentences. → Point 2

I can draft any of these sections directly into the document next.
