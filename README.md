# Automated Governance Framework for LLM-Driven Refactoring

## 🚀 Project Overview
This Proof of Concept (PoC) demonstrates an **Instruction-Driven Orchestration** layer designed to integrate LLMs into enterprise workflows with high determinism. By utilizing a **Policy-as-Code** approach, this framework ensures that AI agents (GitHub Copilot) adhere to strict reliability, security, and architectural standards during automated refactoring of Java microservices.



## 🛠 The Problem: "AI Hallucinations & Technical Debt"
Unchecked AI code generation often leads to:
* **Architectural Drift:** Logic leaking across Controller/Service layers.
* **Non-Deterministic Output:** Inconsistent coding patterns across different sessions.
* **Reliability Gaps:** Missing null-safety, improper transaction handling, and generic error logging.

## 💡 The Solution: Governance via Instruction Contracts
By implementing a centralized `.github/copilot-instructions.md` configuration, we establish a **Source of Truth** that the AI Agent must satisfy. This transforms the LLM from a simple autocomplete tool into a **Standard-Compliant Refactoring Agent**.

## 🏗 Key Reliability Guardrails Enforced
The governance layer automates the following "Vital Signs" of production-ready code:

* **Memory & Thread Safety:** Mandates `Optional` and `Objects.requireNonNull` to eliminate production `NullPointerExceptions`.
* **Data Integrity:** Automatically ensures all data-modifying methods are annotated with `@Transactional`.
* **Observability:** Standardizes `private static final` Logger definitions to ensure consistent trace logs for **Root-Cause Analysis (RCA)**.
* **Decoupled Architecture:** Enforced Dependency Injection via constructors and strict separation of concerns.
* **Deterministic Exceptions:** Replaces generic errors with domain-specific custom exceptions (e.g., `UserNotFoundException`).

## 🚦 Getting Started

### Prerequisites
* **VS Code** (required for full Agent Mode support).
* **GitHub Copilot & Copilot Chat Extensions**.

### Setup & Activation
1. Clone the repository to your local environment.
2. Enable Instruction Files in VS Code:
   * Navigate to `Settings` > `Extensions` > `GitHub Copilot Chat`.
   * Check the box: **"Code Generation: Use Instruction Files"**.
3. The AI Agent will now automatically ingest the global rules defined in `.github/copilot-instructions.md`.

## 📈 Impact & Results
In testing, the framework successfully autonomously refactored a legacy service by:
1.  **Correcting Architectural Smells:** Moving business logic from controllers to services.
2.  **Automated Directory Management:** Creating new exception packages and classes based on the instruction contract.
3.  **100% Compliance:** Resolving all compilation errors while maintaining strict adherence to Swisscom-standard naming conventions.

## 🧠 SRE Perspective
This project applies **Systems Thinking** to AI tooling. By treating the LLM as a system component that requires monitoring and guardrails, we can automate the "boring stuff" (toil) while maintaining the high-reliability standards required for production-scale environments.
