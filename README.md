Here’s a **terse but solid `README.md`** you can drop straight into the repo. It’s written to read like a systems / distributed engineering experiment, not a tutorial fluff piece.

---

# Colocation, Latency, and System Design

## Overview

This project explores **colocating code and data as a latency optimization** by comparing two REST API architectures that are identical at the application layer but differ in how and where data is accessed. One implementation uses a traditional client–server PostgreSQL database running as a separate service, while the other uses an embedded SQLite database colocated within the application process. A third program acts as a load generator to measure and record request latency.

The goal is to quantify how architectural decisions—specifically data placement—impact end-to-end latency, and to connect those results to broader principles in distributed and multicore system design.

---

## Colocating Code and Data

Colocation refers to placing computation and the data it operates on as close together as possible, either logically or physically. In this project, SQLite is embedded directly into the application process, eliminating network hops, kernel context switches, and serialization overhead required when communicating with an external database.

By contrast, PostgreSQL represents a decoupled client–server model where even local communication incurs IPC, networking stacks, and protocol overhead. This experiment demonstrates how colocating code and data can significantly reduce request latency, especially for simple, read-heavy workloads.

---

## Optimizing for Low Latency in Distributed Systems

In distributed systems, latency compounds quickly: network hops, load balancers, TLS handshakes, serialization, and remote scheduling all add measurable cost. While horizontal scalability and fault isolation often require service separation, this project shows the trade-off between scalability and latency.

The PostgreSQL-backed implementation mirrors real-world distributed architectures, while the embedded SQLite version approximates a single-node or edge-deployed system. The comparison highlights why colocated designs are common in latency-sensitive systems such as caches, edge services, and embedded control planes.

---

## Optimizing for Low Latency in Multicore Systems

Colocation is not only a distributed systems concern—it also matters within a single machine. Embedded databases benefit from shared memory access, CPU cache locality, and reduced synchronization boundaries compared to cross-process communication.

This project implicitly demonstrates multicore optimization principles: fewer context switches, better cache utilization, and tighter control over concurrency primitives. These factors become increasingly important as core counts grow and memory access patterns dominate performance.

---

## What This Project Demonstrates

* How data placement directly impacts request latency
* The cost of network and IPC boundaries, even on localhost
* Why embedded and colocated designs excel for low-latency workloads
* The trade-offs between scalability, isolation, and performance

---

## Repository Structure (Conceptual)

* **postgres-api/** — REST API backed by PostgreSQL
* **sqlite-api/** — REST API backed by embedded SQLite
* **load-generator/** — Concurrent HTTP client measuring latency


