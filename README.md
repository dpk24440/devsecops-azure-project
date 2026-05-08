Below is a **production-quality `README.md`** you can **copy-paste directly into your Git repository**.
It is **interview-ready**, explains **each pipeline stage**, **why it exists**, and **what risk it mitigates**.

---

# 🚀 DevSecOps CI/CD Pipeline on Azure DevOps

This repository demonstrates a **job-ready DevSecOps pipeline** implemented using **Azure DevOps**, following **shift-left security** and **fail-fast principles**.

The pipeline integrates **security, quality, and delivery** into every stage of the software lifecycle.

---

## 🧠 High-Level Pipeline Flow

```
SAST (Bandit)
→ Dependency Scan (pip-audit)
→ Unit Tests & Coverage
→ Build & Push Docker Image
→ Container Security Scan (Trivy + SARIF)
```

Each stage exists to **reduce risk as early as possible**.

---

## 🔐 Stage 1: SAST – Static Application Security Testing (Bandit)

### What this stage does

* Scans **Python source code**
* Detects insecure coding patterns (e.g. hardcoded secrets, unsafe functions)

### Tool used

* **Bandit**

### Why this stage exists

* Prevents insecure code from ever entering the pipeline
* Catches issues **before dependencies, builds, or deployments**
* Implements **shift-left security**

### What happens if it fails

* Pipeline stops immediately
* No tests, builds, or deployments are executed

### Risk mitigated

* Code-level vulnerabilities
* Insecure development practices

---

## 🔗 Stage 2: Dependency Security Scan (pip-audit)

### What this stage does

* Scans `requirements.txt`
* Checks Python dependencies against known CVEs

### Tool used

* **pip-audit**

### Why this stage exists

* Most modern attacks come from **vulnerable dependencies**
* Protects against **supply-chain attacks**
* Ensures only secure libraries are allowed

### What happens if it fails

* Pipeline fails fast
* Prevents vulnerable dependencies from reaching runtime

### Risk mitigated

* Known CVEs
* End-of-life or unpatched libraries

---

## 🧪 Stage 3: Unit Tests & Code Coverage

### What this stage does

* Runs automated unit tests using `pytest`
* Generates:

  * JUnit test reports
  * Code coverage reports

### Tools used

* **pytest**
* **pytest-cov**

### Why this stage exists

* Ensures application correctness
* Prevents broken or untested code from being packaged
* Provides measurable quality metrics

### Outputs

* Test results (JUnit)
* Coverage reports (Cobertura XML)

### Risk mitigated

* Functional defects
* Regressions
* Untested code paths

---

## 🐳 Stage 4: Build & Push Docker Image (ACR)

### What this stage does

* Builds a Docker image
* Pushes the image to **Azure Container Registry (ACR)**

### Tool used

* **Docker@2 (buildAndPush)**

### Why this stage exists

* Produces a **single immutable artifact**
* Ensures the same image is used for scanning and deployment
* Avoids “works on my machine” issues

### Key design decision

* **Build and push occur in the same job**
* This avoids Azure DevOps agent isolation issues

### Risk mitigated

* Artifact drift
* Inconsistent builds

---

## 🛡️ Stage 5: Container Security Scan (Trivy + SARIF)

### What this stage does

* Scans the container image stored in ACR
* Detects OS and dependency vulnerabilities
* Generates a **SARIF security report**

### Tool used

* **Trivy**

### Why this stage exists

* Containers are the **runtime attack surface**
* Ensures only secure images are deployed
* Enforces security gates at the artifact level

### Output

* `trivy.sarif` file
* Uploaded as **CodeAnalysisLogs**

### Security integration

* SARIF is ingested by **Azure DevOps Advanced Security**
* Findings appear in:

  ```
  Pipelines → Run → Security → Code Analysis
  ```

### Risk mitigated

* Runtime vulnerabilities
* High / Critical CVEs in base images or packages

---

## 📊 SARIF & Azure DevOps Security Tab

### What is SARIF

* **Static Analysis Results Interchange Format**
* Industry-standard format for security findings

### Why SARIF is used

* Tool-agnostic
* Native support in Azure DevOps
* Enables centralized security visibility

### Important note

* Azure DevOps shows SARIF results **only if vulnerabilities exist**
* A clean scan produces **no Security tab findings**

---

## 🔒 Security Philosophy Used

