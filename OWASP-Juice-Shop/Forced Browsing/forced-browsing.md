# [Finding #] – [Vulnerability Title]

## Summary
One or two sentences describing what was found and its impact.

## Severity
Critical / High / Medium / Low / Informational
(Optionally map to CVSS score if you want to look more professional)

## Category (OWASP Classification)
A01:2025 – Broken Access Control

## Affected Component
- URL/Endpoint: `/score-board`
- Application: OWASP Juice Shop
- Environment: Local instance, version 20.2.0

## Description
Explain the vulnerability in technical detail — what's happening under the hood,
why it exists (e.g., client-side-only restriction, no server-side enforcement).

## Steps to Reproduce
1. On home page, manually enter `/score-board` in the URL bar
2. Observe that the page loads without authentication or menu access
3. Note that the "Score Board" menu item now appears in the nav bar

## Proof of Concept (Screenshots/Video)
Before: navigation menu without Score Board.
![screenshot](forced-browsing-before.png)  
After: page loaded directly via URL, and the feature now appears.
![screenshot](forced-browsing-after.png)

## Impact
What could an attacker do with this? (e.g., access hidden admin functionality,
bypass intended user flow, discover other hidden routes)

## Root Cause
Why did this happen from a dev perspective (e.g., relying on hiding UI elements
instead of enforcing access control server-side or via route guards)

## Remediation / Recommendation
- Implement server-side authorization checks for sensitive routes
- Don't rely on hiding UI elements as a security control
- Apply the principle of "fail securely" — deny by default

## References
- OWASP Top 10: A01:2025 – Broken Access Control
