# SecureVault – Secure Cloud File Storage

A complete beginner-friendly full-stack project using **HTML, CSS, JavaScript, Node.js, Express, SQLite and AES-256-GCM encryption**.

## Features

- Register and log in with a username and password
- Passwords hashed using bcrypt
- Short-lived JWT authentication
- Upload files through a web interface
- Files encrypted on the server using AES-256-GCM before being written to disk
- Download files (server decrypts them after checking ownership)
- Delete files
- Each user can only list, download, and delete their own files
- SQLite database stores account and file metadata
- Upload size limit (20 MB by default), Helmet security headers and API rate limiting

## Requirements

- Node.js 18 or later (Node.js 20 LTS recommended)
- npm
- A modern web browser

## Run on Windows

1. Extract the ZIP file.
2. Open the extracted `secure-cloud-file-storage` folder in VS Code.
3. Open Terminal → New Terminal.
4. Install packages:

   ```bash
   npm install
   ```

5. Create your environment file:

   ```bash
   copy .env.example .env
   ```

6. Generate a random JWT secret and 32-byte encryption key. Run these commands in the terminal:

   ```bash
   node -e "console.log(require('crypto').randomBytes(48).toString('hex'))"
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

   Copy the first output into `JWT_SECRET` and the second output into `FILE_ENCRYPTION_KEY` in `.env`. Do not include quotation marks.

7. Start the server:

   ```bash
   npm start
   ```

8. Open **http://localhost:3000** in your browser.
9. Select **Create account**, register, log in, and upload a file.

## Project structure

```text
secure-cloud-file-storage/
├── public/
│   ├── index.html       # Frontend page
│   ├── style.css        # Frontend styling
│   └── app.js           # Browser logic and API calls
├── data/                # Created automatically; SQLite database
├── storage/             # Created automatically; encrypted files
├── server.js            # Express backend and REST API
├── package.json
├── .env.example
└── README.md
```

## API endpoints

| Method | Endpoint | Purpose | Login required |
|---|---|---|---|
| POST | `/api/register` | Create an account | No |
| POST | `/api/login` | Get a JWT token | No |
| GET | `/api/files` | List the logged-in user's files | Yes |
| POST | `/api/files` | Upload and encrypt a file | Yes |
| GET | `/api/files/:id/download` | Decrypt and download a file | Yes |
| DELETE | `/api/files/:id` | Delete a file | Yes |

For protected routes, send `Authorization: Bearer <token>`.

## How encryption works

1. Browser sends the selected file over the connection to the backend.
2. Backend creates a random 12-byte initialization vector (IV).
3. Backend encrypts the file using AES-256-GCM and a 256-bit key.
4. The encrypted output is saved in the `storage/` folder. The IV and authentication tag are stored with the encrypted bytes.
5. When the owner downloads the file, the backend verifies ownership and decrypts it.

**Important:** This is a college/demo project, not a production cloud-storage service. For real deployment, use HTTPS, secure HttpOnly cookies, CSRF protection, a managed secrets vault/KMS, backups, malware scanning, audit logs, object storage, and security testing. The server sees the original file during upload; this is server-side encryption, not end-to-end encryption. Never lose or publish your `.env` encryption key: encrypted files cannot be recovered without it. Do not deploy this demo with the example secrets.

## Troubleshooting

- **Missing environment key:** Ensure `.env` exists and `FILE_ENCRYPTION_KEY` is exactly 64 hexadecimal characters.
- **Port already in use:** Change `PORT=3000` in `.env` to another free port, such as `3001`.
- **Install error with better-sqlite3:** Use Node.js 20 LTS and run `npm install` again.
- **File too large:** Change `MAX_FILE_SIZE_MB` in `.env` and restart the server.

## Suggested project title

**Secure Cloud File Storage System Using AES-256 Encryption and User Authentication**
