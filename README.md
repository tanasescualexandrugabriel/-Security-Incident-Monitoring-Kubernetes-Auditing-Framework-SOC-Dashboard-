# 🛡️ Security Incident Monitoring & Kubernetes Auditing Framework (SOC Dashboard)

This project that I've created is a practical web application inspired by the day-to-day operations of a **SOC (Security Operations Center)**. Its primary role is to aggregate security alerts from multiple sources (such as brute-force password attempts in Linux or configuration flaws in Kubernetes), save them into a MySQL database, and display them securely on an interactive web dashboard.

The project is built defensively with an automated fallback mechanism: if an employer reviews this project directly on **GitHub** (without having XAMPP running or the local database configured on their machine), the web page will instantly detect this. It will automatically switch to **Demo Mode**, loading realistic test data saved directly within the code so that the employer can experience a fully functional interface immediately without encountering broken paths or errors.

---

## 📦 How to Download and Extract the Project Files
To prevent nested directory execution issues and maintain full structural integrity, all project folders and source codes have been securely packaged into a single archive file.

1. **Download:** Click on the `.zip` or `.rar` file located in the root of this repository and select **Download**.
2. **Extraction:** Locate the downloaded file on your computer, right-click, and extract it using an archival tool such as **WinRAR** or **7-Zip**.
3. **Open:** Open the newly extracted `cybersecurity-dashboard` folder inside **Visual Studio Code** to view the clean workspace.

> 🛡️ **SECURITY NOTICE:** The packaged archive has been thoroughly inspected, compiled in an isolated sandbox enviornment, and **fully scanned for threats**. It is **100% clean and safe** to download and execute on any host machine.

---

## 📂 Project Structure Made Simple

Once extracted, the project is organized into 5 core folders. Each folder has a dedicated, well-separated responsibility, mirroring standard development practices used in tech companies:

```text
cybersecurity-dashboard/
│
├── config/                  # Settings and data persistence
│   └── database.sql         # SQL script to initialize tables in MySQL
│
├── backend/                 # Behind-the-scenes layer supplying data to the frontend
│   ├── connect.php          # Establishes a secure and isolated DB connection
│   └── get_alerts.php       # Fetches alerts from MySQL and prepares them as JSON for the web
│
├── scripts/                 # Automated Python programs (Security Tasks)
│   ├── log_parser.py        # Scans Linux logs to detect brute-force password attacks
│   └── k8s_audit.py         # Inspects Kubernetes manifests for structural misconfigurations
│
├── frontend/                # Visible user interface (The SOC Dashboard)
│   ├── index.html           # Structural backbone of the web page
│   ├── app.js               # JavaScript logic handling secure data fetching and the Demo Fallback
│   └── style.css            # Visual layout (Dark cyberpunk hacker terminal theme)
│
├── k8s/                     # Infrastructure as Code folder
│   └── deployment.yaml      # A sample deployment file intentionally left vulnerable for scanning
│
└── README.md                # This technical guide and documentation
```

---

## 🧠 How Each Component Works

### 1. The Database Layer (`config/database.sql`)
*   **What it does:** It creates a dedicated database table named `security_alerts` to centralize all discovered issues. Each alert is logged with a unique ID, its original source, a detailed message, and a severity tier ranging from `LOW` to `CRITICAL` so analysts know what to remediate first.

### 2. The PHP Backend (`backend/`)
*   **`connect.php`:** Connects the application to MySQL. It implements a critical defensive mechanism: if the database goes down, **`mysqli_report(MYSQLI_REPORT_OFF)`** prevents the server from dumping raw system errors (stack traces) onto the screen. This stops malicious actors from gaining clues about your internal hosting environment.
*   **`get_alerts.php`:** Executes a SQL query to fetch the latest alerts in reverse chronological order. Before broadcasting the payload, it wraps values in structural filters to ensure that malicious data payloads are parsed securely across the network.

### 3. The Python Automation Scripts (`scripts/`)
*   **`log_parser.py` (Linux Security Monitoring):** This script acts like a mini SIEM agent. It parses text files where Linux tracks authentication events. Using **Regular Expressions (Regex)**, it isolates the string "Failed password". When triggered, it grabs the attacker's IP and inserts an alert directly into MySQL flagging a high-priority "SSH Brute Force Attack".
*   **`k8s_audit.py` (Kubernetes Compliance):** This script opens the `deployment.yaml` file inside the `k8s` folder and inspects it line by line. It watches out for two massive configuration flaws commonly made by DevOps teams: running containers as `root` (which allows complete container escape capabilities) and establishing `privileged: true` states (which gives a pod full access to the underlying Linux host kernel). Any violation is instantly flagged and written to the database.

### 4. The Frontend Web Interface (`frontend/`)
*   **The Design (`index.html` & `style.css`):** The interface is styled to replicate a real-world security terminal window, using high-contrast tactical mapping (red indicators for critical infrastructure breaches, orange for high risks, and yellow for medium issues).
*   **Frontend Security (`app.js`):** This is the core application security boundary on the client side. When rendering dynamic data fields, the script enforces the use of **`textContent`** instead of the dangerous `innerHTML` method. If an attacker manages to inject a malicious payload into a log file (for example: `<script>malicious_code()</script>`), the browser will refuse to compile it as active code. Instead, it prints it harmlessly as raw text, entirely neutralizing **Cross-Site Scripting (XSS)** vulnerabilities.

---

## 🚀 Installation & Local Deployment Guide (For Employers)

To test the entire automated pipeline end-to-end on your local system after extracting the archive, follow these deployment steps:

### Prerequisites
*   A local web server stack like **XAMPP**, **WampServer**, or an equivalent Docker setup (for Apache and MySQL).
*   **Python 3.x** installed locally.

### Steps to Run
1.  **Hosting the Project:** Copy the entire extracted `cybersecurity-dashboard` directory into your server's root directory (e.g., `C:\xampp\htdocs\` on Windows or `/var/www/html/` on Linux).
2.  **Database Provisioning:** Open `http://localhost/phpmyadmin`, navigate to the SQL tab, copy the contents of `config/database.sql`, and press the Go/Execute button.
3.  **Installing Python Requirements:** Open a terminal window inside VS Code and install the necessary database and file parsers by running:
    ```bash
    pip install mysql-connector-python pyyaml
    ```
4.  **Simulating Security Incidents:** Run the automated scripts from your terminal to scan configurations, parse mock logs, and populate your local database:
    ```bash
    python scripts/log_parser.py
    python scripts/k8s_audit.py
    ```
5.  **Launching the UI:** Access `http://localhost/cybersecurity-dashboard/frontend/index.html` in your web browser. The status badge in the upper right corner will change to green and read `LOCAL BACKEND CONECTAT (MySQL Live)`.

---

## 🔒 Demonstrated Core Security Skills
*   **XSS Mitigation:** Comprehensive client-side protection by rendering untrusted data through `textContent`.
*   **Error Handling Hardening:** Preventing information disclosure vectors in PHP by suppressing technical database errors.
*   **Cloud Infrastructure Auditing:** Static parsing of Kubernetes YAML templates to intercept illegal container privilege escalations.
*   **Log Parsing & Regex Integration:** Building Python ingestion code to detect brute-force threats targeting Linux access paths.
