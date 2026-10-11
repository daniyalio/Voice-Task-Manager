# Voice Task Manager

A simple task manager built with React, Express, PostgreSQL, and browser speech recognition.

You can manage tasks by typing or by speaking commands such as:

- `add exercise`
- `complete exercise`
- `mark exercise as done`
- `delete exercise`

## Project structure

```text
Task Manager/
├── backend/
│   ├── package.json
│   ├── package-lock.json
│   └── server.js
├── frontend/
│   ├── package.json
│   ├── package-lock.json
│   └── src/
└── README.md
```

## Requirements

Install these before starting the project:

- Node.js and npm
- PostgreSQL
- Google Chrome or Microsoft Edge for voice recognition
- Git, if cloning from GitHub

Chrome and Edge provide the best support for the browser speech-recognition API. The browser must be allowed to use the microphone.

## Clone the project

Replace the URL with the repository URL:

```powershell
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd "Task Manager"
```

## Set up PostgreSQL

The backend expects a PostgreSQL database named `task_manager` with a table named `tasks`.

### Create the database

Using `psql`, run:

```sql
CREATE DATABASE task_manager;
```

Connect to that database and create the table:

```sql
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  title TEXT NOT NULL,
  completed BOOLEAN NOT NULL DEFAULT FALSE
);
```

You can also add a sample task:

```sql
INSERT INTO tasks (title) VALUES ('Try voice commands');
```

## Configure the backend database connection

The current connection settings are in `backend/server.js`:

```js
user: "postgres",
host: "localhost",
database: "task_manager",
password: "YOUR_POSTGRES_PASSWORD",
port: 5432
```

Update the `password` value to the password for the local PostgreSQL user. If your PostgreSQL username, database name, host, or port is different, update those values too.

Do not commit real passwords to GitHub. For a public or production project, move these values to environment variables before deploying.

## Install and start the backend

Open a terminal in the project root and run:

```powershell
cd backend
npm install
node server.js
```

The backend should display:

```text
Server running on http://localhost:5000
```

Keep this terminal running.

The backend provides these endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/` | Check that the backend is running |
| GET | `/tasks` | Load all tasks |
| POST | `/tasks` | Create a task |
| PATCH | `/tasks/:id` | Toggle or explicitly set completion |
| DELETE | `/tasks/:id` | Delete one task |

## Install and start the frontend

Open a second terminal in the project root and run:

```powershell
cd frontend
npm install
npm run dev
```

Vite will display a local URL, normally:

```text
http://localhost:5173
```

Open that URL in Chrome or Edge.

## Using voice commands

Click **Start automatic voice**. The app will continue listening and will restart recognition after each phrase. Click **Stop automatic voice** when you are finished.

Supported commands:

| Say this | Result |
|---|---|
| `add exercise` | Creates a task named `exercise` |
| `create task buy groceries` | Creates a task named `buy groceries` |
| `complete exercise` | Completes the exact matching task |
| `finish exercise` | Completes the exact matching task |
| `mark exercise as done` | Completes the exact matching task |
| `delete exercise` | Deletes every task named `exercise` |
| `remove exercise` | Deletes every task named `exercise` |

Task matching ignores capitalization and spaces at the beginning or end of a title.

If more than one task has the same title, completing that title displays choices so you can select the correct task. Deleting a title deletes all exact matches.

## Troubleshooting

### The task list does not load

Check that:

1. PostgreSQL is running.
2. The `task_manager` database exists.
3. The `tasks` table exists.
4. The password and other connection settings in `backend/server.js` are correct.
5. The backend terminal is running on port 5000.

### The browser cannot reach the backend

Make sure the frontend is opened at `http://localhost:5173` and the backend is running at `http://localhost:5000`. The backend currently allows requests from the Vite development URL only.

### Voice input does not work

- Use Chrome or Edge.
- Allow microphone access when the browser asks.
- Use `http://localhost`, not an arbitrary insecure network address.
- Check that the computer microphone is working.
- Check the browser console for a speech-recognition error.

### The port is already in use

Stop the other process using port 5000 or 5173, then start the corresponding server again. If you change the backend port, also update `API_URL` in `frontend/src/App.jsx` and the CORS setting in `backend/server.js`.

## Development checks

Run the frontend lint and production build with:

```powershell
cd frontend
npm run lint
npm run build
```

Check backend JavaScript syntax with:

```powershell
cd backend
node --check server.js
```

## Security note

The current project is configured for local development. Before making the repository public or deploying it:

- Remove database passwords from source code.
- Use environment variables and provide a `.env.example` file.
- Add `.env` to `.gitignore`.
- Configure CORS for the real frontend URL.
- Use HTTPS for deployed voice input and API requests.
- Use a separate database user with limited permissions.
