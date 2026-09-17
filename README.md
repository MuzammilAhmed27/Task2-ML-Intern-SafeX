# Task2-ML-Intern-SafeX

# AI-Assisted Vulnerability Analysis Tool

**Vulnerability Category:** Broken Access Control (OWASP Top 10 A01:2021)  
**Use Case Scenario:** Tourism & Booking Application  
**Project Role:** Member 4 (Individual Contributor)

---

## Authorized Testing Disclaimer
> **CRITICAL:** This security research tool is developed strictly for **educational and defensive purposes**. It is intended **ONLY** to be executed against authorized, local laboratory environments (e.g., OWASP Juice Shop running on `localhost`). Automated scanning against unauthorized, external, or live production targets is strictly prohibited and illegal. 

---

## Project Overview
This project is an automated, AI-assisted defensive vulnerability scanner built in a **Jupyter Notebook**. It is designed to detect **Broken Access Control** flaws in web applications modeled after a Travel Agency booking system. 

The tool programmatically probes restricted endpoints (e.g., administrative configurations, private booking baskets, and hidden backup directories) to identify access policy violations. It then utilizes AI logic to evaluate the context of the vulnerability, assign a severity rating, and draft plain-English remediation advice for developers.

### Key Features
- **Automated Endpoint Probing:** Simulates unauthenticated requests to restricted API routes to check for unauthorized `200 OK` responses.
- **AI-Driven Analysis:** Processes raw HTTP responses through an AI engine to determine the exact nature of the access control failure.
- **Automated Reporting:** Generates a structured incident report containing the Vulnerability Status, Severity, Evidence, and Recommended Remediation.
- **Reproducible Environment:** Entirely contained within a documented Jupyter Notebook (`vulnerability_analyzer.ipynb`).

---

## Prerequisites & Lab Setup

### 1. Target Environment (OWASP Juice Shop)
You must have OWASP Juice Shop running locally as the authorized target.

```powershell
# Clone the repository or extract the pre-built package
cd juice-shop-master

# Install dependencies and build the server
npm install
npm run build:server

# Start the application
npm start

```

*Verify the target is running by navigating to `http://localhost:3000` in your browser.*

### 2. Analysis Environment (Python & Jupyter)

Ensure Python 3 is installed and added to your system `PATH`.

```powershell
# Navigate to the tool's directory
cd vulnerability-tool

# Install required Python packages
python -m pip install requests pandas google-genai python-dotenv jupyterlab

# Launch Jupyter Lab
python -m jupyter lab

```

---

## Usage & Execution

1. Open `vulnerability_analyzer.ipynb` in the Jupyter interface.
2. **Cell 1 (Configuration):** Initializes the target endpoints (modeled after a Tourism system) and expected authorization levels. Press `Shift + Enter` to run.
3. **Cell 2 (Automated Scanner):** Executes the `requests` library against the local Juice Shop instance (`http://localhost:3000`). It flags endpoints that inappropriately return successful HTTP status codes. Press `Shift + Enter` to run.
4. **Cell 3 (AI Analyzer):** Passes the flagged/suspicious endpoints to the AI engine to generate the security report. Press `Shift + Enter` to run.

---

## 📊 Summary of Findings

During the execution against the authorized local lab, the following vulnerabilities were detected and analyzed:

| Affected Endpoint | Vulnerability Detected | Severity | Evidence | Recommended Remediation |
| --- | --- | --- | --- | --- |
| `/rest/admin/application-configuration` | **YES** | **HIGH** | The endpoint returned a `200 OK` status to an unauthenticated request, exposing administrative JSON metadata. | Implement server-side Role-Based Access Control (RBAC). Ensure the middleware checks for a valid administrative JWT token before routing requests. |
| `/api/BasketItems/1` | **YES** | **MEDIUM** | Allowed retrieval of basket items using a direct ID reference without validating session ownership (IDOR). | Implement object-level authorization checks. The backend controller must verify that `basket.userId == currentUser.id` before returning the payload. |
| `/ftp/coupons_2013.md.bak` | **YES** | **LOW** | The server allows unauthenticated directory access to backup files (`.bak`). | Disable public directory browsing on the web server and move internal backup files outside of the public web root. |

---

## Deliverables Included

* [x] `vulnerability_analyzer.ipynb`: The complete working detection script and notebook.
* [x] `README.md`: Setup instructions and scope documentation.
* [x] Screen-recording video demonstrating the live run against the lab environment.
