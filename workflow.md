# Red Hat Certification Workflow Pipeline

## 1. Onboarding & Product Listing Setup

**Tool:** Red Hat Partner Connect Portal

**Links:**
- Portal: <a href="https://connect.redhat.com/" target="_blank">https://connect.redhat.com/</a>
- Documentation: <a href="https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_software_certification_quick_start_guide" target="_blank">https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_software_certification_quick_start_guide</a>
- Workflow Guide: <a href="https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index" target="_blank">https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index</a>

**Steps:**
- Join Red Hat Connect Technology Partner Program
- Accept program terms and conditions
- Create a product listing (select category: Standalone, Containerized, or OpenStack)
- Complete company profile information
- Add components to the product listing
- Complete product listing information tabs:
  - General
  - Features
  - Quick Start
  - Resources
  - FAQs
  - Support
  - Contacts
  - Legal
  - SEO
- Generate API key for automation (if using automated submission)

---

## 2. System Under Test (SUT) Setup

**Definition:** A system on which the product that needs certification is installed or configured.

**Prerequisite:** Required for all certification types

**Links:**
- Setup Documentation: <a href="https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index#assembly_configuring-the-system-and-running-tests-by-using-cockpit-for-non-containerized-application_openshift-sw-cert-workflow-appendix" target="_blank">https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index#assembly_configuring-the-system-and-running-tests-by-using-cockpit-for-non-containerized-application_openshift-sw-cert-workflow-appendix</a>

**Requirements:**
- RHEL 8 or later installed
- Red Hat subscription with Partner Certification access
- Red Hat Certification Repository configured
- System registration via Red Hat Subscription Management

**Steps:**
1. Register system using Red Hat Subscription Manager
2. Identify available subscriptions and certification pools
3. Attach appropriate subscription pool (provides Red Hat Certification Repository access)
4. Configure Red Hat Certification Repository
5. (Optional) Use Red Hat-provided kickstart files to streamline OS installation

**Note:** Kickstart files are available from Red Hat to simplify SUT OS installation

---

## 3. Container Certification (Prerequisite for Operators & Helm Charts)

**Tool:** Preflight CLI & Red Hat Vulnerability Scanner

**Links:**
- Preflight Tool: <a href="https://github.com/redhat-openshift-ecosystem/openshift-preflight/releases/latest" target="_blank">https://github.com/redhat-openshift-ecosystem/openshift-preflight/releases/latest</a>
- Pyxis API Docs: <a href="https://catalog.redhat.com/api/containers/docs/" target="_blank">https://catalog.redhat.com/api/containers/docs/</a>

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

## 4. Test Plan Execution & Results Submission

**Tool:** Red Hat Certification Portal

**Links:**
- Certification Portal: <a href="https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index" target="_blank">https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index</a>
- Workflow Guide: <a href="https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index" target="_blank">https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index</a>

**Steps:**
- Log in to Red Hat Certification Portal
- Download test plan for your component/product
- Configure System Under Test (SUT) according to test plan requirements
- Run certification tests on your SUT using:
  - CLI interface, or
  - Cockpit web interface
- Review test results
- Download results files
- Upload results to Red Hat Certification Portal

---

## 5. Orchestration Certification

### Operator Workflow

**Tool:** Operator Pipelines / Operator SDK

**Links:**
- Operator Pipelines Docs: <a href="https://redhat-openshift-ecosystem.github.io/operator-pipelines/" target="_blank">https://redhat-openshift-ecosystem.github.io/operator-pipelines/</a>
- Certified Operators Repo: <a href="https://github.com/redhat-openshift-ecosystem/certified-operators" target="_blank">https://github.com/redhat-openshift-ecosystem/certified-operators</a>
- Marketplace Operators Repo: <a href="https://github.com/redhat-openshift-ecosystem/redhat-marketplace-operators" target="_blank">https://github.com/redhat-openshift-ecosystem/redhat-marketplace-operators</a>

**Steps:**
- Fork Red Hat repo, add bundle
- Check formatting locally using `operator-courier verify`
- Run CI pipeline to test OLM deployment
- Submit GitHub Pull Request; automated PR pipelines check annotations and formatting

### Helm Chart Workflow

**Tool:** Chart Verifier (`chart-verifier`)

**Links:**
- Chart Verifier Tool: <a href="https://github.com/redhat-certification/chart-verifier" target="_blank">https://github.com/redhat-certification/chart-verifier</a>
- Partner Connect Portal: <a href="https://connect.redhat.com/" target="_blank">https://connect.redhat.com/</a>

**Steps:**
- Verify that all images deployed by the Helm chart are Red Hat certified
- Fork the Red Hat upstream repository and run the `chart-verifier` CLI tool
- This tool checks chart formatting and validates required OpenShift metadata
- Submit certified Helm chart and verification report via pull request
- Customers can download published charts from <a href="https://charts.openshift.io" target="_blank">https://charts.openshift.io</a> and the Red Hat Ecosystem Catalog

---

## 6. Specialized Badge Certification (CNI, CSI, CNF)

**Tool:** OpenShift Operator Pipelines (Custom test plans)

**Links:**
- Policy Documentation: <a href="https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_openshift_software_certification_policy_guide" target="_blank">https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_openshift_software_certification_policy_guide</a>

**Steps:**
- Run specialized OpenShift interoperability and lifecycle tests
- Submit results via Red Hat Certification Portal or GitHub PR

---

## 7. Publishing & Lifecycle

**Tool:** Red Hat Ecosystem Catalog

**Links:**
- Catalog: <a href="https://catalog.redhat.com/" target="_blank">https://catalog.redhat.com/</a>

**Steps:**
- Add certified application to product listing page
- Merge approved PRs (for Operators and Helm Charts)
- Publish to Red Hat Ecosystem Catalog and embedded OperatorHub
- Maintain application components and periodically rebuild containers for recertification
- Product information is displayed on the Red Hat Ecosystem Catalog using provided product information
