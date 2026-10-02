# 🌐 AWS Multi-VPC Architecture & Cross-VPC Peering Implementation

A hands-on, production-grade cloud networking project demonstrating the configuration of two isolated VPC environments (test-vpc and prod-vpc) in AWS Region *us-east-2 (Ohio)* and establishing private, secure cross-VPC communication using *AWS VPC Peering*, custom route tables, and granular Security Group controls.

---

## 📌 Project Overview

In this project, two completely isolated virtual private clouds were built from scratch with custom non-overlapping CIDR blocks. To enable secure private communication between workloads in both VPCs without routing traffic over the public internet, a *VPC Peering Connection* was established and verified using bi-directional ICMP (ping) tests between EC2 instances.

### Key Objectives:
* Design custom VPCs, multi-AZ subnets, and internet gateway attachments.
* Configure explicit route tables for default internet egress and private peer routing.
* Establish and accept a local AWS VPC Peering connection (test-prod-peering).
* Implement least-privilege inbound Security Group rules for cross-VPC traffic.
* Validate end-to-end bi-directional private connectivity across VPC boundaries.

---

## 🏗️ Network Architecture & Topology

```text
┌──────────────────────────────────────────────────────────────────────────────────────────┐
│ AWS Region: us-east-2 (Ohio)                                                             │
│                                                                                          │
│   ┌────────────────────────────────────────┐    ┌────────────────────────────────────┐   │
│   │ test-vpc (10.0.0.0/24)                 │    │ prod-vpc (192.0.0.0/16)            │   │
│   │                                        │    │                                    │   │
│   │  ┌──────────────────────────────────┐  │    │  ┌──────────────────────────────┐  │   │
│   │  │ test-public-subnet (10.0.0.0/24) │  │    │  │ prod-public-subnet-1       │  │   │
│   │  │ AZ: us-east-2a                   │  │    │  │ (192.0.0.0/24) AZ: us-east-2b│  │   │
│   │  │                                  │  │    │  │                              │  │   │
│   │  │  [test-instance]                 │  │    │  │  [prod-instance]             │  │   │
│   │  │  Priv IP: 10.0.0.176             │  │    │  │  Priv IP: 192.0.0.15          │  │   │
│   │  │  Pub IP:  3.144.254.51           │  │    │  │  Pub IP:  3.142.45.53         │  │   │
│   │  └──────────────────────────────────┘  │    │  └──────────────────────────────┘  │   │
│   │                 │                      │    │                 │                  │   │
│   │            test-rt                     │    │              prod-rt               │   │
│   └─────────────────┼──────────────────────┘    └─────────────────┼──────────────────┘   │
│                     │                                             │                      │
│                     └───────► [ VPC Peering: test-prod-peering ] ◄┘                      │
│                               (pcx-063ccc43bc91a1996)                                    │
└──────────────────────────────────────────────────────────────────────────────────────────┘
