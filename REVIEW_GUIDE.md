# Technical review guide

[Portfolio overview](README.md) · [Website](https://ozangirginwork-wq.github.io/)

Use the links below to inspect a relevant project without reading every repository. These are personal lab exercises; the evidence boundaries are part of each project's scope.

## Support and systems

### 1. Linux service recovery

- **Problem:** SSH access from the Windows host stopped working.
- **Investigation and outcome:** the [ticket](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/tickets/TICKET-003-ssh-service-recovery.md) documents inactive service/socket units, missing runtime directory, configuration validation, restoration, and a successful connection from the original client.
- **Review:** [diagnosis and recovery evidence](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/evidence/README.md), [disk-monitoring script](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/scripts/check-disk.sh).
- **Boundary:** VirtualBox NAT and a single Ubuntu VM; simulated support incidents rather than customer cases. No GitHub Actions workflow is present.

### 2. Windows identity and file access

- **Problem:** establish domain authentication, workstation policy, and departmental file permissions in an isolated network.
- **Review:** [network troubleshooting](https://github.com/ozangirginwork-wq/windows-server-active-directory-lab/blob/main/docs/troubleshooting.md), [share setup script](https://github.com/ozangirginwork-wq/windows-server-active-directory-lab/blob/main/scripts/New-DepartmentShares.ps1), [allowed IT / denied HR evidence](https://github.com/ozangirginwork-wq/windows-server-active-directory-lab/blob/main/screenshots/10-allowed-denied-access-test.jpg).
- **Boundary:** the demonstrated access test covers one user against two shares, not every permission combination. The README distinguishes original live evidence from later script changes inspected statically. No GitHub Actions workflow is present.

### 3. Python diagnostics

- **Problem:** collect Windows resource, connectivity, service, and event-log checks into readable reports.
- **Review:** [sample report](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab/blob/main/reports/sample-report.json), [event-log implementation](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab/blob/main/src/event_log_checks.py), [failure-path tests](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab/blob/main/tests/test_event_log_checks.py), [Windows CI](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab/actions).
- **Boundary:** TCP/443 reachability does not validate TLS or HTTP. Mocked event-log tests do not prove access to a real machine's logs.

## Cloud and infrastructure

### 4. AWS investigation and offline detection

- **Problem:** investigate public administrative access and excessive IAM privilege, then correlate remediation events.
- **Review:** [incident report](https://github.com/ozangirginwork-wq/aws-security-incident-response-lab/blob/main/docs/incidents/INCIDENT-001-public-ssh.md), [detector](https://github.com/ozangirginwork-wq/aws-security-incident-response-lab/blob/main/scripts/cloudtrail_detector.py), [regression tests](https://github.com/ozangirginwork-wq/aws-security-incident-response-lab/blob/main/tests/test_cloudtrail_detector.py).
- **Boundary:** published events are sanitized reconstructions, and the portfolio images are reconstructed from that dataset. The detector's `RESOLVED` state means a matching remediation event was found; it does not query current AWS state. The optional Terraform baseline was not deployed in the live investigation.

### 5. Terraform security checks

- **Problem:** validate an AWS network/storage configuration and review scanner findings before deployment.
- **Review:** [workflow](https://github.com/ozangirginwork-wq/terraform-cicd-pipeline/blob/main/.github/workflows/terraform-ci.yml), [storage controls](https://github.com/ozangirginwork-wq/terraform-cicd-pipeline/blob/main/storage.tf), [accepted exceptions](https://github.com/ozangirginwork-wq/terraform-cicd-pipeline#accepted-scanner-exceptions).
- **Boundary:** CI runs formatting, validation, and security scanning, not `terraform apply`. A passing workflow includes documented resource-specific exceptions; it is not proof of a running production environment.

### 6. Automated AWS response

- **Problem:** remove public SSH exposure from a designated lab security group and verify the resulting state.
- **Review:** [response code](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/blob/main/lambda/remediation.py), [state verifier](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/blob/main/lambda/verifier.py), [safety and failure-path tests](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/blob/main/tests/test_response_pipeline.py), [historical evidence](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/blob/main/evidence/README.md).
- **Design decisions:** restrict the target with IAM and application checks; gate live changes; verify state after the revoke operation.
- **Boundary:** revoking a broad matching rule can remove access beyond SSH. The trigger focuses on `AuthorizeSecurityGroupIngress`; pre-existing exposure needs additional detection. Historical live evidence covers the original IPv4 TCP/22 case; later hardening is tested in code. The AWS environment was torn down.
- **Operational learning:** [timeout, runtime compatibility, and cleanup problems](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response#troubleshooting--lessons-learned).

### 7. Kubernetes deployment and recovery

- **Problem:** run nginx with reduced privileges and demonstrate access controls and workload recovery.
- **Review:** [deployment manifest](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab/blob/main/manifests/deployment.yaml), [runtime assertions](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab/blob/main/scripts/test_cluster.sh), [troubleshooting](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab/blob/main/docs/troubleshooting.md), [security review](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab/blob/main/docs/security-review.md).
- **Design decisions:** provide bounded writable temporary storage while keeping the root filesystem read-only; disable automatic API-token mounting; test allowed → blocked → allowed traffic using the same client.
- **Boundary:** three kind nodes share one physical host. Pod replacement does not prove host-level high availability. A green Checkov gate retains the documented UID exception.

## Reproduction without an AWS deployment

Run commands from each cloned repository's root and use an isolated Python environment. Follow the linked project instructions for prerequisites.

| Project | Local review command | What it establishes |
|---|---|---|
| Python diagnostics | `python -m pip install -r requirements.txt`, then `python -m pytest tests -q` | Regression behavior; [Windows setup and usage](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab#installation) |
| AWS offline detector | `python -m unittest discover -s tests -v` | Detector behavior against supplied fixtures |
| AWS automated response | `python -m pip install -r requirements-dev.txt`, then `python -m pytest tests -q` | Detection/response behavior with test doubles; no live deployment |
| Terraform | `terraform init -backend=false -input=false`, `terraform fmt -check -recursive`, `terraform validate`, `checkov -d . --framework terraform` | Static checks; requires Terraform and Checkov plus provider downloads |

For a runtime container exercise, follow the [Kubernetes reproduction guide](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab/blob/main/docs/reproduce.md). It creates a disposable local cluster and consumes local CPU/RAM; it is separate from the offline checks above.

## CI evidence at review time

Reviewed September 23, 2026. These are existing GitHub Actions results, not new runs performed for this review. Each run below matches the repository's main-branch head observed during the review. Follow the project's Actions page for subsequent results.

| Project | Observed run | Interpretation |
|---|---|---|
| Python diagnostics | [Success](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab/actions/runs/34788097894) | Windows regression suite |
| AWS offline detector | [Success](https://github.com/ozangirginwork-wq/aws-security-incident-response-lab/actions/runs/34788098601) | Repository CI checks |
| Terraform | [Success](https://github.com/ozangirginwork-wq/terraform-cicd-pipeline/actions/runs/34788098705) | Static validation and configured security gate |
| AWS automated response | [Success](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/actions/runs/34788099303) | Python tests and Terraform checks |
| Kubernetes | [Success](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab/actions/runs/34788099714) | Both static validation and disposable-cluster runtime jobs passed |

CI is supporting evidence within each workflow's scope, not a complete security audit or a measure of independent interview readiness.
