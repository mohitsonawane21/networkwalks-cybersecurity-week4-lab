# NetworkWalks Week 4 | Black-Box Penetration Testing Report

**Milestone 4: Final Assessment Report**  
**Assessment target:** Mediroza General Hospital training target  
**Prepared by:** Phenyo Bareki  
**Programme:** NetworkWalks Cybersecurity Programme  
**Assessment window:** Five days, as specified in the exercise brief  
**Assessment approach:** Black-box web application assessment  
**Status:** Technical milestones M1–M3 reported complete; M4 documentation

> **Training exercise notice:** This report documents a fictional NetworkWalks training scenario. The target, records and findings are exercise material, not claims about a real hospital or real individuals. The tutor has approved sharing the exercise evidence. Screenshots are mapped in the supporting milestone READMEs and are used to substantiate the reported activities.

---

## 1. Executive Summary

This report consolidates the reconnaissance, application assessment, document-security testing and sensitive-data exposure activities completed during the NetworkWalks Week 4 black-box penetration testing exercise.

The assessment began with domain and web reconnaissance. The observed web service used LiteSpeed, redirected HTTP traffic to HTTPS, and disclosed some implementation details through its responses. Automated technology fingerprinting and WAF-detection results were inconsistent, so application mapping was continued manually through Burp Suite.

During application testing, the patient login workflow returned a database syntax error when supplied with unexpected input. A subsequent controlled test produced an HTTP redirect to the patient portal without a valid password. This behaviour demonstrated an SQL injection condition capable of bypassing the application's authentication check.

The portal exposed three password-protected PDF reports. Password recovery was performed against the exercise files, and all three documents were subsequently opened as part of the milestone work. Reviewing document properties and embedded comments led to an old/legacy location associated with database-related material. The exercise evidence records subsequent discovery of staff and shareholder information at that location.

The principal security concern is the combination of weak input handling and insufficient protection of sensitive resources. An authentication weakness can provide a route into restricted application functionality; weak document passwords and exposed legacy material can then increase the impact of that access.

**Overall qualitative assessment: Critical.** This is a qualitative assessment based on the sensitivity of the exercise data and the demonstrated authentication bypass and subsequent data exposure. No CVSS score was calculated.

### Milestone status

| Milestone | Objective | Outcome |
|---|---|---|
| M1 | Locate three confidential PDF lab reports | Completed according to the exercise evidence |
| M2 | Recover access to all three protected PDFs | Completed according to the exercise evidence |
| M3 | Identify the further critical data exposure | Completed according to the user's completion update and evidence folder |
| M4 | Produce the consolidated professional report | Documented in this README |

## 2. Scope and Rules of Engagement

### 2.1 Authorised scope

- **Target:** The Mediroza General Hospital hostname specified in the NetworkWalks exercise brief.
- **Assessment type:** Black-box assessment of the assigned web application.
- **Environment:** NetworkWalks-provided training environment. The user reported that the tutor confirmed the target isolation.
- **Assessment period:** Five days, according to the exercise brief.
- **Permitted objectives:** The three technical milestones described in the assignment and preparation of the final report.

### 2.2 Out of scope

The assessment was limited to the assigned target. DNS references to mail providers, hosting control-panel services, third-party platforms or other associated infrastructure were treated as reconnaissance context only and were not treated as permission to test those systems.

The exercise did not authorise denial-of-service activity, social engineering, persistence, testing unrelated infrastructure or accessing systems outside the defined target.

### 2.3 Evidence handling

Evidence should be limited to what is necessary to demonstrate each finding. Patient-style records, staff details, shareholder information, passwords, session cookies and database contents must be redacted from public documentation. Original evidence should be retained only in the approved private location and shared with the tutor through the agreed channel.

## 3. Scope and Methodology

The assessment followed a black-box workflow informed by the general principles of PTES and the OWASP Web Security Testing Guide.

### 3.1 Environment

- Kali Linux virtual machine running in VMware.
- VMware network mode: NAT.
- Burp Suite Community Edition for manual web application mapping and request inspection.
- ExifTool for document metadata review.
- John the Ripper for offline password recovery against the supplied exercise documents.
- Supporting command-line tools: WHOIS, WhatWeb, nslookup, DNSRecon, cURL, Wafw00f, strings and qpdf.

### 3.2 Assessment phases

1. **Reconnaissance:** Domain registration, DNS records, HTTP response headers, technology fingerprinting and WAF behaviour.
2. **Application mapping:** Manual browsing and review of the Burp Suite site map to identify application routes and login surfaces.
3. **Authentication testing:** Review of the patient login request and response behaviour, including database error disclosure and authentication-bypass validation.
4. **Document assessment:** Identification of the three exercise PDFs, extraction of password hashes, offline password recovery and verification that the documents could be opened.
5. **Metadata and exposure review:** Inspection of document metadata and embedded comments, followed by review of the legacy location identified during the exercise.
6. **Reporting:** Consolidation of evidence, impact, risk and remediation recommendations.

