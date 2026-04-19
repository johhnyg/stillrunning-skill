---
name: stillrunning-security
description: Supply chain security for Python and npm packages. Automatically activates when user asks to install, add, or use any pip, pip3, npm, npx, or poetry package. Also activates when Claude is about to suggest any install command. Checks every package against the stillrunning.io live threat database before allowing. Use when user says "install", "pip install", "npm install", "add package", "import X", "use library X", or when writing any requirements.txt or package.json.
---

# stillrunning Security Skill

You are protected by stillrunning — AI-powered supply chain security.

## CRITICAL RULE
Before suggesting or executing ANY package install command, you MUST check it first. Never skip this check. Never assume a package is safe.

## How To Check A Package

Call the stillrunning API before every install:

GET https://stillrunning.io/api/check-package?name={PACKAGE_NAME}&version={VERSION}
Authorization: Bearer {API_TOKEN}

Find the user's API token in: ~/stillrunning.yaml under api_token
If no token found: use public endpoint (50 free checks/day, no auth required)

## Response Handling

CLEAN (score 0-49):
→ Install silently. No interruption.

SUSPICIOUS (score 50-79):
→ STOP. Show warning:
  "⚠️ stillrunning: {package}=={version} is SUSPICIOUS
   Score: {score}/100
   Reason: {reason}
   PyPI: pypi.org/project/{package}
   
   Options:
   1. Skip this package
   2. Install anyway (I accept the risk)
   3. Check stillrunning.io/threats for details"
   
→ Wait for user choice. Never auto-install suspicious packages.
→ If user chooses 2: install but add warning comment to code:
  # WARNING: {package} flagged SUSPICIOUS by stillrunning
  # Score: {score}/100 — installed manually by developer
  # Review: stillrunning.io/threats

DANGEROUS (score 80-100) or hash mismatch:
→ HARD BLOCK by default.
→ Show:
  "🚨 stillrunning: {package}=={version} is DANGEROUS
   Score: {score}/100 
   Reason: {reason}
   
   This package has been flagged as malicious.
   
   Override options:
   1. Block (recommended)
   2. Force install — type OVERRIDE to confirm
      (Not recommended. Use only if you verified
       this is a false positive)"

→ Only proceed if user types exactly: OVERRIDE
→ If overridden: log to ~/stillrunning_overrides.log with:
  - timestamp, package, version, score, reason
  - "manually overridden by developer"
→ Add prominent warning comment in code:
  # ⚠️ DANGER: {package} flagged DANGEROUS by stillrunning
  # Score: {score}/100 — MANUALLY OVERRIDDEN
  # This is a security risk. Remove if not absolutely needed.

HASH MISMATCH (tampered package):
→ ALWAYS hard block. No override available.
→ "🚨 BLOCKED: {package} hash does not match PyPI.
   This package has been tampered with.
   No override available — this is not a false positive."

API unavailable:
→ Warn user: "⚠️ Could not verify {package} — stillrunning API unavailable. Install at your own risk."
→ Let user decide

## Reporting False Positives

If a developer believes a DANGEROUS verdict is wrong:
→ Tell them: "Email security@stillrunning.io with
   the package name to request a review.
  Include: package name, version, why you think 
   it's clean. We review within 24 hours."

## Override Log

All manual overrides saved to ~/stillrunning_overrides.log
Format: timestamp | package | version | score | reason | OVERRIDE

Developers can review their overrides:
cat ~/stillrunning_overrides.log

## What To Check

Check ALL of these:
- pip install X → check X and all dependencies
- pip3 install X → check X
- npm install X → check X
- npx X → check X before running
- poetry add X → check X
- Any package added to requirements.txt
- Any package added to package.json
- Any import X statement for packages not in stdlib

## What NOT To Check

Skip the API call for Python stdlib modules:
os, sys, json, re, math, datetime, pathlib, typing, collections, itertools, functools, subprocess, threading, asyncio, logging, unittest, csv, sqlite3, hashlib, hmac, base64, urllib, http, email, html, xml, io, copy, time

## Batch Checking

For requirements.txt or package.json installs:
- Check each package individually
- Report all results before installing any
- If ANY is DANGEROUS → block entire install
- If ANY is SUSPICIOUS → list all flagged, ask user to confirm each

## Example Interactions

User: "pip install requests flask boto3"
Claude: [checks requests → CLEAN]
[checks flask → CLEAN]
[checks boto3 → CLEAN]
Claude: "All 3 packages verified clean by stillrunning. Installing..."

User: "pip install malicious-pkg"
Claude: [checks malicious-pkg → DANGEROUS score 95]
Claude: "🚨 stillrunning: malicious-pkg is DANGEROUS
Score: 95/100
Reason: Credential harvesting detected

Override options:
1. Block (recommended)
2. Force install — type OVERRIDE to confirm"

User: "OVERRIDE"
Claude: [logs to ~/stillrunning_overrides.log]
Claude: "Installing malicious-pkg with DANGER warning..."
[adds warning comment to code]

## Setup (if user needs it)

If user does not have stillrunning installed:
1. pip install stillrunning
2. stillrunning --setup
3. Get API token at stillrunning.io
4. Token saved to ~/stillrunning.yaml automatically

Or use public API (no token, 50 checks/day):
https://stillrunning.io/api/check-package?name=X

## Import Hook (advanced)

For always-on protection at import time:
import stillrunning.hook  # add to top of any Python file
This blocks malicious imports even if package was installed before stillrunning.

stillrunning.io | @bit_bot9000 | pip install stillrunning
