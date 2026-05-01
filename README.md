# Signal

A lightweight, high-performance Signal system for Luau, It utilizes a **Doubly Linked List** structure to ensure $O(1)$ time complexity for connection and disconnection operations.

---
## v0.1.8 - Critical Fix Summary

> [!CAUTION]
> **Immediate Update Required:** This update fixes a logic error that caused event loss in systems with multiple subscribers.

### The Problem
There was a pointer management error within the linked list logic. When registering new listeners via `.Connect()` or `.Once()`, the module was overwriting the root's `.Next` pointer instead of correctly appending the new node to the end of the queue (`.Previous`).

### The Impact
* **Signal Amnesia:** Every new subscriber "stepped on" the previous one.
* **System Failures:** Only the most recently connected script would receive updates. For example, if the UI HUD connected after the Market Economy, the Market would stop receiving signals entirely.

### The Solution
Pointer assignments were refactored to maintain the integrity of the **circular linked list**. The root node now correctly maintains its reference to the head of the list while the tail updates dynamically with each new connection.

---

## Features
* **High Performance:** $O(1)$ insertion and removal of listeners.
* **Strict Typing:** Full support for Luau `!strict` and generics.
* **Safety:**