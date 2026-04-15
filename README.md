# Digital Forensics Lab Report – Autopsy (macOS via Docker)

## 1. Overview

This report documents the process of performing a basic digital forensic examination using Autopsy. The investigation was conducted on macOS using a Docker-based deployment of Autopsy. The objective was to load and analyze a forensic disk image and to understand the structure and methodology of digital forensic analysis.

---

## 2. Objectives

* Understand the role of forensic disk images in investigations
* Deploy and operate Autopsy in a macOS environment
* Import and analyze an evidence image (.E01 format)
* Perform manual inspection of filesystem artifacts
* Identify and extract embedded or non-obvious data

---

## 3. Environment and Tooling

* Host OS: macOS
* Execution Environment: Docker container
* Tool: Autopsy (web/TSK-based interface)
* Evidence Type: E01 forensic disk image

Docker was configured with filesystem sharing to allow access to the host's `/Users` directory. A volume mount was used to expose the evidence file to the container.

---

## 4. Evidence Acquisition

The evidence file used in this lab was:

* Filename: `nps-2010-emails.E01`
* Location: `/Users/kk/Downloads/`

This file represents a forensic image of a storage medium. It was not modified during the investigation, in accordance with standard forensic practices.

---

## 5. Case Initialization

A new case was created in Autopsy with the following parameters:

* Case Name: `Incident_2026_04_15`
* Description: Basic forensic analysis
* Investigator: `kk`

A host was defined to represent the system under investigation:

* Host Name: `host1`
* Timezone: Default (system-derived)
* Time skew: 0

---

## 6. Image Ingestion

The disk image was imported using the following configuration:

* Data Source Type: Disk Image / VM File
* Import Method: Symlink
* Path (container-visible): `/data/Downloads/nps-2010-emails.E01`

Autopsy successfully parsed the image and identified a partition:

* Partition Type: Win95 FAT32 (0x0b)
* File System Representation: FAT16 (as interpreted by Autopsy)
* Mount Point: `C:/`

The filesystem was then made available for analysis.

---

## 7. Filesystem Analysis

The primary analysis was conducted through the `File Analysis` interface. The root directory (`C:/`) was inspected manually.

### Observations

* The dataset did not reflect a typical operating system layout
* No conventional user directories (e.g., `/Users`, `/Documents`) were present
* The filesystem contained standalone files rather than structured user data

### Identified Artifacts

The following file types were identified:

* Text files (`.txt`)
* PDF documents (`.pdf`)
* Office documents (`.docx`)
* Archive file (`.zip`)

System-related directories were also present:

* `.Trashes`
* `.fseventsd`

These were excluded from further analysis due to low evidentiary value.

---

## 8. Embedded Data Investigation

A ZIP archive (`myfile.zip`) was identified as a potentially relevant artifact.

### Analysis Approach

* The archive contents were not directly parsed by the interface
* The file was manually extracted outside Autopsy
* The extracted file (`myfile.txt`) was inspected for readable content

### Rationale

In forensic workflows, archives are commonly used to conceal or bundle data. Manual extraction is required when tooling does not automatically parse embedded content.

---

## 9. Analytical Workflow

The investigation followed a structured approach:

1. Load and verify the forensic image
2. Identify partitions and mount filesystems
3. Perform directory-level inspection
4. Identify non-standard or suspicious file types
5. Extract and analyze embedded data

Additional analysis features such as keyword search and file type categorization were identified but not fully utilized due to the minimal dataset.

---

## 10. Key Learnings

### Technical

* Forensic disk images represent complete snapshots of storage media
* Autopsy (TSK-based interface) requires manual navigation and analysis
* Containerized environments require explicit filesystem mapping
* Filesystem interpretation may differ from original formats (e.g., FAT32 vs FAT16 representation)

### Methodological

* Forensic analysis is hypothesis-driven rather than exploratory interaction
* Not all datasets resemble real-world systems; some are intentionally minimal
* Evidence may be embedded within archives or nested structures
* System artifacts should be filtered to prioritize user-generated data

### Operational

* Docker volume configuration is critical for evidence accessibility
* Symlink-based ingestion preserves original evidence integrity
* Manual extraction is sometimes required for unsupported file types

---

## 11. Limitations

* The dataset was artificial and lacked realistic user behavior patterns
* No timeline or metadata correlation was performed
* Email-specific artifacts were not explicitly identified despite dataset naming
* Automated ingest modules were not available in the web-based interface

---

## 12. Conclusion

This lab demonstrates the foundational workflow of digital forensic analysis using Autopsy. The process emphasized controlled evidence handling, manual inspection of filesystem structures, and identification of potentially hidden data within archives.

The exercise highlights the importance of structured investigation methodology and reinforces the need for critical analysis over tool-driven automation.

---

## 13. Future Work

* Perform keyword-based searches across the dataset
* Analyze file metadata and timestamps in detail
* Use a more realistic dataset with user activity
* Compare results with full Autopsy GUI (non-web version)
* Integrate timeline reconstruction and artifact correlation
