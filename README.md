# Medical Shop — PharmaSpot

Pharmacy point-of-sale application with separate frontend and backend folders
in one repository. Based on the supplied PharmaSpot 1.5.3 source; original MIT
licenses and attribution are retained in both folders.

| Folder | Technology | Purpose |
| --- | --- | --- |
| `frontend/` | Electron, HTML/CSS, JavaScript, jQuery, Bootstrap, Gulp, Jest | Desktop POS client |
| `backend/` | Node.js, Express 5, NeDB, Multer, bcryptjs | Standalone HTTP API and storage |

The frontend remains an Electron desktop application. It is not a browser-only
website. The backend runs independently and does not import Electron.

## Start

Use Node.js 22 or newer. In one terminal:

```bash
cd backend
npm ci
npm start
```

In a second terminal on a machine with a graphical desktop:

```bash
cd frontend
npm ci
npm run build
npm start
```

The client defaults to `http://127.0.0.1:3210/api/`. Set
`PHARMASPOT_API_URL` before starting the client to use another backend.
Set `HOST`, `PORT`, and `DATA_DIR` in the backend `.env` if required.
See each folder's README for configuration, maintenance, and known limitations.

## Validation

```bash
npm test --prefix frontend
npm test --prefix backend
npm run build --prefix frontend
```

21 frontend tests and 8 backend tests passed during the separation; the
frontend asset build passed. Integration tests cover API access, persistence,
product/image uploads, login, server lifecycle, and offline backup/restore.
Graphical Electron launch, physical receipt printing, and installer packaging
were not verified in this workspace.

## Changes made during separation

- Removed the embedded server from the desktop client.
- Made backend storage configurable without Electron.
- Fixed the Express 5 startup wildcard incompatibility.
- Connected upload validation to backend-owned code.
- Served product images over HTTP for remote clients.
- Fixed default-user initialization so requests complete.
- Moved backup/restore to offline commands on the backend host.
- Added package lockfiles, tests, and CI for both packages.

## Inherited limitations

Legacy API routes still need server-side authentication and permissions;
user responses include password hashes; checkout trusts client values and
transaction/stock writes are not atomic. The service binds to loopback by
default. These inherited issues need resolution before public production use.
The original `admin/admin` bootstrap login is retained for development.
