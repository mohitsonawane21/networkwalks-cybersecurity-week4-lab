# M2 | PDF Document Assessment and Password Recovery

This folder contains the supporting evidence for Milestone 2 of the NetworkWalks Week 4 exercise. The objective was to assess the protection applied to the three PDF reports identified during the preceding milestone, recover access to the supplied files where possible, and verify the results by opening each document.

## 1. Objective

Assess the password protection applied to the three supplied PDF reports using offline password-auditing techniques. The work was limited to the files provided for this fictional training exercise.

## 2. Tools and process

- **PDF hash extraction utility:** Extracted password-verification data from each supplied PDF for offline auditing.
- **John the Ripper:** Used to perform password-recovery attempts against the extracted hashes.
- **NetworkWalks password tool:** Used for an initial attempt against the third document.
- **PDF reader:** Used to verify whether each recovered password successfully opened its corresponding document.

The workflow was:

1. Obtain the three PDF reports identified during the exercise.
2. Extract the password hash from each PDF.
3. Attempt offline password recovery using the available training tools.
4. Record the outcome of each attempt.
5. Open each PDF using the recovered password to verify access.
6. Document the evidence and security implications.

## 3. Screenshot evidence map

The screenshots are embedded below in the same sequence as their existing filenames and numbering. Each caption explains what the evidence demonstrates and how it contributes to the milestone.

### Report 1 — Hash extraction, password recovery and document access

**Evidence 1.** Extracting the password hash from the first PDF report.

![Evidence 1 - Report 1 hash extraction](./1.%20patient_report_1.pdf%20extracted%20hash.png)

**Evidence 1.1.** Password-recovery result for the first report.

![Evidence 1.1 - Report 1 password recovery](./1.1%20patient_record_1_password.png)

**Evidence 1.11.** Verification that the first PDF opens successfully using the recovered password.

![Evidence 1.11 - Report 1 opened](./1.11%20patient_record_1_openpdf.png)

**Result:** The first report's password was recovered, and the document was opened successfully.

### Report 2 — Hash extraction, password recovery and document access

**Evidence 2.** Extracting the password hash from the second PDF report.

![Evidence 2 - Report 2 hash extraction](./2.%20patient_report_2.pdf%20extracted%20hash.png)

**Evidence 2.1.** Password-recovery result for the second report.

![Evidence 2.1 - Report 2 password recovery](./2.1%20patient_record_2_password.png)

**Evidence 2.11.** Verification that the second PDF opens successfully using the recovered password.

![Evidence 2.11 - Report 2 opened](./2.11%20patient_record_2_openpdf.png)

**Result:** The second report's password was recovered, and the document was opened successfully.

### Report 3 — Initial failure, alternate recovery and document access

**Evidence 3.1.** The initial password-recovery attempt using the NetworkWalks tool did not produce a successful result.

![Evidence 3.1 - Initial recovery attempt failed](./3.1%20patient_record_3_password_cracking_failed_network_walkstool.png)

**Evidence 3.1.** Extracting the password hash from the third PDF report. The original filename includes the additional period after the evidence number.

![Evidence 3.1 - Report 3 hash extraction](./3.1.%20patient_report_3.pdf%20extracted%20hash.png)

**Evidence 3.111.** Using John the Ripper for the subsequent password-recovery attempt, which succeeded.

![Evidence 3.111 - John the Ripper recovery](./3.111%20patient_record_3_password_cracking_using_john%20works.png)

**Evidence 3.1111.** Verification that the third PDF opens successfully using the recovered password.

![Evidence 3.1111 - Report 3 opened](./3.1111%20patient_record_3_openpdf.png)

**Result:** The first recovery attempt was unsuccessful. After extracting the PDF hash, John the Ripper was used successfully, and the third report was opened.

## 4. Consolidated results

| PDF report | Hash extracted | Recovery outcome | Document opened |
|---|---|---|---|
| Report 1 | Yes | Successful | Yes |
| Report 2 | Yes | Successful | Yes |
| Report 3 | Yes | Initial NetworkWalks tool attempt unsuccessful; subsequent John the Ripper attempt successful | Yes |

**Milestone outcome:** Password recovery and document-access verification were completed for all three supplied PDFs.

## 5. Security analysis

The exercise demonstrates why password-protected documents should not be treated as secure simply because a password prompt is present. If an attacker obtains a copy of an encrypted document, password guessing can take place offline. This means the application hosting the document may not be able to detect or rate-limit those attempts.

The successful recovery of all three document passwords indicates that the password choices or protection strength used in the exercise were insufficient to prevent recovery with the available techniques.

### Potential impact

- Unauthorised access to the contents of protected documents.
- Exposure of confidential information if the same password practices are used for operational records.
- Reduced effectiveness of application-level controls when a protected file can be copied and attacked offline.
- Increased impact when document access is combined with other weaknesses, such as the authentication issue documented in the main report.

### Recommendations

- Keep sensitive documents behind server-side authentication and authorisation controls.
- Use strong, unique, randomly generated passwords where document encryption is required.
- Apply modern encryption settings approved by organisational policy.
- Distribute document passwords through a separate, controlled channel.
- Avoid reusing passwords across documents, users or systems.
- Review whether sensitive documents need to be downloadable.
- Keep recovered passwords and other authentication material out of source control and public evidence.

## 6. Relationship to the final assessment

This milestone supports **Finding F-02 — Weak password protection on PDF reports** in the [main M4 assessment report](../README.md).

The next stage of the exercise examined the document metadata and embedded comments for further information that could lead to exposed resources. That work is documented in [M3 — Critical Data Exposure](../3.%20critical-data-exposure/README.md).

---

[Back to the main M4 report](../README.md)
