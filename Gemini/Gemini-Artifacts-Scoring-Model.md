This scoring model maps directly to the exact JSON paths and keys present in **artifacts-raw-telemetry.json**. It computes an overall quality and adoption score from **0 to 100 points**, followed by a **hard gatekeeper rule** for security vetoes.

### **Formula Overview**

{Total Score} \= {Adoption (35)} \+ {Freshness \\& Velocity (25)} \+ {Security \\& Hygiene (25)} \+ {Packaging (15)}

### **Dimension 1: Ecosystem Adoption (35 Points Max)**

Evaluates ecosystem adoption across libraries, application codebases, and developer interest.

> * **Maven Package Dependents (15 pts):** Uses http\_sources.deps\_dev.dependentCount (or fallback playwright\_sources.central\_sonatype\_com.directDependentsCount): {Score}\_{{pkg}} \= \\min\\left(15, \\; \\log\_{10}(tt{deps\\\_dev.dependentCount} \+ 1\) \\times 3.75\\right) *(10 \\to 3.75, 100 \\to 7.5, 1,000 \\to 11.25, \\ge 10,000 \\to 15 pts)*  
> * **Application Consumer Repositories (12 pts):** Uses playwright\_sources.libraries\_io.dependentRepositoriesCount: {Score}\_{{apps}} \= \\min\\left(12, \\; \\log\_{10}(tt{libraries\\\_io.dependentRepositoriesCount} \+ 1\) \\times 3.0\\right) *(10 \\to 3.0, 100 \\to 6.0, 1,000 \\to 9.0, \\ge 10,000 \\to 12 pts)*  
> * **Repository Stargazers / Social Proof (8 pts):** Uses http\_sources.deps\_dev.starsCount: {Score}\_{{stars}} \= \\min\\left(8, \\; \\log\_{10}(tt{deps\\\_dev.starsCount} \+ 1\) \\times 2.0\\right) *(10 \\to 2.0, 100 \\to 4.0, 1,000 \\to 6.0, \\ge 10,000 \\to 8 pts)*

### **Dimension 2: Maintenance Freshness & Velocity (25 Points Max)**

Evaluates whether the artifact is actively maintained and compatible with modern Java/Gradle baselines.

> * **Release Recency (15 pts):** Calculated by comparing the current date against http\_sources.search\_maven\_org.timestamp (epoch ms) or parsing http\_sources.repo1\_maven\_org.lastUpdated (YYYYMMDDHHMMSS):  
  * **\< 6 months:** **15 pts**  
  * **6–12 months:** **12 pts**  
  * **12–24 months:** **8 pts**  
  * **24–36 months:** **4 pts**  
  * **\> 36 months:** **0 pts**  
> * **Release Velocity (10 pts):** Uses http\_sources.search\_maven\_org.versionCount:  
  * **\\ge 10 versions:** **10 pts** (proven multi-release lifecycle)  
  * **3–9 versions:** **6 pts**  
  * **2 versions:** **3 pts**  
  * **1 version:** **1 pt** (high risk of a dead or experimental upload)  
  * **0 / null:** **0 pts**

### **Dimension 3: Security Posture & Supply-Chain Hygiene (25 Points Max)**

Evaluates active vulnerabilities in the target version alongside upstream project health.

> * **Vulnerability Penalty (15 pts baseline):** Evaluates http\_sources.osv\_dev.vulns:  
  * **vulns is empty (\[\]) or null:** **15 pts** (Clean bill of health)  
  * **Highest database\_specific.severity is LOW or MEDIUM:** **7 pts**  
  * **Highest database\_specific.severity is HIGH or CRITICAL:** **0 pts**  
> * **Repository Supply Chain Hygiene (10 pts):** Evaluates http\_sources.deps\_dev.scorecard:  
  * **If scorecard is present:** {Score}\_{{hygiene}} \= tt{deps\\\_dev.scorecard.overallScore} \\quad ({Scale } 0.0 { to } 10.0)  
  * **If scorecard is null (Fallback to Libraries.io SourceRank):** {Score}\_{{hygiene}} \= \\min\\left(10, \\; \\frac{tt{libraries\\\_io.sourceRank}}{3}\\right)  
  * **If both are null/missing:** Assign a baseline neutral of **5.0 pts**.

