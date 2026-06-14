# GitLab Arbitrary File Disclosure Simulation (CVE-2016-9086)

This repository provides a step-by-step simulation and testing environment for **CVE-2016-9086**, a critical path traversal vulnerability found within GitLab's project Import/Export feature. By exploiting this flaw, an authenticated user can read arbitrary host system files accessible to the high-privilege `git` service account.

---

## Vulnerability Overview

* [cite_start]**Vulnerability Type:** Path Traversal / Directory Traversal (CWE-22) [cite: 34]
* [cite_start]**Affected Component:** `Gitlab::ImportExport::Importer` & `Gitlab::ImportExport::FileImporter` [cite: 92]
* [cite_start]**Impacted Versions:** Self-hosted GitLab instances (CE/EE) running versions `8.9.0` through `8.13.2` [cite: 107]
* [cite_start]**Severity:** Critical (Can lead to administrative cookie forgery, database credential theft, and full Remote Code Execution) [cite: 104, 114, 115]

[cite_start]The vulnerability is rooted in an archive decompression flaw[cite: 91, 94]. [cite_start]When extracting a user-supplied `.tar.gz` project migration export [cite: 93][cite_start], the Ruby on Rails backend fails to validate file types in the tarball headers[cite: 94, 101]. [cite_start]If an archive contains a **symbolic link (symlink)** [cite: 93][cite_start], GitLab blindly writes it to the disk[cite: 94, 101]. [cite_start]When the application later attempts to parse or read that file, it follows the symlink out of the temporary extraction sandbox and into the host's root filesystem[cite: 87, 88].

---

## Step-by-Step Execution Guide

### Step 1: Start the Vulnerable Environment
[cite_start]Launch the target environment by executing the following command in your shell or VS Code terminal[cite: 200]:

```bash
docker compose up -d
```

### Step 2: Access and load data
After having set up the environment, access it at ```http://localhost:8080``` import the ```test.tar.gz``` file.

### Important notice

The```load-extension.sh``` file is essential to load the data of the GitLab instance.