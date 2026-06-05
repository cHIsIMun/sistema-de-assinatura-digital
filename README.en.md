# sistema-de-assinatura-digital

🇺🇸 English | 🇧🇷 [Português](README.md)

> A Flask web app where registered users sign and verify PDF documents with their own encrypted RSA keys.

## Overview

A web application for **digital document signing**. Each user generates an RSA 2048-bit key pair, uploads PDFs, signs them with their password-protected private key, and verifies signatures — on their own documents or on documents shared by others. It complements [certsim](https://github.com/cHIsIMun/certsim): where certsim teaches the **X.509/PKI infrastructure** at a low level, this app is the **end-user application** for signing.

## What it implements

| Capability | How |
|------------|-----|
| Per-user key pair | RSA 2048-bit |
| Private-key protection | PBKDF2-HMAC-SHA256 (16-byte salt, 100k iterations) + Fernet |
| Signing | RSA-PSS with SHA-256 + MGF1 |
| Verification | RSA-PSS with the public key |
| Document integrity | SHA-256 hash per upload |
| Storage | SQLite via SQLAlchemy |

Signatures are appended to the PDF as JSON metadata (delimited block with the hex signature).

## Stack

Python 3.12 · Flask · SQLAlchemy · flask-login · [cryptography](https://cryptography.io) · Jinja2 · SQLite. Managed with Poetry.

## Running

```bash
poetry install
poetry run python init_db.py
flask run          # http://127.0.0.1:5000
```

Configure via `.env` (see `.env.example`): `SECRET_KEY`, `SQLALCHEMY_DATABASE_URI`.

## Structure

```
app/
├── __init__.py      # Flask app, db, session
├── routes.py        # auth + document routes
├── controllers.py   # business logic (users, keys, documents)
├── models.py        # User, Document (ORM)
├── auth.py          # password hashing
├── crypto.py        # RSA, PBKDF2, Fernet
├── schemas.py       # Pydantic validation
└── templates/       # Jinja2 views
init_db.py
```

## Project status

Beta / prototype. Core flows (sign, verify, upload, download) work. Known issues: no unit tests; minor code duplication in `routes.py`; session-based auth (no JWT); HTTP-only dev setup.

> 🇧🇷 Documentação em português em [README.md](README.md).

## License

This project does not yet declare a license. Until one is added, all rights are reserved by the author.
