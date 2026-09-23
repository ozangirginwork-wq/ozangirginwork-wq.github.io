# Ozan Girgin — IT, Cloud & Security Portfolio

[Portfolio website](https://ozangirginwork-wq.github.io/) · [Resume](Ozan-Girgin-Resume.pdf)

## Start here: two projects to review

These personal lab projects demonstrate troubleshooting and cloud security workflows in controlled environments. They are educational exercises, not client engagements or production employment.

| Area | Project | Suggested review path |
|---|---|---|
| IT support and Linux operations | [Linux IT Support & Troubleshooting](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab) | Read the [SSH recovery ticket](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/tickets/TICKET-003-ssh-service-recovery.md), then compare the diagnosis and restored-service screenshots in the [evidence index](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/evidence/README.md). |
| AWS security automation | [AWS Detection & Automated Incident Response](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response) | Review the [design and safety controls](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response#automated-remediation), [historical evidence](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/blob/main/evidence/README.md), and [production considerations](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response#scope--production-considerations). |

### Linux support: diagnose, restore, verify

Five documented scenarios cover account access, SSH configuration and recovery, file permissions, and disk capacity. Tickets describe symptoms, investigation, resolution, and verification. A [Bash disk monitor](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/scripts/check-disk.sh) adds configurable thresholds, logging, and exit codes.

The environment is an Ubuntu VM using VirtualBox NAT. The incidents are isolated lab scenarios; the tickets are technical documentation rather than records of customer support work.

### AWS response: contain exposure and check the resulting state

CloudTrail records a security-group change, EventBridge routes the event, and a Python Lambda responder removes public SSH exposure from a designated lab security group. A separate AWS state query verifies the result.

Key decisions documented in the repository:

- **Bounded remediation:** IAM and application checks restrict changes to the protected security group.
- **Explicit safety gates:** dry-run behavior and a live-remediation setting control whether changes occur.
- **Independent verification:** API success alone is insufficient; the responder checks whether exposure remains.
- **Containment tradeoff:** revoking a matching port-range or all-protocol rule can remove access beyond SSH.
- **Known detection gap:** the event trigger focuses on one ingress API; pre-existing exposure needs additional detection or reconciliation.

The repository includes [implementation lessons](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response#troubleshooting--lessons-learned) covering Lambda timeout, CI runtime compatibility, and CloudTrail bucket cleanup.

**Evidence boundary:** the historical live exercise covers the original IPv4 TCP/22 scenario. Later hardening is covered by automated tests, not a new live deployment. The AWS infrastructure was torn down. See the [tests](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/tree/main/tests), [workflow history](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/actions), and [commit history](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response/commits/main/) for review.

## Other lab projects

- [Windows Server & Active Directory](https://github.com/ozangirginwork-wq/windows-server-active-directory-lab)
- [Python IT & Cloud Automation](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab)
- [AWS Security Incident Response](https://github.com/ozangirginwork-wq/aws-security-incident-response-lab)
- [Terraform & CI/CD](https://github.com/ozangirginwork-wq/terraform-cicd-pipeline)
- [Secure Kubernetes Deployment & Troubleshooting](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab)

## Website maintenance

Static HTML and CSS, with no build dependencies, backend, paid APIs, or AWS services.
This dedicated repository is separate from the seven technical labs linked on the site.

## Publishing
In Settings → Pages, choose Deploy from a branch, main, /(root), then Save.
GitHub Pages supports public repositories on GitHub Free; no custom domain is required.

## Updating
- Edit index.html to update text and project links.
- Edit style.css to change appearance.
- Replace Ozan-Girgin-Resume.pdf to update the resume, preserving its filename.
- The website uses the owner-selected portrait, compressed to WebP for faster loading.
- Commit changes to this repository only. GitHub Pages republishes the selected branch.

No photo has been added to the lab repositories or resume.