Automated tools were used conservatively. WhatWeb and Wafw00f produced inconsistent responses, including a 403 response in one fingerprinting attempt and differing WAF identification results. These observations were treated as evidence of variable request handling, not definitive proof that a particular WAF configuration was effective.

## 4. Findings and Proof of Exploitation

The findings below describe the behaviour documented during the training exercise. Sensitive values and operational exploit strings are intentionally omitted from this public report.

### F-01 — SQL Injection leading to patient-portal authentication bypass

**Severity:** Critical  
**Affected component:** Patient login workflow  
**Category:** Injection / Authentication failure  
**Evidence:** Reconnaissance folder screenshots 8–10 (retain redacted copies for external sharing).

**Observation**

The patient login workflow returned a database syntax error when unexpected input was submitted in the username field. This indicated that user-controlled input was reaching a database query in an unsafe manner.

A subsequent controlled test produced an HTTP 302 redirect to the patient portal despite the absence of a valid password. The response was consistent with the authentication check being bypassed. The exercise screenshots also document access to the portal after this response.

**Impact**

An unauthenticated user could potentially gain access to functionality intended for authenticated patients. In the context of this exercise, that access provided a route to the three protected PDF reports. If present in a real environment, this weakness could expose highly sensitive personal or medical information.

**Recommendation**

- Use parameterised queries/prepared statements for every database operation involving user input.
- Avoid constructing SQL statements through string concatenation.
- Apply server-side input validation as a secondary control, not as a substitute for parameterisation.
- Return generic authentication errors to users and log detailed database errors securely on the server.
- Review all login and search endpoints for the same unsafe query pattern.
- Rotate or invalidate sessions after authentication state changes and enforce server-side authorisation checks on every protected resource.

