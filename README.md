# Ozan Girgin — IT, Cloud & Security Portfolio

[Portfolio website](https://ozangirginwork-wq.github.io/) · [Resume](Ozan-Girgin-Resume.pdf) · [Technical review guide](REVIEW_GUIDE.md)

I am seeking IT support, systems/network support, and junior cloud operations opportunities. These seven personal labs document troubleshooting, access controls, automation, and cloud security in controlled environments.

## Choose a review path

| Role focus | Start here | What to inspect |
|---|---|---|
| IT support / Linux operations | [Linux troubleshooting](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab) | [SSH failure → diagnosis → recovery](https://github.com/ozangirginwork-wq/linux-it-support-troubleshooting-lab/blob/main/tickets/TICKET-003-ssh-service-recovery.md), with client-side verification |
| Windows / systems support | [Active Directory](https://github.com/ozangirginwork-wq/windows-server-active-directory-lab) | DNS, domain join, GPO verification, and an [allowed/denied file-access test](https://github.com/ozangirginwork-wq/windows-server-active-directory-lab#positive-and-negative-authorization-test) |
| Cloud operations / automation | [Python diagnostics](https://github.com/ozangirginwork-wq/python-it-cloud-automation-lab) and [Terraform CI](https://github.com/ozangirginwork-wq/terraform-cicd-pipeline) | Diagnostic reports, failure handling, and infrastructure validation without deployment |
| Cloud security / incident response | [AWS automated response](https://github.com/ozangirginwork-wq/aws-security-automated-incident-response) | Scoped remediation, independent state verification, tests, and historical evidence |
| Containers / infrastructure | [Kubernetes troubleshooting](https://github.com/ozangirginwork-wq/secure-kubernetes-deployment-lab) | Hardened rollout recovery, NetworkPolicy allow/deny tests, and disposable-cluster CI |

The [technical review guide](REVIEW_GUIDE.md) links directly to code, tests, reproduction instructions, and limitations. [Manual AWS incident investigation](https://github.com/ozangirginwork-wq/aws-security-incident-response-lab) complements the automated-response project.

## Evidence and scope

These are learning projects, not client engagements or production employment. Each lab separates its implementation from the evidence available for it. Historical screenshots do not establish that today's code is deployed; mocked tests do not establish live cloud behavior. AWS lab resources were cleaned up after the exercises.

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

