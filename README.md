# Detection Rules Repository
## Overview

This repository contains a curated set of security detection rules designed to identify suspicious and malicious activity on endpoints. The rules are intended to support threat detection, investigation, and response use cases across enterprise environments.

All detection rules in this repository are authored in Kusto Query Language (KQL) and are designed to operate on endpoint telemetry collected from Windows systems. The detections focus on behavioral signals rather than static indicators, enabling coverage across a wide range of attack techniques.

The rules rely on telemetry generated from:

- Windows System Event Logs
- Sysmon (System Monitor) events

These events are forwarded by the SYNERGIX LEDR agent, which runs locally on endpoints and securely ships telemetry to the centralized data platform where the KQL detections are executed.

## Detection Rules
The detection rules in this repository share the following characteristics:

- Language: Kusto Query Language (KQL)
- Data sources:
 - Windows System event logs
 - Sysmon event logs (process creation, network connections, file creation, registry activity, etc.)
- Collection mechanism:
 - Events are collected and forwarded by the SYNERGIX LEDR agent installed on endpoints
- Execution environment:
 - Centralized analytics platform supporting KQL queries

Each rule is designed to:

- Detect specific attacker techniques or anomalous behaviors
- Minimize false positives through context‑aware logic
- Be easily tunable for different environments

Rules may include:

- Process execution and command‑line analysis
- Persistence and autorun mechanism detection
- Credential access and token abuse indicators
- Living‑off‑the‑land binary (LOLBin) usage
- Suspicious parent/child process relationships
- Network‑based indicators derived from endpoint telemetry

> Note: Proper Sysmon configuration and reliable event forwarding by the SYNERGIX LEDR agent are required for full detection coverage.


## Summary
This repository provides a practical and extensible collection of KQL‑based detection rules built on high‑fidelity endpoint telemetry. By leveraging System and Sysmon events forwarded by the SYNERGIX LEDR agent, these detections enable visibility into attacker behaviors that are often missed by signature‑based controls.

The rules are intended to be:
- Transparent and easy to review
- Modular and customizable
- Aligned with modern detection engineering and threat‑hunting practices

Contributions, feedback, and improvements are encouraged to help expand coverage and improve detection quality over time.
