# Passive Recon Dossier: Safaricom PLC (`safaricom.co.ke`)

## 1. Scope & Authorisation Statement
This assessment was conducted strictly under the authorisation of Safaricom's public Bugcrowd program and their Responsible Disclosure policy (Policy Link: [https://www.safaricom.co.ke/responsible-disclosure/](https://www.safaricom.co.ke/responsible-disclosure/)). 

**Passive Discipline:** No active scanning was performed. I explicitly did NOT perform port scans, header probes, or direct HTTP requests against any Safaricom infrastructure. All data was gathered via third-party OSINT.

## 2. Attack-Surface Map (Evidenced & Sourced)
The following assets were discovered using passive OSINT sources. 

### Authentication & Remote Access
*   **`authdxl.safaricom.co.ke`** - Source: [crt.sh Query Result](https://crt.sh/?q=authdxl.safaricom.co.ke)
*   **`azurevpn.safaricom.co.ke`** - Source: [URLScan.io Result](https://urlscan.io/domain/azurevpn.safaricom.co.ke)

### Corporate & Business Portals
*   **`business.safaricom.co.ke`** - Source: [crt.sh Query Result](https://crt.sh/?q=business.safaricom.co.ke)
*   **`corporate.safaricom.co.ke`** - Source: [URLScan.io Result](https://urlscan.io/domain/corporate.safaricom.co.ke)

### Developer, Staging & API Infrastructure
*(Note: Out of scope for active testing, but listed for surface mapping)*
*   **`api.safaricom.co.ke`** - Source: [crt.sh Query Result](https://crt.sh/?q=api.safaricom.co.ke)
*   **`staging.safaricom.co.ke`** - Source: [crt.sh Query Result](https://crt.sh/?q=staging.safaricom.co.ke)
*   **`developer.safaricom.co.ke`** - Source: [URLScan.io Result](https://urlscan.io/domain/developer.safaricom.co.ke)

### Marketing & Consumer
*   **`www.safaricom.co.ke`** - Source: [URLScan.io Result](https://urlscan.io/domain/www.safaricom.co.ke)
*   **`mypage.safaricom.co.ke`** - Source: [crt.sh Query Result](https://crt.sh/?q=mypage.safaricom.co.ke)
*   **`shop.safaricom.co.ke`** - Source: [crt.sh Query Result](https://crt.sh/?q=shop.safaricom.co.ke)

## 3. People & Process Signals
Beyond infrastructure, OSINT reveals organizational context and potential vectors:
*   **Tech Stack Clues (LinkedIn):** Searching LinkedIn for Safaricom engineering roles (e.g., "Cloud Engineer", "DevOps") frequently lists experience with **Azure, Kubernetes, and CI/CD pipelines**. This aligns with the discovery of the `azurevpn` subdomain, suggesting a heavy Microsoft Azure cloud dependency.
*   **Code Repositories (GitHub):** A search of GitHub for `org:safaricom` reveals public developer documentation and API integration guides (e.g., for M-Pesa Daraja API). While no direct leaks were found, monitoring these public repos for accidental credential commits by third-party developers is a continuous threat vector.

## 4. Prioritised Likely Entry Points & Rationale
Based on threat-actor TTPs (Tactics, Techniques, and Procedures), here is the prioritization:

1.  **Tier 1: Remote Access Gateways (`azurevpn`, `authdxl`)**
    *   *Rationale:* Threat actors heavily target VPNs and authentication portals for initial access. If these portals lack Multi-Factor Authentication (MFA) or have unpatched vulnerabilities (e.g., recent Ivanti or Citrix VPN flaws), they offer a direct path into the internal network.
2.  **Tier 2: B2B & Corporate Portals (`business`, `corporate`)**
    *   *Rationale:* These are high-value targets for credential stuffing and Business Email Compromise (BEC). They often process sensitive financial data and may have weaker rate-limiting than consumer-facing login pages.
3.  **Tier 3: Third-Party SaaS & API Dependencies (`api`, `developer`)**
    *   *Rationale:* Threat actors focus on supply chain attacks. Misconfigurations in exposed API endpoints or cloud infrastructure (like Azure) could lead to massive data exposure outside Safaricom's direct perimeter.
