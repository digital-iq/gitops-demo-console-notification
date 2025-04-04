# Infra GitOps: Console Notification Helm Chart and ApplicationSet

## 📖 Description

This repository provides a GitOps-driven mechanism for deploying `ConsoleNotification` custom resources across OpenShift clusters using Helm and ArgoCD ApplicationSets. It supports both Non-Production and Production environments through environment-specific Helm values. CI/CD integration is implemented via GitHub Actions.

---

## 📁 Repo Structure

```
.
├── applicationset/
│   ├── values.yaml       # Helm values for Openshift clusters under Argocd
│
├── helm-console-notification/
│   ├── Chart.yaml                # Helm Chart metadata
│   └── templates/
│       └── ConsoleNotification.yaml  # ConsoleNotification CR manifest
│
├── .github/
│   └── workflows/
│       └── push-cd.yaml          # GitHub Action for CD pipeline
│
└── README.md                     # Documentation and usage guide
```

---

## 🎯 Purpose of Helm Chart

The `helm-console-notification` chart manages the deployment of OpenShift `ConsoleNotification` resources that display banner messages in the web console. These notifications help communicate environment-specific or system-critical information to platform users.

---

## 🚀 How to Apply Manually

You can render and apply the Helm chart manually using the following steps:

```bash
# Clone the repository
git clone https://github.com/digital-iq/gitops-demo-console-notification
cd gitops-demo-console-notification

# Lint
helm lint helm-*/

# Clone applicationset git repo
git clone https://github.com/digital-iq/gitops-demo-applicationset-templates
cd gitops-demo-applicationset-templates

# Render
helm template main-applicationset ./main-applicationset -f ../applicationset/values.yaml > rendered.yaml

# Apply to cluster
oc apply -f rendered.yaml
```
---

## 👀 How to See the Result

1. Log in to the OpenShift web console.
2. You should see a colored banner at the top or bottom (depending on your Helm values).
3. Use `oc get consolnotifications` to validate the CR has been applied:
   ```bash
   oc get consolnotifications
   ```

---

## 🔄 GitHub Workflow Integration

The `.github/workflows/push-cd.yaml` pipeline automates the rendering and application of Helm charts in the GitOps cluster. It can also optionally handle the creation of secrets required by the chart.

Key Features:
- Reusable pipeline integration
- Cluster targeting via environment
- GitOps ApplicationSet updates

---

## Helm Chart Variables

The following table outlines the configurable parameters used in the `ConsoleNotification` Helm chart and their default values:

| Variable         | Description                                 | Default Value       |
|------------------|---------------------------------------------|---------------------|
| `backgroundColor`| Background color of the banner              | `"#000000"`         |
| `color`          | Text color of the banner                    | `"#FFFFFF"`         |
| `text`           | The message text shown in the banner        | `"Environment Notice"` |

These values can be overridden via the appropriate `values-nonprod.yaml` or `values-prod.yaml` file.

---

## ⚠️ Limitations

- Assumes ArgoCD and ApplicationSet controller are already deployed in the GitOps cluster.
- Only supports OpenShift environments with access to the GitOps instance.
- No native rollback or diff detection — relies on ArgoCD for state sync.

---

## 🛠️ TODO

None

---

## 📦 Prerequisites

- OpenShift 4.x with access to the OpenShift Console
- ArgoCD with ApplicationSet controller installed
- GitHub Actions configured with access to the GitOps cluster
- Helm 3.0+ installed locally for manual rendering

---

## 🧪 Testing Locally

You can validate the rendered manifest before applying:

```bash
# Clone the repository
git clone https://github.com/digital-iq/gitops-demo-console-notification/
cd gitops-demo-console-notification

# Lint
helm lint helm-*/

# Extract values from applicationset/values.yaml to local file values.yaml for particular cluster

# Render
helm template helm-console-notification \
  -f values.yaml \
  > rendered.yaml

# Apply to cluster
oc apply -f rendered.yaml
```

---

## ✍️ Contribution Guidelines

Contributions, issues, and feature requests are welcome. Please fork the repository and submit a pull request following our branch naming conventions and review process.

---

## 📄 License

This repository is licensed under the [MIT License](LICENSE), unless otherwise noted.

---

## 👥 Maintainers

- Platform Team, Li9 DevOps Group
- Contacts: alexandr.vishniakov@li9.com, oleg.klimin@li9.com, artemii.kropachev@li9.com
