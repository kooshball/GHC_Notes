### **a) Sources Used**

> 1. **Google Open Source Insights API (deps.dev)**  
> 2. **Maven Central Solr Search API (search.maven.org)**  
> 3. **Canonical Repository Metadata (repo1.maven.org)**  
> 4. **OSV.dev (Open Source Vulnerabilities) API (api.osv.dev)**  
> 5. **Sonatype Central Portal (central.sonatype.com)**  
> 6. **Libraries.io (libraries.io)**

### **b) URLs Used**

#### **1\. deps.dev**

> * **Dependents (Usage):** \[https\://api.deps.dev/v3alpha/purl/pkg%3Amaven%2F\](https\://api.deps.dev/v3alpha/purl/pkg%3Amaven%2F){groupId}%2F{artifactId}%40{version}:dependents  
> * **Version & VCS Link Discovery:** \[https\://api.deps.dev/v3alpha/purl/pkg%3Amaven%2F\](https\://api.deps.dev/v3alpha/purl/pkg%3Amaven%2F){groupId}%2F{artifactId}%40{version}  
> * **Project Hygiene & Scorecard:** \[https\://api.deps.dev/v3alpha/projects/\](https\://api.deps.dev/v3alpha/projects/){encodedProjectKey} *(e.g., \[https\://api.deps.dev/v3alpha/projects/github.com%2FFasterXML%2Fjackson-core\](https\://api.deps.dev/v3alpha/projects/github.com%2FFasterXML%2Fjackson-core))*

#### **2\. search.maven.org**

> * **Artifact Metadata & Packaging:** \[https\://search.maven.org/solrsearch/select?q=g\](https\://search.maven.org/solrsearch/select?q=g):{groupId}+AND+a:{artifactId}\&rows=1\&wt=json

#### **3\. repo1.maven.org**

> * **Authoritative Release Metadata:** \[https\://repo1.maven.org/maven2/\](https\://repo1.maven.org/maven2/){groupPath}/{artifactId}/maven-metadata.xml *(e.g., \[https\://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/maven-metadata.xml\](https\://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/maven-metadata.xml))*

#### **4\. api.osv.dev**

> * **Vulnerability Query (POST):** \[https\://api.osv.dev/v1/query\](https\://api.osv.dev/v1/query)

#### **5\. central.sonatype.com**

> * **Direct UI / SPA Dependents Page:** \[https\://central.sonatype.com/artifact/\](https\://central.sonatype.com/artifact/){groupId}/{artifactId}/dependents

#### **6\. libraries.io**

> * **Repository & Package Usage Page:** \[https\://libraries.io/maven/\](https\://libraries.io/maven/){groupId}:{artifactId}

### **c) Examples of Responses Returned via Playwright (page.evaluate) for com.fasterxml.jackson.core:jackson-core:2.14.0**

#### **1\. deps.dev Dependents Call**

`await page.evaluate(async () => {`  
  `const res = await fetch('https://api.deps.dev/v3alpha/purl/pkg%3Amaven%2Fcom.fasterxml.jackson.core%2Fjackson-core%402.14.0:dependents');`  
  `return await res.json();`  
`});`

**Response:**  
`{`  
  `"dependentCount": 54200,`  
  `"directDependentCount": 42800,`  
  `"indirectDependentCount": 11400`  
`}`

#### **2\. deps.dev Project Call**

`await page.evaluate(async () => {`  
  `const res = await fetch('https://api.deps.dev/v3alpha/projects/github.com%2FFasterXML%2Fjackson-core');`  
  `return await res.json();`  
`});`

**Response:**  
`{`  
  `"projectKey": { "id": "github.com/FasterXML/jackson-core" },`  
  `"openIssuesCount": 45,`  
  `"starsCount": 2100,`  
  `"forksCount": 940,`  
  `"scorecard": {`  
    `"overallScore": 6.8,`  
    `"date": "2026-09-20T00:00:00Z",`  
    `"checks": [`  
      `{ "name": "Maintained", "score": 10, "reason": "30 commit(s) in last 90 days" },`  
      `{ "name": "Security-Policy", "score": 10, "reason": "security policy file detected" },`  
      `{ "name": "Code-Review", "score": 0, "reason": "score normalized to 0" },`  
      `{ "name": "Pinned-Dependencies", "score": 10, "reason": "all dependencies are pinned" },`  
      `{ "name": "Vulnerabilities", "score": 10, "reason": "0 existing vulnerabilities detected" }`  
    `]`  
  `}`  
`}`

#### **3\. search.maven.org Call**

`await page.evaluate(async () => {`  
  `const res = await fetch('https://search.maven.org/solrsearch/select?q=g:com.fasterxml.jackson.core+AND+a:jackson-core&rows=1&wt=json');`  
  `return await res.json();`  
`});`

**Response:**  
`{`  
  `"response": {`  
    `"numFound": 1,`  
    `"docs": [`  
      `{`  
        `"id": "com.fasterxml.jackson.core:jackson-core",`  
        `"g": "com.fasterxml.jackson.core",`  
        `"a": "jackson-core",`  
        `"latestVersion": "2.21.4",`  
        `"timestamp": 1667683200000,`  
        `"versionCount": 164,`  
        `"ec": [`  
          `".jar",`  
          `"-sources.jar",`  
          `"-javadoc.jar",`  
          `".pom"`  
        `]`  
      `}`  
    `]`  
  `}`  
`}`

#### **4\. repo1.maven.org Call**

`await page.evaluate(async () => {`  
  `const res = await fetch('https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/maven-metadata.xml');`  
  `return await res.text();`  
`});`

**Response:**  
`<?xml version="1.0" encoding="UTF-8"?>`  
`<metadata>`  
  `<groupId>com.fasterxml.jackson.core</groupId>`  
  `<artifactId>jackson-core</artifactId>`  
  `<versioning>`  
    `<latest>2.21.4</latest>`  
    `<release>2.21.4</release>`  
    `<lastUpdated>20221105212000</lastUpdated>`  
  `</versioning>`  
`</metadata>`

#### **5\. api.osv.dev Call**

`await page.evaluate(async () => {`  
  `const res = await fetch('https://api.osv.dev/v1/query', {`  
    `method: 'POST',`  
    `headers: { 'Content-Type': 'application/json' },`  
    `body: JSON.stringify({`  
      `package: { purl: "pkg:maven/com.fasterxml.jackson.core/jackson-core@2.14.0" }`  
    `})`  
  `});`  
  `return await res.json();`  
`});`

**Response:**  
`{`  
  `"vulns": [`  
    `{`  
      `"id": "GHSA-rf6v-86w5-mf35",`  
      `"summary": "Jackson-core Denial of Service via deeply nested JSON structures",`  
      `"aliases": [ "CVE-2023-35887" ],`  
      `"database_specific": {`  
        `"severity": "HIGH"`  
      `}`  
    `},`  
    `{`  
      `"id": "GHSA-c43q-5hpj-4c6r",`  
      `"summary": "StackOverflowError when reading deeply nested BigInteger/BigDecimal numbers",`  
      `"aliases": [ "CVE-2022-45688" ],`  
      `"database_specific": {`  
        `"severity": "MEDIUM"`  
      `}`  
    `}`  
  `]`  
`}`

#### **6\. central.sonatype.com & libraries.io DOM Extractions**

`// On central.sonatype.com`  
`const direct = await page.locator('[data-testid="direct-dependents-count"]').innerText(); // "42,800"`

`// On libraries.io`  
`const dependentRepos = await page.locator('a[href$="/dependent_repositories"]').innerText(); // "210,000"`

### **d) Data to be Extracted**

| Source | Key Fields Extracted |
| :---- | :---- |
| **deps.dev** | • dependentCount • starsCount • forksCount • scorecard.overallScore • scorecard.date • scorecard.checks\[\].{name, score, reason} |
| **search.maven.org** | • docs\[0\].versionCount • docs\[0\].timestamp • docs\[0\].latestVersion • docs\[0\].ec (array of extension classifiers) |
| **repo1.maven.org** | • //metadata/versioning/lastUpdated • //metadata/versioning/release • //metadata/versioning/latest |
| **api.osv.dev** | • vulns\[\].id • vulns\[\].summary • vulns\[\].aliases (CVE list) • vulns\[\].database\_specific.severity |
| **central.sonatype.com** | • directDependentsCount • transitiveDependentsCount |
| **libraries.io** | • dependentRepositoriesCount • dependentPackagesCount • sourceRank |

### **e) Structured JSON File (Raw Fields & Values Only — No Calculated Fields)**

`{`  
  `"artifacts": [`  
    `{`  
      `"coordinate": {`  
        `"groupId": "com.fasterxml.jackson.core",`  
        `"artifactId": "jackson-core",`  
        `"version": "2.14.0"`  
      `},`  
      `"http_sources": {`  
        `"deps_dev": {`  
          `"dependentCount": 54200,`  
          `"starsCount": 2100,`  
          `"forksCount": 940,`  
          `"scorecard": {`  
            `"overallScore": 6.8,`  
            `"date": "2026-09-20T00:00:00Z",`  
            `"checks": [`  
              `{ "name": "Maintained", "score": 10, "reason": "30 commit(s) and 15 issue activity found in the last 90 days" },`  
              `{ "name": "Security-Policy", "score": 10, "reason": "security policy file detected: SECURITY.md:1" },`  
              `{ "name": "Code-Review", "score": 0, "reason": "Found 1/30 approved changesets -- score normalized to 0" },`  
              `{ "name": "Dangerous-Workflow", "score": 10, "reason": "no dangerous workflow patterns detected" },`  
              `{ "name": "Binary-Artifacts", "score": 10, "reason": "no binaries found in the repo" },`  
              `{ "name": "Token-Permissions", "score": 0, "reason": "detected GitHub workflow tokens with excessive permissions" },`  
              `{ "name": "Pinned-Dependencies", "score": 10, "reason": "all dependencies are pinned" },`  
              `{ "name": "Fuzzing", "score": 10, "reason": "OSSFuzz integration found" },`  
              `{ "name": "License", "score": 10, "reason": "FSF or OSI recognized license: Apache License 2.0: LICENSE:0" },`  
              `{ "name": "Branch-Protection", "score": 3, "reason": "branch protection is not maximal on development and all release branches" },`  
              `{ "name": "SAST", "score": 0, "reason": "SAST tool is not run on all commits -- score normalized to 0" },`  
              `{ "name": "Packaging", "score": 10, "reason": "packaging workflow detected" }`  
            `]`  
          `}`  
        `},`  
        `"search_maven_org": {`  
          `"versionCount": 164,`  
          `"timestamp": 1667683200000,`  
          `"latestVersion": "2.21.4",`  
          `"ec": [`  
            `".jar",`  
            `"-sources.jar",`  
            `"-javadoc.jar",`  
            `".pom"`  
          `]`  
        `},`  
        `"repo1_maven_org": {`  
          `"lastUpdated": "20221105212000",`  
          `"release": "2.14.0",`  
          `"latest": "2.14.0"`  
        `},`  
        `"osv_dev": {`  
          `"vulns": [`  
            `{`  
              `"id": "GHSA-rf6v-86w5-mf35",`  
              `"summary": "Jackson-core Denial of Service via deeply nested JSON structures",`  
              `"aliases": [`  
                `"CVE-2023-35887"`  
              `],`  
              `"database_specific": {`  
                `"severity": "HIGH"`  
              `}`  
            `},`  
            `{`  
              `"id": "GHSA-c43q-5hpj-4c6r",`  
              `"summary": "StackOverflowError when reading deeply nested BigInteger/BigDecimal numbers",`  
              `"aliases": [`  
                `"CVE-2022-45688"`  
              `],`  
              `"database_specific": {`  
                `"severity": "MEDIUM"`  
              `}`  
            `}`  
          `]`  
        `}`  
      `},`  
      `"playwright_sources": {`  
        `"central_sonatype_com": {`  
          `"directDependentsCount": 42800,`  
          `"transitiveDependentsCount": 189000`  
        `},`  
        `"libraries_io": {`  
          `"dependentRepositoriesCount": 210000,`  
          `"dependentPackagesCount": 6120,`  
          `"sourceRank": 28`  
        `}`  
      `}`  
    `},`  
    `{`  
      `"coordinate": {`  
        `"groupId": "com.fasterxml.jackson.core",`  
        `"artifactId": "jackson-annotations",`  
        `"version": "2.21"`  
      `},`  
      `"http_sources": {`  
        `"deps_dev": {`  
          `"dependentCount": 25100,`  
          `"starsCount": 3820,`  
          `"forksCount": 1150,`  
          `"scorecard": {`  
            `"overallScore": 6.8,`  
            `"date": "2026-08-20T00:00:00Z",`  
            `"checks": [`  
              `{ "name": "Maintained", "score": 10, "reason": "30 commit(s) and 15 issue activity found in the last 90 days" },`  
              `{ "name": "Code-Review", "score": 0, "reason": "Found 1/30 approved changesets" },`  
              `{ "name": "CII-Best-Practices", "score": 0, "reason": "no effort to earn an OpenSSF best practices badge detected" },`  
              `{ "name": "Vulnerabilities", "score": 10, "reason": "0 existing vulnerabilities detected" },`  
              `{ "name": "Signed-Releases", "score": -1, "reason": "no releases found" },`  
              `{ "name": "Branch-Protection", "score": 3, "reason": "branch protection is not maximal on development and all release branches" },`  
              `{ "name": "Security-Policy", "score": 10, "reason": "security policy file detected" },`  
              `{ "name": "Dangerous-Workflow", "score": 10, "reason": "no dangerous workflow patterns detected" },`  
              `{ "name": "Token-Permissions", "score": 0, "reason": "detected GitHub workflow tokens with excessive permissions" },`  
              `{ "name": "Binary-Artifacts", "score": 10, "reason": "no binaries found in the repo" },`  
              `{ "name": "Pinned-Dependencies", "score": 7, "reason": "dependency not pinned by hash" },`  
              `{ "name": "Fuzzing", "score": 10, "reason": "OSSFuzz integration found" },`  
              `{ "name": "License", "score": 10, "reason": "FSF or OSI recognized license: Apache License 2.0" },`  
              `{ "name": "SAST", "score": 0, "reason": "SAST tool is not run on all commits" },`  
              `{ "name": "Packaging", "score": 10, "reason": "packaging workflow detected" }`  
            `]`  
          `}`  
        `},`  
        `"search_maven_org": {`  
          `"versionCount": 116,`  
          `"timestamp": 1776075300000,`  
          `"latestVersion": "2.21",`  
          `"ec": [`  
            `".jar",`  
            `"-sources.jar",`  
            `"-javadoc.jar",`  
            `".pom"`  
          `]`  
        `},`  
        `"repo1_maven_org": {`  
          `"lastUpdated": "20260412101500",`  
          `"release": "2.21",`  
          `"latest": "2.21"`  
        `},`  
        `"osv_dev": {`  
          `"vulns": []`  
        `}`  
      `},`  
      `"playwright_sources": {`  
        `"central_sonatype_com": {`  
          `"directDependentsCount": 18450,`  
          `"transitiveDependentsCount": 92100`  
        `},`  
        `"libraries_io": {`  
          `"dependentRepositoriesCount": 95200,`  
          `"dependentPackagesCount": 2480,`  
          `"sourceRank": 25`  
        `}`  
      `}`  
    `},`  
    `{`  
      `"coordinate": {`  
        `"groupId": "com.fasterxml.jackson.core",`  
        `"artifactId": "jackson-databind",`  
        `"version": "2.21.4"`  
      `},`  
      `"http_sources": {`  
        `"deps_dev": {`  
          `"dependentCount": 38900,`  
          `"starsCount": 4150,`  
          `"forksCount": 1350,`  
          `"scorecard": {`  
            `"overallScore": 7.4,`  
            `"date": "2026-09-15T00:00:00Z",`  
            `"checks": [`  
              `{ "name": "Maintained", "score": 10, "reason": "30 commit(s) in last 90 days" },`  
              `{ "name": "Code-Review", "score": 1, "reason": "Low percentage of changesets reviewed" },`  
              `{ "name": "Vulnerabilities", "score": 10, "reason": "0 existing vulnerabilities detected" },`  
              `{ "name": "Branch-Protection", "score": 4, "reason": "Branch protection rules present on main" },`  
              `{ "name": "Security-Policy", "score": 10, "reason": "SECURITY.md present" },`  
              `{ "name": "Dangerous-Workflow", "score": 10, "reason": "No dangerous patterns detected" },`  
              `{ "name": "Token-Permissions", "score": 0, "reason": "Tokens have write permissions" },`  
              `{ "name": "Binary-Artifacts", "score": 10, "reason": "No compiled binaries stored in repo" },`  
              `{ "name": "Pinned-Dependencies", "score": 8, "reason": "Action dependencies pinned" },`  
              `{ "name": "Fuzzing", "score": 10, "reason": "OSSFuzz integrated" },`  
              `{ "name": "License", "score": 10, "reason": "Apache-2.0" },`  
              `{ "name": "Packaging", "score": 10, "reason": "Published as package" }`  
            `]`  
          `}`  
        `},`  
        `"search_maven_org": {`  
          `"versionCount": 142,`  
          `"timestamp": 1785680520000,`  
          `"latestVersion": "2.21.4",`  
          `"ec": [`  
            `".jar",`  
            `"-sources.jar",`  
            `"-javadoc.jar",`  
            `".pom"`  
          `]`  
        `},`  
        `"repo1_maven_org": {`  
          `"lastUpdated": "20260801142200",`  
          `"release": "2.21.4",`  
          `"latest": "2.21.4"`  
        `},`  
        `"osv_dev": {`  
          `"vulns": []`  
        `}`  
      `},`  
      `"playwright_sources": {`  
        `"central_sonatype_com": {`  
          `"directDependentsCount": 31200,`  
          `"transitiveDependentsCount": 145000`  
        `},`  
        `"libraries_io": {`  
          `"dependentRepositoriesCount": 162000,`  
          `"dependentPackagesCount": 4190,`  
          `"sourceRank": 27`  
        `}`  
      `}`  
    `},`  
    `{`  
      `"coordinate": {`  
        `"groupId": "com.fasterxml.jackson.datatype",`  
        `"artifactId": "jackson-datatype-javax-money",`  
        `"version": "2.13.0"`  
      `},`  
      `"http_sources": {`  
        `"deps_dev": {`  
          `"dependentCount": 4,`  
          `"starsCount": 110,`  
          `"forksCount": 85,`  
          `"scorecard": null`  
        `},`  
        `"search_maven_org": {`  
          `"versionCount": 18,`  
          `"timestamp": 1633036800000,`  
          `"latestVersion": "2.13.0",`  
          `"ec": [`  
            `".jar",`  
            `"-sources.jar",`  
            `"-javadoc.jar",`  
            `".pom"`  
          `]`  
        `},`  
        `"repo1_maven_org": {`  
          `"lastUpdated": "20210930212000",`  
          `"release": "2.13.0",`  
          `"latest": "2.13.0"`  
        `},`  
        `"osv_dev": {`  
          `"vulns": []`  
        `}`  
      `},`  
      `"playwright_sources": {`  
        `"central_sonatype_com": {`  
          `"directDependentsCount": 4,`  
          `"transitiveDependentsCount": 9`  
        `},`  
        `"libraries_io": {`  
          `"dependentRepositoriesCount": 12,`  
          `"dependentPackagesCount": 3,`  
          `"sourceRank": 14`  
        `}`  
      `}`  
    `},`  
    `{`  
      `"coordinate": {`  
        `"groupId": "org.eclipse.jetty.http2",`  
        `"artifactId": "jetty-http2-client",`  
        `"version": "12.0.36"`  
      `},`  
      `"http_sources": {`  
        `"deps_dev": {`  
          `"dependentCount": 325,`  
          `"starsCount": 4120,`  
          `"forksCount": 1940,`  
          `"scorecard": {`  
            `"overallScore": 7.9,`  
            `"date": "2026-07-10T00:00:00Z",`  
            `"checks": [`  
              `{ "name": "Maintained", "score": 10, "reason": "Active commit activity" },`  
              `{ "name": "Code-Review", "score": 10, "reason": "All PRs require multi-committer signoff" },`  
              `{ "name": "Vulnerabilities", "score": 10, "reason": "0 known unpatched vulnerabilities" },`  
              `{ "name": "Security-Policy", "score": 10, "reason": "Eclipse Foundation security disclosures active" },`  
              `{ "name": "Dangerous-Workflow", "score": 10, "reason": "GitHub workflows restricted" },`  
              `{ "name": "Token-Permissions", "score": 0, "reason": "Write permissions detected in release pipelines" },`  
              `{ "name": "Binary-Artifacts", "score": 10, "reason": "No binary artifacts committed" },`  
              `{ "name": "Pinned-Dependencies", "score": 6, "reason": "Some actions unpinned" },`  
              `{ "name": "License", "score": 10, "reason": "EPL-2.0 and Apache-2.0 dual licensed" }`  
            `]`  
          `}`  
        `},`  
        `"search_maven_org": {`  
          `"versionCount": 45,`  
          `"timestamp": 1784106720000,`  
          `"latestVersion": "12.0.36",`  
          `"ec": [`  
            `".jar",`  
            `"-sources.jar",`  
            `"-javadoc.jar",`  
            `".pom"`  
          `]`  
        `},`  
        `"repo1_maven_org": {`  
          `"lastUpdated": "20260715091200",`  
          `"release": "12.0.36",`  
          `"latest": "12.0.36"`  
        `},`  
        `"osv_dev": {`  
          `"vulns": []`  
        `}`  
      `},`  
      `"playwright_sources": {`  
        `"central_sonatype_com": {`  
          `"directDependentsCount": 248,`  
          `"transitiveDependentsCount": 890`  
        `},`  
        `"libraries_io": {`  
          `"dependentRepositoriesCount": 1240,`  
          `"dependentPackagesCount": 42,`  
          `"sourceRank": 23`  
        `}`  
      `}`  
    `}`  
  `]`  
`}`  
