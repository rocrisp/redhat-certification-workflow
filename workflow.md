# Red Hat Certification Workflow Pipeline

## 1. Onboarding & Product Listing Setup

**Tool:** Red Hat Partner Connect Portal

**Links:**
- Portal: [https://connect.redhat.com/](https://connect.redhat.com/)
- Documentation: [https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_software_certification_quick_start_guide](https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_software_certification_quick_start_guide)
- Workflow Guide: [https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index](https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index)

**Steps:**
- Join Red Hat Connect Technology Partner Program
- Accept program terms and conditions
- Create a product listing (select category: "Containerized Application" for containers, or "Standalone" / "OpenStack" as applicable)
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

---

## 2. Container Certification (Prerequisite for Operators & Helm Charts)

**Tool:** Preflight CLI & Red Hat Vulnerability Scanner

**Links:**
- Preflight Tool: [https://github.com/redhat-openshift-ecosystem/openshift-preflight/releases/latest](https://github.com/redhat-openshift-ecosystem/openshift-preflight/releases/latest)
- Container Checks: [https://github.com/redhat-openshift-ecosystem/openshift-preflight/blob/main/docs/skills/preflight-check-container/SKILL.md#common-container-checks](https://github.com/redhat-openshift-ecosystem/openshift-preflight/blob/main/docs/skills/preflight-check-container/SKILL.md#common-container-checks)
- Pyxis API Docs: [https://catalog.redhat.com/api/containers/docs/](https://catalog.redhat.com/api/containers/docs/)

**Steps:**
1. Build your container image
2. Upload image to an OCI-compliant registry of your choice
3. Download the Preflight certification utility
4. Run Preflight against your container image
5. Submit test results on the Red Hat Partner Connect portal
6. Red Hat scans container layers for vulnerabilities and assigns a Container Health Index grade (requires Grade "A")
7. Add the certified container to your Product Listing page
8. Publish the certified product listing on the Red Hat Ecosystem Catalog

**Pipeline Example:**
```bash
preflight check container registry.example.org/<namespace>/<image>:<tag> \
  --submit \
  --pyxis-api-token=<api_token> \
  --certification-project-id=<component_id> \
  --docker-config=./temp-auth.json
```

---

## 3. Test Plan Execution & Results Submission (Standalone / Non-Containerized Apps)

**Tool:** Red Hat Certification Portal

**Links:**
- Workflow Guide: [https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index](https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index)

**Steps:**
- Log in to Red Hat Certification Portal
- Download test plan for your component/product
- Configure System Under Test (SUT) according to test plan requirements
- Run certification tests on your SUT using CLI interface
- Review test results
- Download results files
- Upload results to Red Hat Certification Portal

---

## 4. Operator Certification

**Tool:** Operator Pipelines / Operator SDK

**Prerequisite:** All containers referenced in your Operator Bundle must be certified and published in the Red Hat Ecosystem Catalog before certifying the Operator Bundle.

**Links:**
- Operator Pipelines Docs: [https://redhat-openshift-ecosystem.github.io/operator-pipelines/](https://redhat-openshift-ecosystem.github.io/operator-pipelines/)
- Operator Checks: [https://github.com/redhat-openshift-ecosystem/openshift-preflight/blob/main/docs/skills/preflight-check-operator/SKILL.md](https://github.com/redhat-openshift-ecosystem/openshift-preflight/blob/main/docs/skills/preflight-check-operator/SKILL.md)
- Certified Operators Repo: [https://github.com/redhat-openshift-ecosystem/certified-operators](https://github.com/redhat-openshift-ecosystem/certified-operators)
- Marketplace Operators Repo: [https://github.com/redhat-openshift-ecosystem/redhat-marketplace-operators](https://github.com/redhat-openshift-ecosystem/redhat-marketplace-operators)

**Steps:**
1. Fork the Red Hat upstream certified-operators repository and add your Operator bundle
2. Install and run the Red Hat certification pipeline on your test environment (recommended: run locally to integrate with your own CI/CD workflows)
3. Alternatively, use Red Hat's hosted pipeline by submitting your Operator bundle via a GitHub Pull Request
4. Review test results and troubleshoot any issues
5. Submit final results to Red Hat via a GitHub Pull Request
6. After the PR is merged, add the Certified Operator to your Product Listing page
7. The Operator is published on the Red Hat Ecosystem Catalog and in the embedded OperatorHub

---

## 5. Helm Chart Certification

**Tool:** Chart Verifier (`chart-verifier`)

**Links:**
- Chart Verifier Tool: [https://github.com/redhat-certification/chart-verifier](https://github.com/redhat-certification/chart-verifier)
- Partner Connect Portal: [https://connect.redhat.com/](https://connect.redhat.com/)

**Steps:**
- Verify that all images deployed by the Helm chart are Red Hat certified
- Fork the Red Hat upstream repository and run the `chart-verifier` CLI tool
- This tool checks chart formatting and validates required OpenShift metadata
- Submit certified Helm chart and verification report via pull request
- Customers can download published charts from [https://charts.openshift.io](https://charts.openshift.io) and the Red Hat Ecosystem Catalog

---

## 6. Specialized Badge Certification (CNI, CSI, CNF)

**Tool:** OpenShift Operator Pipelines (Custom test plans)

**Links:**
- Policy Documentation: [https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_openshift_software_certification_policy_guide](https://docs.redhat.com/en/documentation/red_hat_software_certification/2026/html/red_hat_openshift_software_certification_policy_guide)

**Steps:**
- Run specialized OpenShift interoperability and lifecycle tests
- Submit results via Red Hat Certification Portal or GitHub PR

---

## 7. Publishing & Lifecycle

**Tool:** Red Hat Ecosystem Catalog

**Links:**
- Catalog: [https://catalog.redhat.com/](https://catalog.redhat.com/)

**Steps:**
- Add certified application to product listing page
- Merge approved PRs (for Operators and Helm Charts)
- Publish to Red Hat Ecosystem Catalog and embedded OperatorHub
- Maintain application components and periodically rebuild containers for recertification

---

## Appendix: Cockpit (Optional Testing Interface)

**Description:** Cockpit is an optional web-based system management interface that can be used as an alternative to CLI for running certification tests.

**Links:**
- Cockpit Testing Guide: [https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index#assembly_configuring-the-system-and-running-tests-by-using-cockpit-for-non-containerized-application_openshift-sw-cert-workflow-appendix](https://docs.redhat.com/en/documentation/red_hat_software_certification/2025/html-single/red_hat_software_certification_workflow_guide/index#assembly_configuring-the-system-and-running-tests-by-using-cockpit-for-non-containerized-application_openshift-sw-cert-workflow-appendix)

**When to Use:**
- Cockpit is an alternative convenience tool for partners who prefer a web-based interface
- Not required for certification; CLI testing is the primary method
- Use this approach if you prefer web-based system management over command-line testing