### **Dimension 4: Packaging & IDE Experience (15 Points Max)**

Evaluates developer ergonomics inside Gradle and IDEs via http\_sources.search\_maven\_org.ec:

> * **ec contains .jar or .aar:** **\+5 pts** (runtime binary available)  
> * **ec contains \-sources.jar:** **\+5 pts** (source code navigation & debugging)  
> * **ec contains \-javadoc.jar:** **\+5 pts** (in-editor API documentation)

### **Hard Gatekeeper Rule (Auto-Rejection)**

Regardless of the total numerical score:  
**Hard Veto:** If http\_sources.osv\_dev.vulns contains any vulnerability where database\_specific.severity is "HIGH" or "CRITICAL", the artifact status is automatically overridden to **REJECTED** or **FLAGGED** for that target version.

### **Catalog Ingestion Thresholds**

| Total Score | Status | Catalog Action (libs.versions.toml) |
| :---- | :---- | :---- |
| **\\ge 70** | **APPROVED** | Write directly into \[libraries\] and \[versions\]. |
| **45 – 69** | **REVIEW** | Place under a \# Requires Review comment block (specialized utility, stable low-churn module). |
| **\< 45** | **REJECTED** | Exclude completely; flag as abandoned or unvetted. |

### **Worked Evaluation of Artifacts in artifacts-raw-telemetry.json**

| Artifact Coordinate | Adoption (35) | Freshness (25) | Security (25) | Packaging (15) | Total Score | Gatekeeper Status | Catalog Action |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **jackson-core:2.14.0** | 35.0 | 10.0 | 6.8 | 15.0 | **66.8** | **VETO (HIGH CVE)** | **REJECTED** *(Upgrade to 2.21.x)* |
| **jackson-annotations:2.21** | 35.0 | 25.0 | 21.8 | 15.0 | **96.8** | Passed | **APPROVED** |
| **jackson-databind:2.21.4** | 35.0 | 25.0 | 22.4 | 15.0 | **97.4** | Passed | **APPROVED** |
| **jackson-datatype-javax-money:2.13.0** | 6.1 | 10.0 | 19.7 | 15.0 | **50.8** | Passed | **REVIEW** *(Stale \>3 yrs, low reach)* |
| **jetty-http2-client:12.0.36** | 24.0 | 25.0 | 22.9 | 15.0 | **86.9** | Passed | **APPROVED** |

#### **Step-by-Step Breakdown Notes:**

> 1. **jackson-core:2.14.0**: Scores 35 on adoption and 15 on packaging, but release age is \>36 months (0 pts for recency \+ 10 for velocity). Because osv\_dev.vulns contains GHSA-rf6v-86w5-mf35 (HIGH), the vulnerability score drops to 0 and triggers the **Gatekeeper Hard Veto**.  
> 2. **jackson-annotations:2.21** & **jackson-databind:2.21.4**: Max adoption (35.0), active releases within 6 months (25.0), clean OSV scans (15.0), strong OpenSSF scores (6.8 / 7.4), and complete packaging (15.0).  
> 3. **jackson-datatype-javax-money:2.13.0**: Low adoption (4 dependents \\to 2.6 pts, 12 repos \\to 3.3 pts, 110 stars \\to 0.2 pts). No release since 2021 (0 pts recency). OpenSSF scorecard is null, so it falls back to Libraries.io sourceRank (14 / 3 \= 4.7 pts). Clean OSV (15.0 pts). Total is **50.8**, correctly categorizing it as **REVIEW**.  
> 4. **jetty-http2-client:12.0.36**: Moderate-high adoption (24.0 pts), recent release in mid-2026 (25.0 pts), 0 CVEs \+ 7.9 OpenSSF score (22.9 pts), complete packaging (15.0 pts) \\to **86.9 (APPROVED)**.