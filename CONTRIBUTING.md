# Contributing Guide
Thank you for your interest in contributing to this repository. Contributions help improve detection quality, expand coverage, and strengthen endpoint security for everyone. This guide outlines how to propose changes, author detection rules, and maintain consistency across the project.

## Overview
This repository contains security detection rules written in Kusto Query Language (KQL).
All rules rely on Windows System and Sysmon events that are forwarded by the SYNERGIX LEDR agent running on endpoints.
By contributing, you agree to follow the guidelines below to ensure detections are:

- Accurate
- Maintainable
- Understandable by other practitioners
- Compatible with the expected data sources


## What You Can Contribute
We welcome:

✅ New detection rules

✅ Improvements to existing rules (logic, performance, false‑positive reduction)

✅ Documentation updates or clarifications

✅ Bug fixes and minor refactoring

✅ Additional context (comments, edge cases, tuning advice)


We generally do not accept:

❌ Detections that depend on unsupported data sources

❌ Obfuscated or intentionally unreadable queries

❌ Rules that execute destructive actions

❌ Vendor‑specific features not documented in the repository



## Detection Rule Requirements
All detection rules must meet the following criteria:
### 1. Language and Data Sources

- Rules must be written in Kusto Query Language (KQL)
- Rules must rely on:
 - Windows System event logs
 - Sysmon event logs
- Events must be assumed to be collected by the SYNERGIX LEDR agent

Rules that require additional telemetry must clearly state that dependency.

### 2. Rule Structure
Each detection rule should include:

- A clear rule name
- A concise description explaining what the rule detects
- The KQL query
- Inline comments explaining complex logic
- Assumptions or prerequisites (e.g., Sysmon configuration)

Example header format:
```
// Rule Name: Suspicious Process Creation via LOLBin
// Description: Detects abuse of trusted Windows binaries often used for living-off-the-land attacks
// Data Sources: Sysmon Event ID 1 (Process Create), System events
// Author: <your name or handle>
// Last Updated: YYYY-MM-DD
```

### 3. Query Quality Guidelines
Contributed queries should:
- Prefer behavioral detection over static indicators
- Avoid overly broad logic that produces excessive noise
- Use well‑defined filters and conditions
- Be readable and well‑commented
- Handle common edge cases where possible

Avoid:
- Hard‑coded environment‑specific values
- Overly complex one‑line queries
- Regex where simple operators are sufficient


### 4. Performance Considerations
Please consider:
- Reducing unnecessary joins or lookups
- Filtering early in the query
- Using project to limit output fields when applicable

A detection that times out or runs inefficiently is unlikely to be accepted.

## Submitting a Contribution
### Step 1: Fork the Repository
Create a personal fork of the repository and clone it locally.

### Step 2: Create a Branch
Create a descriptive branch name:
```
git checkout -b add-detection-suspicious-regsvr32
```

### Step 3: Make Your Changes

- Add or modify detection rules
- Update documentation if needed
- Ensure formatting and comments follow repository conventions


### Step 4: Test and Review
Before submitting:

- Validate the KQL syntax
- Manually review logic for false positives and gaps
- Ensure the rule aligns with available System and Sysmon data


### Step 5: Open a Pull Request
When opening a PR:

- Clearly describe what the change does
- Explain why it is needed
- Reference any related issues, research, or attack techniques
- Mention required Sysmon events or configurations, if applicable


## Review Process
All pull requests are reviewed for:

- Correctness
- Clarity
- Performance
- Alignment with repository goals

Reviewers may:

- Request changes or clarifications
- Suggest tuning or refactoring
- Ask for additional documentation

Please be patient—security detections require careful review.

## Style and Documentation

- Use clear, professional language
- Avoid slang or offensive terms
- Prefer explicit logic over assumptions
- Add comments where detection intent may not be obvious

Good documentation is as important as the query itself.

## Code of Conduct
By contributing, you agree to:

- Be respectful and constructive
- Provide actionable feedback
- Focus discussions on technical merit

Harassment, hostility, or unprofessional behavior will not be tolerated.

## Final Notes
This repository is intended to support **defensive security, detection engineering, and threat hunting**. 
Contributions should always align with those goals.

Thank you for helping improve detection quality and operational security.
