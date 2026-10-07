# Red Hat Certification Workflow Pipeline

## 1. Container Certification Pipeline

*   **Stage 1: Certification Onboarding**
    *   **Details:** Join the Red Hat Connect program, create a "Containerized Application" product listing, and add your components[cite: 1].
    *   **Action:** Generate a Pyxis API key from the Red Hat Partner Connect portal to enable automated test submissions via the REST API[cite: 1, 5].
    *   **Links:** [Red Hat Partner Connect Portal](https://connect.redhat.com/partner-admin/dashboard)[cite: 5]

*   **Stage 2: Certification Testing**
    *   **Details:** Build your container image using Podman and push it to an OCI-compliant registry[cite: 1, 5]. Download and execute the Preflight utility against your image[cite: 1, 5].
    *   **Pipeline Example:**
        ```bash
        preflight check container registry.example.org/<namespace>/<image>:<tag> \
          --submit \
          --pyxis-api-token=<api_token> \
          --certification-project-id=<component_id> \
          --docker-config=./temp-auth.json
        ```[cite: 1, 5]

*   **Stage 3: Vulnerability Scanning & Publishing**
    *   **Details:** Red Hat asynchronously scans the submitted image layers for vulnerabilities and assigns a Container Health Index grade[cite: 1, 5, 10].
    *   **Action:** Once the image achieves a passing grade (Grade A), publish it to the Red Hat Ecosystem Catalog[cite: 1, 10].
    *   **Links:** [Red Hat Ecosystem Catalog](https://catalog.redhat.com/)[cite: 8]

## 2. Operator Certification Pipeline

*   **Stage 1: Prerequisites & Onboarding**
    *   **Details:** Ensure all containers referenced in your Operator Bundle are certified and published first[cite: 1]. Convert your operator to the File-Based Catalog (FBC) format[cite: 1].
    *   **Example FBC Configuration (`ci.yaml`):**
        ```yaml
        cert_project_id: <your component pid>
        fbc:
          enabled: true
        ```[cite: 1]

*   **Stage 2: Automated Testing (CI/CD)**
    *   **Details:** Fork the Red Hat `certified-operators` repository, add your operator bundle, and submit a GitHub Pull Request[cite: 1, 2, 4].
    *   **Validation Rules:** The PR title must match `operator <package-name> (<version>)`[cite: 1, 4]. Referenced images must be pinned to specific SHA digests instead of tags[cite: 1, 4].
    *   **Links:** [Certified Operators Repository](https://github.com/redhat-openshift-ecosystem/certified-operators)[cite: 2, 3]

*   **Stage 3: Catalog Auto-Release**
    *   **Details:** To automate the catalog release process, include a `release-config.yaml` file inside your bundle version directory[cite: 1]. The CI pipeline will build the bundle, run tests, and auto-merge the catalog updates[cite: 1].
    *   **Example Auto-Release Configuration (`release-config.yaml`):**
        ```yaml
        ---
        catalog_templates:
          - template_name: basic.yaml
            channels: [stable]
            replaces: <your-operator-name>.v1.2.2
        ```[cite: 1]

## 3. Helm Chart Certification Pipeline

*   **Stage 1: Validation & Onboarding**
    *   **Details:** Verify that all images deployed by the Helm chart are Red Hat certified[cite: 1]. Create a Helm Chart component in the Partner Connect portal[cite: 1].

*   **Stage 2: Verification Testing**
    *   **Details:** Fork the Red Hat upstream repository and run the `chart-verifier` CLI tool[cite: 1]. This tool checks chart formatting and validates required OpenShift metadata[cite: 1].

*   **Stage 3: Pull Request Submission & Publishing**
    *   **Details:** Submit the certified Helm chart, the generated chart verification report, or both via a pull request to the Red Hat OpenShift Helm chart repository[cite: 1].
    *   **Links:** Customers can download published charts from `charts.openshift.io` and the Red Hat Ecosystem Catalog[cite: 1].
