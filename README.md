# Security Guardian Component

## Overview

This project provides a **security guardian framework** designed to monitor and protect a defined memory region.

It is intended strictly for **research and development (R&D) purposes**, and should not be interpreted as a replacement for established cryptographic libraries or standards such as AES implementations, TLS stacks, or certified security modules.

Instead, the framework relies on **user-provided callback implementations** that may internally use cryptographic algorithms (including components of AES or other primitives), but the security component itself does not enforce or implement any specific cryptographic standard.

---

## Core Concept

The system operates as a **memory region guardian**, providing:

- Integrity monitoring of a protected region
- Runtime verification of data consistency
- Optional encryption / hashing / storage abstraction via callbacks
- Continuous or scheduled security enforcement

At its core, it is a **security orchestration layer**, not a cryptographic implementation.

---

## Architecture

The framework is composed of four main parts:

### 1. Security Execution (Core Engine)
Responsible for runtime enforcement.

- `_sec_context()`  
  Initializes and binds the security context to a memory region.

- `_sec_runtime()`  
  Executes continuous or periodic integrity checks.

---

### 2. Security Callbacks (Consumer-Implemented)
These are **mandatory consumer-provided implementations**.

They define cryptographic and security behavior:

- `_sec_hash()` – hashing primitive  
- `_sec_encrypt()` – encryption primitive  
- `_sec_decrypt()` – decryption primitive  
- `_sec_nonce()` – secure nonce generation  
- `_sec_verify()` – integrity verification  
- `_sec_recovery()` – recovery handling  

> These may internally use AES or parts of AES logic, but the framework itself does not mandate or implement AES.

---

### 3. Secure Storage Interface (Consumer-Implemented)

Persistent storage abstraction:

- `_sec_storeread()` – read secure block  
- `_sec_storewrite()` – write secure block  

Used for storing security state, references, or metadata.

---

### 4. Security Utilities (Provided)

Optional non-critical helpers:

- `_sec_prngseed()` – seed PRNG  
- `_sec_genprngseq()` – generate pseudo-random sequences looking as 'noise-like' as much as it is possible considering normal spread nature of PRNGs.

> These utilities are not guaranteed to be cryptographically secure but can be used if security policy isnt AES mandatory. Another posibility is use generated sequences as input for any AES algorithms.

---

## Configuration

Compile-time parameters:

```c

/* default values when cmake properties are not provided 
 * before FetchContent_Declare(..) and FetchContent_MakeAvailable(...) 
 */

#define SEC_HASH_SIZE   32 // default value if cmake property not provided
#define SEC_NONCE_SIZE  16 // default value if cmake property

```

## CMake frendly integration

```cmake

include(FetchContent)

# -------------------------------------------------
# Security configuration (PUBLIC CACHE API)
# -------------------------------------------------
set(SECURITY_CONFIG_HASH_SIZE 32 CACHE STRING
    "Hash output size used by security subsystem")

set(SECURITY_CONFIG_NONCE_SIZE 16 CACHE STRING
    "Nonce size used by security subsystem")

# -------------------------------------------------
# Fetch security guardian
# -------------------------------------------------
FetchContent_Declare(
    rpv_security_guardian
    GIT_REPOSITORY git@github.com:pavelreutski/rpv-security-guardian.git
    GIT_TAG master
)

FetchContent_MakeAvailable(rpv_security_guardian)

```

### Clone repository

```bash
git clone git@github.com:pavelreutski/rpv-security-guardian.git
cd rpv-security-guardian