| Principle           | Implementation                      |
| ------------------- | ----------------------------------- |
| Fail fast           | Pipeline stops on security failures |
| Shift left          | Security before build & deploy      |
| Least privilege     | ACR access via service connection   |
| Immutable artifacts | Same image scanned & deployed       |
| Visibility          | SARIF + Azure DevOps Security tab   |

---

## 📄 Resume-Ready Summary

> Built an enterprise-grade DevSecOps CI/CD pipeline on Azure DevOps integrating SAST, dependency scanning, automated testing, container security scanning with Trivy, SARIF reporting, and security gates using Azure Container Registry.


## ✅ Key Takeaway

This pipeline mirrors **real enterprise DevSecOps practices**, not a toy example:

* Security is enforced early
* Artifacts are immutable
* Results are visible and auditable
* Failures are intentional and meaningful

---
Yes — **you can use almost the same DevSecOps tools for Node.js**, with a few **language-specific swaps**.
This is exactly how **real multi-language pipelines** are designed.

---


## 🔁 Python → Node.js Tool Mapping

| Pipeline Stage  | Python Tool | Node.js Tool             | Same Concept? |
| --------------- | ----------- | ------------------------ | ------------- |
| SAST            | Bandit      | **ESLint** / **Semgrep** | ✅             |
| Dependency Scan | pip-audit   | **npm audit**            | ✅             |
| Unit Tests      | pytest      | **Jest** / Mocha         | ✅             |
| Coverage        | pytest-cov  | Jest coverage            | ✅             |
| Container Build | Docker      | Docker                   | ✅             |
| Container Scan  | **Trivy**   | Trivy                    | ✅             |
| SARIF Upload    | SARIF       | SARIF                    | ✅             |

👉 **Trivy stays exactly the same** (language-agnostic).

