# M3 | Critical Data Exposure and Metadata Review

This folder contains the evidence for Milestone 3 of the NetworkWalks Week 4 exercise. The investigation followed a clue identified during the review of the three PDF reports and led to a legacy location associated with database-related material.

> **Exercise context:** This is a fictional training scenario. The screenshots are included with the tutor's approval and are mapped below to the activities they evidence.

## 1. Objective

Review the properties and embedded information in the exercise documents, identify the additional location indicated by the document evidence, and establish whether database-related or other sensitive information was exposed.

## 2. Investigation summary

The investigation involved:

1. Reviewing PDF metadata and document properties using ExifTool.
2. Examining readable strings and embedded comments in the supplied PDFs.
3. Identifying a comment/metadata clue in one of the documents.
4. Following the clue to an old or legacy location associated with database-related material.
5. Reviewing the exercise evidence for staff and shareholder information at that location.

## 3. Screenshot evidence map

### E-22: PDF metadata inspection

ExifTool was used to inspect document metadata and identify properties that could provide useful context about the files.

![E-22 - ExifTool metadata output](1.%20cde%20exiftool%20metadata.png)

### E-23: Hidden comment identified in document strings

The strings review surfaced a comment embedded in one of the files. The comment provided a clue for the next stage of the investigation.

![E-23 - Strings output showing the embedded comment clue](2.%20cde%20strings%20shows%20that%20there's%20a%20comment%20on%20one%20of%20the%20files%20thats%20hidden.png)

### E-24: Processing and decrypting the three exercise files

The screenshot records the processing/decryption of all three supplied files, enabling their contents to be reviewed as part of the exercise.

![E-24 - Decryption of all three files](3.%20cde%20decrypt%20all%203%20files.png)

### E-25: Legacy database-backup clue

The investigation identified evidence that a database backup had been moved to an old location before a migration. This established the legacy location as a relevant area for further review.

![E-25 - Database backup moved to an old location before migration](4.%20cde%20shows%20db%20backup%20moved%20to%20old%20before%20migration.png)

### E-26: Review of the old domain/location

The next screenshot records checking the old domain/location and identifying data associated with it.

![E-26 - Checking the old domain and identifying data](5.%20cde%20checking%20the%20domain%20name%20of%20old%20and%20finding%20data.png)

### E-27: Staff information discovered

The exercise evidence records the discovery of staff information in the legacy location.

![E-27 - Staff information discovered](6.%20cde%20finding%20staff%20information.png)

### E-28: Shareholder information discovered

The final screenshot records the discovery of shareholder information, completing the evidence trail for this milestone.

![E-28 - Shareholder information discovered](7.%20cde%20finding%20shareholder%20information.png)

## 4. Outcome

The evidence trail progresses from document metadata and an embedded comment, to a legacy database-backup clue, and then to the discovery of staff and shareholder information associated with the old location.

The exact exposure mechanism should be described precisely in the tutor-facing version of the report using the original evidence. The available milestone notes do not establish whether the material was exposed through directory listing, a directly accessible backup file, an old application route or another configuration issue. This README therefore describes it as a legacy-location and sensitive-information exposure rather than assigning an unverified technical root cause.

See finding F-03 in the [main M4 report](../README.md) for the consolidated finding, impact and prioritised recommendations.

## 5. Security implications

Legacy directories, migration artefacts and database backups can unintentionally remain accessible after systems are moved or updated. If access controls are not applied consistently, these resources can expose business records and information that should only be available to authorised personnel.

The exposure of staff and shareholder information may create privacy, financial, regulatory and reputational consequences. If credentials or database exports are present in the exposed material, the potential impact may be broader.

## 6. Recommended remediation

- Remove database backups and exports from public web directories.
- Store backups in a separate, access-controlled storage location.
- Disable directory listing and explicitly deny public access to backup, migration and archive paths.
- Review old domains, aliases, virtual hosts and legacy directories during decommissioning.
- Apply least-privilege permissions to backup files and database exports.
- Rotate any credentials that may have been included in exposed files.
- Review server and application logs for access to the affected location.
- Remove unnecessary metadata and embedded comments before distributing documents.
- Establish backup retention, access review and secure-deletion procedures.

## 7. Evidence handling

The screenshots form part of the fictional exercise evidence and are included with the tutor's approval. Treat any names, financial details or record contents shown in them as exercise data, and avoid presenting them as real individuals or real organisational records.

Return to the [main M4 report](../README.md).
