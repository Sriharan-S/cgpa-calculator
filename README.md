# 🎓 CGPA Calculator

A lightweight, modern CGPA Calculator built for university students. It features a sleek Material Design interface, a robust grade reporting system, and a **fully serverless admin panel** that allows course data to be updated dynamically without touching the code.

**Live Demo:** [cgpa-calculator.sriharan2544.workers.dev](https://cgpa-calculator.sriharan2544.workers.dev)

**Admin Panel:** [cgpa-calculator.sriharan2544.workers.dev/admin](https://cgpa-calculator.sriharan2544.workers.dev/admin.html)

---

## 🚀 Features

### For Students (The Calculator)

* **Dynamic Data:** Automatically fetches the latest semesters and subjects from a central JSON file.
* **Report Generation:** Generates a downloadable, high-quality PNG report card with SGPA and CGPA calculations.
* **Smart Calculation:** Handles credit points, elective subjects, and previous CGPA integration automatically.
* **Responsive UI:** A "Mobile-First" design using Tailwind CSS that works beautifully on phones and desktops.

### For Admins (The Dashboard)

* **No Coding Required:** A graphical interface to add/remove semesters, subjects, and credits.
* **Secure Access:** Protected by a custom password layer.
* **Reordering:** Easily move subjects or semesters up and down with a single click.
* **Instant Updates:** Changes made in the dashboard are instantly pushed to the repository and live for all users.

---

## 🛠️ Technical Architecture

This project uses a **"Serverless CMS"** architecture to maintain dynamic data without a traditional database or backend server.

### 1. The Frontend (Cloudflare Pages)

* **Hosted on:** Cloudflare Pages (connected to GitHub).
* **Files:** `index.html` (Calculator) and `admin.html` (Dashboard).
* **Role:** Serves the UI to the user. `index.html` reads data from `data.json`. `admin.html` sends data to the Worker.

### 2. The Database (GitHub as DB)

* **File:** `data.json` stored in this repository.
* **Role:** Acts as the "Source of Truth". All subject data, credit points, and semester ordering are stored here.
* **Why?** It's free, version-controlled, and instantly available via raw URL.

### 3. The Backend (Cloudflare Worker)

* **Role:** A security proxy between the Admin Panel and GitHub.
* **Function:**
1. Receives the updated JSON from `admin.html`.
2. Verifies the `X-Admin-Password` header.
3. Injects the secure `GITHUB_TOKEN` (stored in Cloudflare Secrets).
4. Commits the changes to `data.json` in the GitHub repo via the GitHub API.



---

## 📖 How to Deploy This

If you want to fork this project and run it yourself, follow these steps:

### Phase 1: The Repository

1. Fork this repository.
2. Ensure you have `index.html`, `admin.html`, and a starting `data.json` file.

### Phase 2: The Backend (Cloudflare Worker)

1. Go to the [Cloudflare Dashboard](https://dash.cloudflare.com/) > **Workers & Pages**.
2. Create a **Worker** (e.g., `cgpa-backend`).
3. Paste the code from `worker.js` (see project files) into the worker editor.
4. Go to **Settings > Variables** and add:
* `ADMIN_PASSWORD` (Secret): Your desired login password.
* `GITHUB_TOKEN` (Secret): A Classic Personal Access Token with `repo` scope.
* `GITHUB_OWNER` (Text): Your GitHub username.
* `GITHUB_REPO` (Text): Your repository name.
* `GITHUB_PATH` (Text): `data.json`.



### Phase 3: The Frontend (Cloudflare Pages)

1. Go to **Workers & Pages** > **Create Application** > **Pages** > **Connect to Git**.
2. Select your repository.
3. **Build Settings:** Leave "Build Command" and "Output Directory" **BLANK**.
4. Deploy.

### Phase 4: Connection

1. Copy your Worker URL (e.g., `https://cgpa-backend.yourname.workers.dev`).
2. Edit `admin.html` in your repo:
```javascript
const WORKER_URL = "https://cgpa-backend.yourname.workers.dev";

```


3. Push the change to GitHub.

---

## 🙋‍♂️ About the Project

**Project Name:** Serverless CGPA Calculator

**Developer:** Sriharan

**Stack:** HTML5, Tailwind CSS, JavaScript (Vanilla), Cloudflare Workers.

This project was born out of a need for a simple, maintainable way to calculate GPA without manually updating HTML files every semester. By leveraging the GitHub API, we turned a static repository into a dynamic Content Management System (CMS), proving that you don't need a heavy database for simple applications.
