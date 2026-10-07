# Red Hat Certification Workflow Pipeline

## 1. Onboarding & Product Listing Setup

**Tool:** Red Hat Partner Connect Portal

**Links:**
- Portal: https://connect.redhat.com/
- Documentation: https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_software_certification_quick_start_guide

**Steps:**
- Join program, accept agreements, create product listing, and add components

---

## 2. Container Certification (Prerequisite)

**Tool:** Preflight CLI & Red Hat Vulnerability Scanner

**Links:**
- Preflight Tool: https://github.com/redhat-openshift-ecosystem/openshift-preflight/releases/latest
- Pyxis API Docs: https://catalog.redhat.com/api/containers/docs/

**Steps:**
- Build image → Push to Registry → Run `preflight check container` → Submit results
- Preflight conducts extensive static analysis and policy checks
- Red Hat scans for vulnerabilities and assigns a Container Health Index grade (requires Grade "A")

**Pipeline Example:**
```bash
preflight check container registry.example.org/<namespace>/<image>:<tag> \
  --submit \
  --pyxis-api-token=<api_token> \
  --certification-project-id=<component_id> \
  --docker-config=./temp-auth.json
```

---

## 3. Orchestration Certification

### Operator Workflow

**Tool:** Operator Pipelines / Operator SDK

**Links:**
- Operator Pipelines Docs: https://redhat-openshift-ecosystem.github.io/operator-pipelines/
- Certified Operators Repo: https://github.com/redhat-openshift-ecosystem/certified-operators
- Marketplace Operators Repo: https://github.com/redhat-openshift-ecosystem/redhat-marketplace-operators

**Steps:**
- Fork Red Hat repo, add bundle
- Check formatting locally using `operator-courier verify`
- Run CI pipeline to test OLM deployment
- Submit GitHub Pull Request; automated PR pipelines check annotations and formatting

### Helm Chart Workflow

**Tool:** Chart Verifier (`chart-verifier`)

**Links:**
- Chart Verifier Tool: https://github.com/redhat-certification/chart-verifier
- Partner Connect Portal: https://connect.redhat.com/

**Steps:**
- Verify that all images deployed by the Helm chart are Red Hat certified
- Fork the Red Hat upstream repository and run the `chart-verifier` CLI tool
- This tool checks chart formatting and validates required OpenShift metadata
- Submit certified Helm chart and verification report via pull request
- Customers can download published charts from https://charts.openshift.io and the Red Hat Ecosystem Catalog

---

## 4. Specialized Badge Certification (CNI, CSI, CNF)

**Tool:** OpenShift Operator Pipelines (Custom test plans)

**Links:**
- Policy Documentation: https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_openshift_software_certification_policy_guide

**Steps:**
- Run specialized OpenShift interoperability and lifecycle tests
- Submit results via Red Hat Certification Portal or GitHub PR

---

## 5. Publishing & Lifecycle

**Tool:** Red Hat Ecosystem Catalog

**Links:**
- Catalog: https://catalog.redhat.com/

**Steps:**
- Merge PRs, publish to Catalog / embedded OperatorHub
- Maintain application components and periodically rebuild containers for recertification