---
2026-05-07T17:55:33.6783534Z ##[section]Starting: Trivy Security Gate (Fail on High/Critical)
2026-05-07T17:55:33.6788741Z ==============================================================================
2026-05-07T17:55:33.6788847Z Task         : Command line
2026-05-07T17:55:33.6788904Z Description  : Run a command line script using Bash on Linux and macOS and cmd.exe on Windows
2026-05-07T17:55:33.6788995Z Version      : 2.268.0
2026-05-07T17:55:33.6789053Z Author       : Microsoft Corporation
2026-05-07T17:55:33.6789114Z Help         : https://docs.microsoft.com/azure/devops/pipelines/tasks/utility/command-line
2026-05-07T17:55:33.6789202Z ==============================================================================
2026-05-07T17:55:33.7963727Z Generating script.
2026-05-07T17:55:33.7978975Z ========================== Starting Command Output ===========================
2026-05-07T17:55:33.7987652Z [command]/usr/bin/bash --noprofile --norc /home/vsts/work/_temp/caa31b33-a75d-46b7-ad29-3a757324393b.sh
2026-05-07T17:55:34.1217399Z 2026-05-07T17:55:34Z	INFO	[vulndb] Need to update DB
2026-05-07T17:55:34.1225620Z 2026-05-07T17:55:34Z	INFO	[vulndb] Downloading vulnerability DB...
2026-05-07T17:55:34.1226054Z 2026-05-07T17:55:34Z	INFO	[vulndb] Downloading artifact...	repo="mirror.gcr.io/aquasec/trivy-db:2"
2026-05-07T17:55:39.3049676Z 303.23 KiB / 92.09 MiB [>____________________________________________________________] 0.32% ? p/s ?1.26 MiB / 92.09 MiB [>______________________________________________________________] 1.37% ? p/s ?4.69 MiB / 92.09 MiB [--->___________________________________________________________] 5.09% ? p/s ?15.00 MiB / 92.09 MiB [------->________________________________________] 16.29% 24.52 MiB p/s ETA 3s29.17 MiB / 92.09 MiB [--------------->________________________________] 31.68% 24.52 MiB p/s ETA 2s43.00 MiB / 92.09 MiB [---------------------->_________________________] 46.70% 24.52 MiB p/s ETA 2s56.49 MiB / 92.09 MiB [----------------------------->__________________] 61.35% 27.40 MiB p/s ETA 1s71.00 MiB / 92.09 MiB [------------------------------------->__________] 77.10% 27.40 MiB p/s ETA 0s84.01 MiB / 92.09 MiB [------------------------------------------->____] 91.23% 27.40 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 29.46 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 29.46 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 29.46 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 27.56 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 27.56 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 27.56 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 25.78 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 25.78 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 25.78 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 24.12 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 24.12 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 24.12 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [---------------------------------------------->] 100.00% 22.56 MiB p/s ETA 0s92.09 MiB / 92.09 MiB [-------------------------------------------------] 100.00% 21.09 MiB p/s 4.6s2026-05-07T17:55:39Z	INFO	[vulndb] Artifact successfully downloaded	repo="mirror.gcr.io/aquasec/trivy-db:2"
2026-05-07T17:55:39.3816285Z 2026-05-07T17:55:39Z	INFO	[vuln] Vulnerability scanning is enabled
2026-05-07T17:55:39.3817190Z 2026-05-07T17:55:39Z	INFO	[secret] Secret scanning is enabled
2026-05-07T17:55:39.3817558Z 2026-05-07T17:55:39Z	INFO	[secret] If your scanning is slow, please try '--scanners vuln' to disable secret scanning
2026-05-07T17:55:39.3818511Z 2026-05-07T17:55:39Z	INFO	[secret] Please see https://trivy.dev/docs/v0.70/guide/scanner/secret#recommendation for faster secret detection
2026-05-07T17:55:44.8913709Z 2026-05-07T17:55:44Z	INFO	[python] Licenses acquired from one or more METADATA files may be subject to additional terms. Use `--debug` flag to see all affected packages.
2026-05-07T17:55:48.0738552Z 2026-05-07T17:55:48Z	INFO	Detected OS	family="debian" version="13.4"
2026-05-07T17:55:48.0743060Z 2026-05-07T17:55:48Z	INFO	[debian] Detecting vulnerabilities...	os_version="13" pkg_num=87
2026-05-07T17:55:48.0890513Z 2026-05-07T17:55:48Z	INFO	Number of language-specific files	num=1
2026-05-07T17:55:48.0891961Z 2026-05-07T17:55:48Z	INFO	[python-pkg] Detecting vulnerabilities...
2026-05-07T17:55:48.0913745Z 2026-05-07T17:55:48Z	WARN	Using severities from other vendors for some vulnerabilities. Read https://trivy.dev/docs/v0.70/guide/scanner/vulnerability#severity-selection for details.
2026-05-07T17:55:48.1256458Z 2026-05-07T17:55:48Z	INFO	Table result includes only package filenames. Use '--format json' option to get the full path to the package file.
2026-05-07T17:55:48.1283189Z 
2026-05-07T17:55:48.1289436Z Report Summary
2026-05-07T17:55:48.1289913Z 
2026-05-07T17:55:48.1294422Z ┌──────────────────────────────────────────────────────────────────────────────────┬────────────┬─────────────────┬─────────┐
2026-05-07T17:55:48.1295099Z │                                      Target                                      │    Type    │ Vulnerabilities │ Secrets │
2026-05-07T17:55:48.1295858Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1296526Z │ devsecops-app:272 (debian 13.4)                                                  │   debian   │        7        │    -    │
2026-05-07T17:55:48.1297296Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1298383Z │ home/appuser/.local/lib/python3.11/site-packages/annotated_doc-0.0.4.dist-info/- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1299111Z │ METADATA                                                                         │            │                 │         │
2026-05-07T17:55:48.1299782Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1300421Z │ home/appuser/.local/lib/python3.11/site-packages/annotated_types-0.7.0.dist-inf- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1301079Z │ o/METADATA                                                                       │            │                 │         │
2026-05-07T17:55:48.1301804Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1302490Z │ home/appuser/.local/lib/python3.11/site-packages/anyio-4.13.0.dist-info/METADATA │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1303539Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1304228Z │ home/appuser/.local/lib/python3.11/site-packages/certifi-2026.4.22.dist-info/ME- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1304883Z │ TADATA                                                                           │            │                 │         │
2026-05-07T17:55:48.1305596Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1306262Z │ home/appuser/.local/lib/python3.11/site-packages/click-8.3.3.dist-info/METADATA  │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1307175Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1307853Z │ home/appuser/.local/lib/python3.11/site-packages/coverage-7.13.5.dist-info/META- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1308934Z │ DATA                                                                             │            │                 │         │
2026-05-07T17:55:48.1309663Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1310337Z │ home/appuser/.local/lib/python3.11/site-packages/fastapi-0.136.1.dist-info/META- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1310947Z │ DATA                                                                             │            │                 │         │
2026-05-07T17:55:48.1311616Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1312234Z │ home/appuser/.local/lib/python3.11/site-packages/h11-0.16.0.dist-info/METADATA   │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1312989Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1313624Z │ home/appuser/.local/lib/python3.11/site-packages/httpcore-1.0.9.dist-info/METAD- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1314233Z │ ATA                                                                              │            │                 │         │
2026-05-07T17:55:48.1314903Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1315539Z │ home/appuser/.local/lib/python3.11/site-packages/httpx-0.28.1.dist-info/METADATA │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1316326Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1317013Z │ home/appuser/.local/lib/python3.11/site-packages/idna-3.13.dist-info/METADATA    │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1318293Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1319017Z │ home/appuser/.local/lib/python3.11/site-packages/iniconfig-2.3.0.dist-info/META- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1319665Z │ DATA                                                                             │            │                 │         │
2026-05-07T17:55:48.1320550Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1321270Z │ home/appuser/.local/lib/python3.11/site-packages/packaging-26.2.dist-info/METAD- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1321906Z │ ATA                                                                              │            │                 │         │
2026-05-07T17:55:48.1322608Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1323319Z │ home/appuser/.local/lib/python3.11/site-packages/pluggy-1.6.0.dist-info/METADATA │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1324100Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1324776Z │ home/appuser/.local/lib/python3.11/site-packages/pydantic-2.13.4.dist-info/META- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1325405Z │ DATA                                                                             │            │                 │         │
2026-05-07T17:55:48.1326178Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1326976Z │ home/appuser/.local/lib/python3.11/site-packages/pydantic_core-2.46.4.dist-info- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1327844Z │ /METADATA                                                                        │            │                 │         │
2026-05-07T17:55:48.1329106Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1329829Z │ home/appuser/.local/lib/python3.11/site-packages/pygments-2.20.0.dist-info/META- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1330795Z │ DATA                                                                             │            │                 │         │
2026-05-07T17:55:48.1333273Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1334976Z │ home/appuser/.local/lib/python3.11/site-packages/pytest-9.0.3.dist-info/METADATA │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1336166Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1340057Z │ home/appuser/.local/lib/python3.11/site-packages/pytest_cov-7.1.0.dist-info/MET- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1341670Z │ ADATA                                                                            │            │                 │         │
2026-05-07T17:55:48.1351146Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1352167Z │ home/appuser/.local/lib/python3.11/site-packages/starlette-1.0.0.dist-info/META- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1352883Z │ DATA                                                                             │            │                 │         │
2026-05-07T17:55:48.1353698Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1354503Z │ home/appuser/.local/lib/python3.11/site-packages/typing_extensions-4.15.0.dist-- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1355210Z │ info/METADATA                                                                    │            │                 │         │
2026-05-07T17:55:48.1355972Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1356746Z │ home/appuser/.local/lib/python3.11/site-packages/typing_inspection-0.4.2.dist-i- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1357464Z │ nfo/METADATA                                                                     │            │                 │         │
2026-05-07T17:55:48.1359938Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1360694Z │ home/appuser/.local/lib/python3.11/site-packages/uvicorn-0.46.0.dist-info/METAD- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1361749Z │ ATA                                                                              │            │                 │         │
2026-05-07T17:55:48.1362460Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1363109Z │ usr/local/lib/python3.11/site-packages/pip-24.0.dist-info/METADATA               │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1363874Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1364524Z │ usr/local/lib/python3.11/site-packages/setuptools-79.0.1.dist-info/METADATA      │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1365381Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1366009Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/autocommand-2.2.2.dis- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1366652Z │ t-info/METADATA                                                                  │            │                 │         │
2026-05-07T17:55:48.1367335Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1368325Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/backports.tarfile-1.2- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1369004Z │ .0.dist-info/METADATA                                                            │            │                 │         │
2026-05-07T17:55:48.1369733Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1370448Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/importlib_metadata-8.- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1372294Z │ 0.0.dist-info/METADATA                                                           │            │                 │         │
2026-05-07T17:55:48.1372998Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1373646Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/inflect-7.3.1.dist-in- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1374292Z │ fo/METADATA                                                                      │            │                 │         │
2026-05-07T17:55:48.1375066Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1375750Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/jaraco.collections-5.- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1376415Z │ 1.0.dist-info/METADATA                                                           │            │                 │         │
2026-05-07T17:55:48.1377167Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1377843Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/jaraco.context-5.3.0.- │ python-pkg │        1        │    -    │
2026-05-07T17:55:48.1381221Z │ dist-info/METADATA                                                               │            │                 │         │
2026-05-07T17:55:48.1382429Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1383608Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/jaraco.functools-4.0.- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1384582Z │ 1.dist-info/METADATA                                                             │            │                 │         │
2026-05-07T17:55:48.1385536Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1386397Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/jaraco.text-3.12.1.di- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1387029Z │ st-info/METADATA                                                                 │            │                 │         │
2026-05-07T17:55:48.1387886Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1389151Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/more_itertools-10.3.0- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1390052Z │ .dist-info/METADATA                                                              │            │                 │         │
2026-05-07T17:55:48.1390811Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1396566Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/packaging-24.2.dist-i- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1397560Z │ nfo/METADATA                                                                     │            │                 │         │
2026-05-07T17:55:48.1398487Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1399170Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/platformdirs-4.2.2.di- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1400026Z │ st-info/METADATA                                                                 │            │                 │         │
2026-05-07T17:55:48.1400619Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1401594Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/tomli-2.0.1.dist-info- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1402503Z │ /METADATA                                                                        │            │                 │         │
2026-05-07T17:55:48.1403335Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1403893Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/typeguard-4.3.0.dist-- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1404824Z │ info/METADATA                                                                    │            │                 │         │
2026-05-07T17:55:48.1405397Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1406148Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/typing_extensions-4.1- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1406879Z │ 2.2.dist-info/METADATA                                                           │            │                 │         │
2026-05-07T17:55:48.1407430Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1408229Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/wheel-0.45.1.dist-inf- │ python-pkg │        1        │    -    │
2026-05-07T17:55:48.1416572Z │ o/METADATA                                                                       │            │                 │         │
2026-05-07T17:55:48.1417233Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1418063Z │ usr/local/lib/python3.11/site-packages/setuptools/_vendor/zipp-3.19.2.dist-info- │ python-pkg │        0        │    -    │
2026-05-07T17:55:48.1418562Z │ /METADATA                                                                        │            │                 │         │
2026-05-07T17:55:48.1419781Z ├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
2026-05-07T17:55:48.1420299Z │ usr/local/lib/python3.11/site-packages/wheel-0.45.1.dist-info/METADATA           │ python-pkg │        1        │    -    │
2026-05-07T17:55:48.1421018Z └──────────────────────────────────────────────────────────────────────────────────┴────────────┴─────────────────┴─────────┘
2026-05-07T17:55:48.1421301Z Legend:
2026-05-07T17:55:48.1421535Z - '-': Not scanned
2026-05-07T17:55:48.1421804Z - '0': Clean (no security findings detected)
2026-05-07T17:55:48.1421932Z 
2026-05-07T17:55:48.1422014Z 
2026-05-07T17:55:48.1422269Z devsecops-app:272 (debian 13.4)
2026-05-07T17:55:48.1422544Z ===============================
2026-05-07T17:55:48.1422812Z Total: 7 (HIGH: 7, CRITICAL: 0)
2026-05-07T17:55:48.1422915Z 
2026-05-07T17:55:48.1423521Z ┌──────────────┬────────────────┬──────────┬──────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
2026-05-07T17:55:48.1424100Z │   Library    │ Vulnerability  │ Severity │  Status  │ Installed Version │ Fixed Version │                            Title                            │
2026-05-07T17:55:48.1424785Z ├──────────────┼────────────────┼──────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1425391Z │ libcap2      │ CVE-2026-4878  │ HIGH     │ affected │ 1:2.75-10+b8      │               │ libcap: libcap: Privilege escalation via TOCTOU race        │
2026-05-07T17:55:48.1425945Z │              │                │          │          │                   │               │ condition in cap_set_file()                                 │
2026-05-07T17:55:48.1426385Z │              │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2026-4878                   │
2026-05-07T17:55:48.1427209Z ├──────────────┼────────────────┤          │          ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1427775Z │ libncursesw6 │ CVE-2025-69720 │          │          │ 6.5+20250216-2    │               │ ncurses: ncurses: Buffer overflow vulnerability may lead to │
2026-05-07T17:55:48.1428939Z │              │                │          │          │                   │               │ arbitrary code execution.                                   │
2026-05-07T17:55:48.1429434Z │              │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2025-69720                  │
2026-05-07T17:55:48.1430049Z ├──────────────┼────────────────┤          │          ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1430605Z │ libsystemd0  │ CVE-2026-29111 │          │          │ 257.9-1~deb13u1   │               │ systemd: systemd: Arbitrary code execution or Denial of     │
2026-05-07T17:55:48.1431176Z │              │                │          │          │                   │               │ Service via spurious IPC...                                 │
2026-05-07T17:55:48.1431677Z │              │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2026-29111                  │
2026-05-07T17:55:48.1432305Z ├──────────────┼────────────────┤          │          ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1432872Z │ libtinfo6    │ CVE-2025-69720 │          │          │ 6.5+20250216-2    │               │ ncurses: ncurses: Buffer overflow vulnerability may lead to │
2026-05-07T17:55:48.1433411Z │              │                │          │          │                   │               │ arbitrary code execution.                                   │
2026-05-07T17:55:48.1434115Z │              │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2025-69720                  │
2026-05-07T17:55:48.1434722Z ├──────────────┼────────────────┤          │          ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1435255Z │ libudev1     │ CVE-2026-29111 │          │          │ 257.9-1~deb13u1   │               │ systemd: systemd: Arbitrary code execution or Denial of     │
2026-05-07T17:55:48.1435773Z │              │                │          │          │                   │               │ Service via spurious IPC...                                 │
2026-05-07T17:55:48.1436221Z │              │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2026-29111                  │
2026-05-07T17:55:48.1436811Z ├──────────────┼────────────────┤          │          ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1437529Z │ ncurses-base │ CVE-2025-69720 │          │          │ 6.5+20250216-2    │               │ ncurses: ncurses: Buffer overflow vulnerability may lead to │
2026-05-07T17:55:48.1438396Z │              │                │          │          │                   │               │ arbitrary code execution.                                   │
2026-05-07T17:55:48.1438913Z │              │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2025-69720                  │
2026-05-07T17:55:48.1439457Z ├──────────────┤                │          │          │                   ├───────────────┤                                                             │
2026-05-07T17:55:48.1439929Z │ ncurses-bin  │                │          │          │                   │               │                                                             │
2026-05-07T17:55:48.1440388Z │              │                │          │          │                   │               │                                                             │
2026-05-07T17:55:48.1440904Z │              │                │          │          │                   │               │                                                             │
2026-05-07T17:55:48.1441559Z └──────────────┴────────────────┴──────────┴──────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────────────┘
2026-05-07T17:55:48.1441721Z 
2026-05-07T17:55:48.1441987Z Python (python-pkg)
2026-05-07T17:55:48.1442243Z ===================
2026-05-07T17:55:48.1442507Z Total: 3 (HIGH: 3, CRITICAL: 0)
2026-05-07T17:55:48.1442618Z 
2026-05-07T17:55:48.1443726Z ┌───────────────────────────┬────────────────┬──────────┬────────┬───────────────────┬───────────────┬──────────────────────────────────────────────────────────────┐
2026-05-07T17:55:48.1444352Z │          Library          │ Vulnerability  │ Severity │ Status │ Installed Version │ Fixed Version │                            Title                             │
2026-05-07T17:55:48.1445312Z ├───────────────────────────┼────────────────┼──────────┼────────┼───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1445905Z │ jaraco.context (METADATA) │ CVE-2026-23949 │ HIGH     │ fixed  │ 5.3.0             │ 6.1.0         │ jaraco.context: jaraco.context: Path traversal via malicious │
2026-05-07T17:55:48.1446444Z │                           │                │          │        │                   │               │ tar archives                                                 │
2026-05-07T17:55:48.1446929Z │                           │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2026-23949                   │
2026-05-07T17:55:48.1447618Z ├───────────────────────────┼────────────────┤          │        ├───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
2026-05-07T17:55:48.1448659Z │ wheel (METADATA)          │ CVE-2026-24049 │          │        │ 0.45.1            │ 0.46.2        │ wheel: wheel: Privilege Escalation or Arbitrary Code         │
2026-05-07T17:55:48.1449225Z │                           │                │          │        │                   │               │ Execution via malicious wheel file...                        │
2026-05-07T17:55:48.1449731Z │                           │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2026-24049                   │
2026-05-07T17:55:48.1450220Z │                           │                │          │        │                   │               │                                                              │
2026-05-07T17:55:48.1450663Z │                           │                │          │        │                   │               │                                                              │
2026-05-07T17:55:48.1451176Z │                           │                │          │        │                   │               │                                                              │
2026-05-07T17:55:48.1451721Z │                           │                │          │        │                   │               │                                                              │
2026-05-07T17:55:48.1452382Z └───────────────────────────┴────────────────┴──────────┴────────┴───────────────────┴───────────────┴──────────────────────────────────────────────────────────────┘
2026-05-07T17:55:48.3348715Z 
2026-05-07T17:55:48.3360632Z ##[error]Bash exited with code '1'.
2026-05-07T17:55:48.3412067Z ##[section]Finishing: Trivy Security Gate (Fail on High/Critical)

