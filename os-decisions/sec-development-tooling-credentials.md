---
title: "sec-development-tooling-credentials"
doc-id: "mb-63e4a84b"
updated: "2026-10-23"
---
# Development Tooling Credentials

**Scope:** credentials that belong to MyBizz's development tooling, not to any application: the ledger's `PGPASSWORD`, the memory governor's writer credentials, GitHub push authentication and the Anvil Uplink keys used in testing. Secrets that belong to an application live in that application's Vault.

## Applicable Threat

Development-tooling credentials are real and powerful. A credential leaked from a tooling repository gives access to the ledger, to GBrain's governed memory or to the GitHub account that every repository lives in.

## Security Requirement

Every development-tooling credential is listed, its storage location confirmed and its exposure risk assessed, with the same rigour as application secrets.

## Approved Pattern

The credential is stored in a `.env` file that `.gitignore` excludes from version control, and is read at run time only by the process that needs it.

## Prohibited Pattern

A credential written into a script, or a `.env` file that no `.gitignore` entry excludes.

## Verification Requirements

For each credential, confirm where it is stored, that its file is excluded from every tracked repository (`git status` does not show it), and that it was never committed (`git log` holds no trace of the file).