**Reference:** [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

### F-02 — Weak password protection on PDF reports

**Severity:** High  
**Affected assets:** Three exercise PDF lab reports  
**Category:** Weak cryptographic protection / Weak password strength  
**Evidence:** Data-extraction folder screenshots documenting hash extraction, password-recovery attempts and successful opening of the three PDFs.

**Observation**

The three password-protected exercise PDFs were processed locally. Password hashes were extracted and tested using John the Ripper. The evidence set records a failed attempt against one document using the NetworkWalks tool and a successful recovery using John the Ripper. 

**Impact**

Password-protected documents using weak or guessable passwords may be vulnerable to offline password recovery. Once recovered, the contents can be read outside the application, bypassing protections that depend solely on the PDF password.

**Recommendation**

- Do not rely on PDF passwords as the primary access-control mechanism for sensitive records.
- Store sensitive documents behind authenticated application access controls.
- Use strong, unique, randomly generated document passwords where document-level encryption is required.
- Use modern encryption settings supported by the document format and approved organisational policy.
- Distribute document passwords through a separate, controlled channel.
- Review whether the documents need to be downloadable at all, and apply data-loss prevention and access logging where appropriate.

### F-03 — Sensitive information exposed through document metadata and a legacy location

**Severity:** Critical  
**Affected component:** /old location associated with database-related material  
**Category:** Sensitive information exposure / Inadequate protection of backup material  
**Evidence:** Critical-data-exposure folder screenshots 1–7. 

**Observation**

During review of the exercise PDFs, document properties and embedded comments were examined in addition to the visible document content. A comment/metadata clue in one of the reports led to an old or legacy location associated with database-related material. The evidence folder records discovery of staff and shareholder information at the location.

This report intentionally does not reproduce database contents, names, salary figures, shareholder details, credentials, file contents or the precise sensitive resource location.

**Impact**

Legacy backups and associated records can expose information that is not intended to be available through the public application. Depending on the contents, unauthorised access could lead to disclosure of employee information, compensation data, ownership information and other confidential business records. If database credentials or backup files are exposed, the impact may extend beyond the records directly observed.

**Recommendation**

- Remove database backups, exports and legacy files from web-accessible directories.
- Store backups in access-controlled locations that are not directly reachable from the public web.
- Enforce least-privilege access and deny public access by default.
- Review web-server aliases, old domains, migration directories and backup-retention locations.
- Remove sensitive metadata and embedded comments before documents are distributed.
- Rotate any credentials that may have been exposed and review access logs for unauthorised use.
- Establish a documented backup-retention and secure-deletion process.
- Monitor for access to backup paths and unusual downloads of sensitive files.

### F-04 — Missing or incomplete HTTP security headers

**Severity:** Low (hardening observation; not independently exploited)  
**Affected component:** Public web response  
**Category:** Security configuration  
**Evidence:** Reconnaissance folder screenshot 4.

**Observation**

The recorded cURL response showed an HTTP-to-HTTPS redirect followed by a successful HTTPS response from a LiteSpeed server. The captured response did not include several commonly recommended browser security headers, including HSTS, Content-Security-Policy and X-Frame-Options.

This is a configuration observation. The absence of these headers alone does not prove that an exploit is possible.

**Recommendation**

Review and configure security headers according to the application's requirements, including:

- Strict-Transport-Security after confirming HTTPS is correctly deployed across the relevant hostnames.
- Content-Security-Policy to reduce the impact of content-injection vulnerabilities.
- X-Content-Type-Options: nosniff.
- An appropriate clickjacking policy using CSP frame-ancestors and/or X-Frame-Options for legacy support.
- Referrer-Policy and Permissions-Policy where appropriate.

Test the headers across relevant application routes and ensure that policies do not break legitimate functionality.

## 5. Risk Rating

The following ratings are qualitative and based on the observed exercise outcomes, potential impact and sensitivity of the affected information. They are not CVSS scores.

| ID | Finding | Rating | Rationale |
|---|---|---|---|
| F-01 | SQL injection leading to authentication bypass | Critical | Authentication controls were bypassed and restricted portal functionality was reached |
| F-02 | Weak password protection on PDF reports | High | Offline password recovery enabled access to all three exercise documents |
| F-03 | Sensitive information exposed through metadata/legacy location | Critical | Exercise evidence records access to database-related material and staff/shareholder information |
| F-04 | Missing or incomplete HTTP security headers | Low | Browser-side hardening opportunity; no direct exploit was demonstrated |

### Overall assessment

The combined chain of weaknesses creates a significant risk. The SQL injection issue provides an initial route into restricted functionality, while weak document passwords and the legacy information exposure increase the potential consequences. The highest priority should be remediation of the injection flaw and removal/protection of sensitive legacy material.

## 6. Recommendations and Remediation Priorities

| Priority | Recommended action | Related finding |
|---|---|---|
| Immediate | Correct unsafe database query construction using prepared statements; review all similar endpoints | F-01 |
| Immediate | Restrict access to the legacy location and remove exposed backups or exports from web-accessible paths | F-03 |
| Immediate | Rotate credentials if any database or application secrets were present in exposed material; review relevant access logs | F-03 |
| High | Enforce server-side authorisation checks for every report and record request | F-01, F-02 |
| High | Replace weak document passwords and review the use of PDF encryption for sensitive records | F-02 |
| High | Review backup storage, migration procedures, retention and secure deletion | F-03 |
| Medium | Remove verbose database errors from user-facing responses and improve secure logging | F-01 |
| Medium | Remove unnecessary metadata and embedded comments from distributed documents | F-03 |
| Low | Implement and validate appropriate HTTP security headers | F-04 |

## 7. Limitations

- The risk ratings are qualitative; no CVSS vector or score was calculated.

## 8. Conclusion

The Week 4 exercise demonstrated how a black-box assessment can move from external reconnaissance to application-level testing and then to the identification of sensitive information exposure.

The most significant result was the authentication bypass associated with unsafe SQL handling. The subsequent document assessment demonstrated that weak PDF passwords can undermine document-level protection, while metadata and legacy-location review can reveal information that is not apparent from the visible content of a file.

The exercise reinforces the importance of secure database access, server-side authorisation, appropriate document protection, safe backup storage and disciplined metadata handling. Remediation should begin with the critical authentication and sensitive-data exposure findings, followed by document-security improvements and web-response hardening.

---

## 9. Evidence Repository

The milestone folders contain the supporting screenshots and technical notes. Each milestone README maps the original screenshot filenames to the corresponding assessment step and explains what each image demonstrates.

- [Reconnaissance evidence (M1)](1.%20reconnaissance/README.md)
- [PDF data-extraction and password-recovery evidence (M2)](2.%20data-extraction/README.md)
- [Critical-data-exposure evidence (M3)](3.%20critical-data-exposure/README.md)

The M2 README includes the complete screenshot sequence for all three PDF reports: hash extraction, password-recovery attempts and successful document-opening verification. The M4 report remains the main entry point, while the milestone READMEs provide the supporting evidence.

## 10. References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP Cheat Sheet Series – SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP Cheat Sheet Series – Authorization](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP Cheat Sheet Series – Cryptographic Storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html)
- [CWE-200: Exposure of Sensitive Information](https://cwe.mitre.org/data/definitions/200.html)




## 11. Video Walkthrough

[![Watch the NetworkWalks Week 4 Black-Box Assessment video on YouTube](https://img.youtube.com/vi/EcpX_bMBOKc/hqdefault.jpg)](https://www.youtube.com/watch?v=EcpX_bMBOKc)

*Click the thumbnail to watch the Week 4 black-box assessment walkthrough on YouTube.*
