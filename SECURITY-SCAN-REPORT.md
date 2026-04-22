# Automated Security Scan Report

**Target:** `/home/naiplawan/Desktop/Workhub/venustus`
**Scanned at:** 2026-04-09T13:44:00+07:00
**Tools run:** Semgrep, Trivy, TruffleHog
**Tools skipped:** Bandit (no Python files in target)

---

## Pre-flight Summary

| Tool       | Status  | Version  |
| ---------- | ------- | -------- |
| Bandit     | SKIPPED | 1.9.4    |
| Semgrep    | OK      | 1.155.0  |
| Trivy      | OK      | 0.69.3   |
| TruffleHog | OK      | 3.93.8   |

---

## Bandit — Python SAST

**Summary:** Skipped: no Python files found in target directory.

---

## Semgrep — OWASP + Python Rules

**Summary:** 5 findings across 1 file (5 blocking)

```
┌─────────────┐
│ Scan Status │
└─────────────┘
  Scanning 255 files tracked by git with 544 Code rules:
                                                                                                                        
  Language      Rules   Files          Origin      Rules                                                                
 ─────────────────────────────        ───────────────────                                                               
  <multilang>       5     255          Community     544                                                                
  js               65      81                                                                                           
  html              1      21                                                                                           
  json              3       5                                                                                           
                                                                                                                        
                
                
┌──────────────┐
│ Scan Summary │
└──────────────┘
✅ Scan completed successfully.
 • Findings: 5 (5 blocking)
 • Rules run: 74
 • Targets scanned: 255
 • Parsed lines: ~99.9%
 • Scan skipped: 
   ◦ Files larger than  files 1.0 MB: 2
   ◦ Files matching .semgrepignore patterns: 64
 • Scan was limited to files tracked by git
 • For a detailed list of skipped files and lines, run semgrep with the --verbose flag
Ran 74 rules on 255 files: 5 findings.

                   
┌─────────────────┐
│ 5 Code Findings │
└─────────────────┘
                                                                                
    /home/naiplawan/Desktop/Workhub/venustus/extension/content/content-script.js
    ❯❱ javascript.browser.security.wildcard-postmessage-configuration.wildcard-postmessage-configuration
          ❰❰ Blocking ❱❱
          The target origin of the window.postMessage() API is set to "*". This could allow for information
          disclosure due to the possibility of any origin allowed to receive the message.                  
          Details: https://sg.run/PJ4p                                                                     
                                                                                                           
           28┆ window.postMessage({ source: 'venustus-command', action: 'toggle-overlays' }, '*');
            ⋮┆----------------------------------------
           31┆ window.postMessage({ source: 'venustus-command', action: 'remove' }, '*');
            ⋮┆----------------------------------------
           35┆ window.postMessage({ source: 'venustus-command', action: 'highlight', selector:
               msg.selector }, '*');                                                          
            ⋮┆----------------------------------------
           38┆ window.postMessage({ source: 'venustus-command', action: 'unhighlight' }, '*');
            ⋮┆----------------------------------------
          100┆ window.postMessage(msg, '*');
```

---

## Trivy — Dependencies & Misconfigurations

**Summary:** 0 vulnerabilities, 0 misconfigurations, 0 secrets

```
2026-04-09T13:44:28+07:00	INFO	[vuln] Vulnerability scanning is enabled
2026-04-09T13:44:28+07:00	INFO	[secret] Secret scanning is enabled
2026-04-09T13:44:28+07:00	INFO	[secret] If your scanning is slow, please try '--scanners vuln' to disable secret scanning
2026-04-09T13:44:28+07:00	INFO	[secret] Please see https://trivy.dev/docs/v0.69/guide/scanner/secret#recommendation for faster secret detection
2026-04-09T13:44:29+07:00	INFO	Suppressing dependencies for development and testing. To display them, try the '--include-dev-deps' flag.
2026-04-09T13:44:29+07:00	INFO	Number of language-specific files	num=1
2026-04-09T13:44:29+07:00	INFO	[npm] Detecting vulnerabilities...

Report Summary

┌───────────────────┬──────┬─────────────────┬─────────┐
│      Target       │ Type │ Vulnerabilities │ Secrets │
├───────────────────┼──────┼─────────────────┼─────────┤
│ package-lock.json │ npm  │        0        │    -    │
└───────────────────┴──────┴─────────────────┴─────────┘
Legend:
- '-': Not scanned
- '0': Clean (no security findings detected)
```

---

## TruffleHog — Secret Detection

**Summary:** 0 secrets detected (0 verified, 0 unverified)

```
🐷🔑🐷  TruffleHog. Unearth your secrets. 🐷🔑🐷

2026-04-09T13:44:32+07:00	info-0	trufflehog	running source	{"source_manager_worker_id": "Puzlv", "with_units": true}
2026-04-09T13:44:32+07:00	info-0	trufflehog	scanning repo	{"source_manager_worker_id": "Puzlv", "unit_kind": "dir", "unit": "/home/naiplawan/.cache/claude-tmp/trufflehog-259024-3231284686", "repo": "file:///home/naiplawan/Desktop/Workhub/venustus"}
2026-04-09T13:44:32+07:00	info-0	trufflehog	finished scanning	{"chunks": 71, "bytes": 529863, "verified_secrets": 0, "unverified_secrets": 0, "scan_duration": "73.620817ms", "trufflehog_version": "3.93.8", "verification_caching": {"Hits":0,"Misses":0,"HitsWasted":0,"AttemptsSaved":0,"VerificationTimeSpentMS":0}}
```

---

## Cross-Tool Observations

No cross-tool overlaps detected.

---

## Coverage Gaps

- **Business logic flaws** — not detectable by static tools
- **IDOR / broken object-level authorization** — requires runtime context
- **Python SAST** — Bandit was skipped (no Python files), but this is expected for a JS project
- **DAST (Dynamic Application Security Testing)** — no runtime scanning was performed against the dev server
- **Container image scanning** — no Dockerfile present; not applicable
- **Browser extension permissions audit** — Semgrep flagged postMessage wildcards, but a manual review of the extension's `manifest.json` permissions scope is recommended
