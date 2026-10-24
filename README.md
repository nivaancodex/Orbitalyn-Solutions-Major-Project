# Orbitalyn Solutions --- Cybersecurity Major Project

**Student:** Ankit Kumar\
**ERP ID:** 6606502\
**Branch:** B.Tech CSE --- Cybersecurity\
**Assigned Enterprise:** Orbitalyn Solutions\
**Lab Platform:** TryHackMe\
**Lab used for documented practical evidence:** Kenobi

> **Scope note:** This README documents the Kenobi lab workflow
> supported by the current project notes: reconnaissance, SMB
> enumeration, ProFTPD `mod_copy` investigation, NFS/SSH-key access, SSH
> login, and SUID/PATH-based privilege-escalation investigation. Do not
> claim that a separate web-upload bypass, PHP web shell, Meterpreter
> session, or web-user-to-root chain was completed unless verified
> evidence is added.

## Project Overview

This repository organizes a cybersecurity practical exercise performed
in an authorized TryHackMe lab. It documents the workflow, command
patterns, evidence, findings, and defensive recommendations.

## Objectives

-   Identify exposed services on the authorized lab target.
-   Enumerate SMB resources and review permitted files.
-   Investigate ProFTPD `mod_copy` and its security implications.
-   Review NFS exposure and SSH-key access in the lab.
-   Investigate SUID binaries and `PATH` handling.
-   Document evidence and recommend defensive mitigations.

## Environment and Tools

-   **Nmap** --- port and service discovery.
-   **SMB client tools** --- SMB share enumeration and access.
-   **SSH** --- remote login in the lab.
-   **NFS utilities** --- checking exports and mounts.
-   **Linux shell utilities** --- file permissions, SUID binaries, and
    `PATH`.
-   **TryHackMe** --- authorized training environment.

## Workflow and Commands

Replace `<TARGET_IP>` with the IP assigned by your active TryHackMe
room. Run commands only against systems you own or are explicitly
authorized to test.

### 1. Reconnaissance --- Nmap

``` bash
nmap -sC -sV <TARGET_IP>
```

**Purpose:** Identify open ports, service versions, and results from
Nmap's default scripts.

**Evidence to record:** Target IP (redact before public release if
needed), scan date, open ports, service names/versions, and
screenshot/output.

### 2. SMB Enumeration

``` bash
smbclient -L //<TARGET_IP>/ -N
```

If a share is listed and accessible in your lab:

``` bash
smbclient //<TARGET_IP>/<SHARE_NAME> -N
```

**Purpose:** Enumerate SMB shares and inspect permitted files. These are
representative command patterns; use your terminal history/screenshots
to confirm the exact commands and share names you used.

### 3. ProFTPD `mod_copy` Investigation

The project notes indicate investigation of ProFTPD `mod_copy` and
file-copy behavior. The exact command and paths are not established in
this README, so copy them from verified terminal history/screenshots
rather than guessing.

**Evidence to record:** Service identification, exact authorized lab
command, source/destination paths, output, and screenshot.

### 4. NFS and SSH-Key Access

The available project notes indicate NFS/SSH-key retrieval followed by
SSH access as user `kenobi`. Confirm the precise export path, mount
command, key filename, and permissions from your original evidence.

Typical SSH command pattern:

``` bash
ssh kenobi@<TARGET_IP>
```

**Never commit a private SSH key.** Store only sanitized screenshots and
describe the access method without publishing credentials.

### 5. SUID and PATH Investigation

The notes indicate investigation of a SUID-enabled menu binary and a
`PATH`-related privilege-escalation path. Confirm the binary name, exact
commands, and output from verified evidence.

**Purpose:** Understand why privileged programs must not trust unsafe
executable search paths and why SUID binaries need careful auditing.

**Evidence to record:** Binary path, owner/permissions, relevant
behavior, controlled lab test, and proof screenshot. Do not add
unverified commands or results.

## Screenshots and Evidence

Suggested repository layout (rename these examples to match the files
you actually add):

``` text
screenshots/
├── 01-nmap-scan.png
├── 02-smb-enumeration.png
├── 03-proftpd-mod-copy.png
├── 04-nfs-or-ssh-key-evidence.png
├── 05-ssh-login.png
└── 06-suid-path-investigation.png
```

Example Markdown reference:

``` markdown
![Nmap scan evidence](screenshots/01-nmap-scan.png)
```

These are suggested filenames, not confirmation that files with those
exact names already exist. Add a caption to every screenshot and ensure
it proves the claim being made.

## Findings Summary

Based on the documented lab workflow:

1.  **Service exposure:** Network services can reveal useful
    attack-surface information.
2.  **SMB access:** Anonymous or overly broad share permissions may
    expose files.
3.  **File-copy features:** FTP modules such as `mod_copy` need careful
    configuration and access restrictions.
4.  **NFS/SSH access:** Weak export restrictions or unsafe key handling
    may enable unintended user access.
5.  **SUID/PATH risks:** Privileged programs that invoke commands
    unsafely may create privilege-escalation opportunities.

These are security lessons from the lab workflow, not a claim that these
issues were found on any production Orbitalyn Solutions system.

## Defensive Recommendations

-   Disable unnecessary services and restrict required services to
    trusted networks.
-   Patch and securely configure FTP services; disable unnecessary
    modules/features.
-   Restrict SMB shares and disable anonymous access where it is not
    required.
-   Limit NFS exports to approved hosts and least-privilege permissions.
-   Protect SSH private keys with correct permissions and rotate exposed
    keys.
-   Audit SUID/SGID binaries and remove unnecessary privileged bits.
-   Avoid relative executable paths and untrusted `PATH` values in
    privileged programs; use safe absolute paths.
-   Monitor authentication, service access, and privilege changes.
-   Re-test fixes in an authorized environment and retain before/after
    evidence.

## Suggested Repository Structure

``` text
Orbitalyn-Solutions-Major-Project/
├── README.md
├── report/
│   └── project-report.pdf
└── screenshots/
    ├── 01-nmap-scan.png
    ├── 02-smb-enumeration.png
    ├── 03-proftpd-mod-copy.png
    ├── 04-nfs-or-ssh-key-evidence.png
    ├── 05-ssh-login.png
    └── 06-suid-path-investigation.png
```

This is a recommended structure. Add only files that actually exist and
update the names/paths to match your repository. You may also create
`commands/kenobi-commands.md` for a detailed command log; the main
workflow is already summarized here.

## Scope and Evidence Limitations

The current evidence summary does **not** by itself prove completion of
separate assignment steps involving a web upload bypass, PHP web shell,
Meterpreter session, or a web-server-user-to-root chain. If those are
mandatory in the assigned brief, perform them only in the explicitly
authorized lab and add verified screenshots/output before marking them
complete.

Do not publish real credentials, private SSH keys, tokens, or sensitive
target details. Replace target-specific values with placeholders before
making the repository public.

## References

-   TryHackMe --- Kenobi room: https://tryhackme.com/room/kenobi
-   Nmap Reference Guide: https://nmap.org/book/man.html
-   OpenSSH manual pages: https://www.openssh.com/manual.html
-   ProFTPD documentation: http://www.proftpd.org/docs/

## Disclaimer

For educational use and authorized cybersecurity testing only. Keep all
practical work within the assigned lab and written permission. Do not
use these techniques against systems without explicit authorization.
