# M1 | Reconnaissance and Application Mapping

This folder contains the evidence collected during the initial black-box reconnaissance and application-mapping phase of the NetworkWalks Week 4 exercise.

**Target:** Mediroza General Hospital training target  
**Environment:** Kali Linux VM in VMware (NAT)  
**Primary tools:** WHOIS, WhatWeb, nslookup, cURL, Wafw00f, DNSRecon and Burp Suite

> **Exercise context:** This is a fictional training scenario. The screenshots are included with the tutor's approval and are mapped below to the activities they evidence.

## 1. Objectives

- Establish basic domain-registration and DNS context.
- Identify observable web-server and application technologies.
- Review HTTP response behaviour and available security headers.
- Observe whether automated requests receive consistent responses.
- Map visible application routes using Burp Suite.
- Document the patient login workflow and the observed authentication weakness.

## 2. Reconnaissance evidence

### E-01 and E-02: WHOIS domain-registration review

The WHOIS lookups record domain-registration and registrar context for the target. The two captures provide the corresponding lookup output from the exercise.

![E-01 - WHOIS lookup](1.%20recon%20whois.png)

![E-02 - Additional WHOIS output](1.1%20recon%20whois.png)

### E-03: Web technology fingerprinting

WhatWeb was used to identify observable web technologies. LiteSpeed was identified; one HTTPS fingerprinting request received a 403 response.

![E-03 - WhatWeb fingerprinting](2.%20recon%20whatweb.png)

### E-04: DNS resolution

The nslookup output was reviewed for the target's DNS information, including relevant A, MX, TXT and NS records.

![E-04 - nslookup results](3.%20recon%20nslookup.png)

### E-05: HTTP response headers and redirects

The cURL request followed the HTTP-to-HTTPS redirect and captured the resulting response headers. The response disclosed LiteSpeed. The captured headers did not include several commonly recommended browser security headers; this is a hardening observation, not by itself proof of an exploitable vulnerability.

![E-05 - cURL response headers](4.%20recon%20curl%20-I%20-L.png)

### E-06: WAF identification attempt

Wafw00f was used to check for a web application firewall. Results varied between runs, so the outcome is treated as inconclusive rather than proof of effective protection or of an absent WAF.

![E-06 - Wafw00f results](5.%20recon%20wafw00f.png)

### E-07: DNS enumeration

DNSRecon was used to enumerate additional DNS records. Mail and service-related records were observed and treated as contextual information only. Third-party mail and hosting infrastructure were outside the assigned testing scope and were not tested.

![E-07 - DNSRecon results](6.%20recon%20dnsrecon%20-d.png)

## 3. Burp Suite application mapping and authentication testing

### E-08: Site map review

Burp Suite was used to browse the assigned application and review the site map. The capture records the routes and application paths visible during the exercise, including the patient and staff login areas.

![E-08 - Burp Suite site map](7.%20recon%20burpsuite%20sitemap.png)

### E-09: Captured patient login request

The patient-login request was sent to Burp Repeater for controlled inspection of the request and response.

![E-09 - Patient login request in Repeater](8.%20recon%20burpsuite%20phplogin%20to%20repeater.png)

### E-10: SQL injection test response

The screenshot records the database error observed during input testing. This response provided evidence that user-supplied input was reaching a database query without safe handling.

![E-10 - SQL injection test in Burp Suite](9.%20recon%20burpsuite%20sql%20injection.png)

### E-11: Authentication-bypass behaviour

A subsequent controlled test returned a redirect to the patient portal without a valid password. This behaviour was documented as SQL injection leading to authentication bypass.

![E-11 - SQL injection test against the live training portal](10.%20recon%20sql%20injection%20live%20portal.png)

### E-12: Portal access after the bypass

The final screenshot records access to portal records following the authentication-bypass test. It is evidence of the resulting access in the fictional exercise; it is not presented as a separate IDOR finding.

![E-12 - Portal records accessed during the exercise](11.%20recon%20live%20site%20accessed%20records.png)

The exact request manipulation and authentication material are not reproduced in this README. Refer to finding F-01 in the main M4 report for the finding, impact and remediation.

## 4. Interpretation and limitations

The recorded responses identified LiteSpeed as the web server. The HTTP endpoint redirected to HTTPS, and the captured HTTPS response returned successfully. WhatWeb and Wafw00f did not produce fully consistent results. A 403 response from one automated fingerprinting request and differing WAF-identification results suggest that request handling may be filtered or variable. The available evidence is not sufficient to claim that a WAF blocks all automated testing or that it provides effective protection.

DNS records referenced mail and hosting-related services. These were treated as contextual information only. No testing of third-party infrastructure was performed.

Burp Suite mapping identified the login workflow and supported the controlled testing that demonstrated SQL injection-related authentication bypass. The resulting portal access is documented as part of the same finding, not as a separate access-control vulnerability.

## 5. Key outcome

The reconnaissance phase established the observable web stack, identified application routes and provided the technical context for finding F-01 in the main M4 report.

## 6. Scope reminder

Only the assigned fictional training target was in scope. DNS references to mail providers, control-panel services and other third-party infrastructure were not permission to test those systems.

Return to the [main M4 report](../README.md).
