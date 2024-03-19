# Chaos Custom Helm Charts

Repository for Chaos Custom Helm Charts.

![Helm Logo](/.attachments/helm-logo.svg)

## Steps to test charts manually

* Lint Helm charts to ensure syntax integrity.

```bash
helm lint charts/<chart-name>
```

* Test chart works by generating a template

```bash
helm template charts/<chart-name>
```

* **OPTIONAL** Test chart deploys by running against a k8s cluster

```bash
helm upgrade --install <release-name> charts/<chart-name> --dry-run --debug
```

* Package new chart version
  * The **--d** flag points the output .tgz file to target dir

```bash
helm package charts/<target-chart>/ -d charts/<target-chart>/packages
```

* Tag new commit **with** chart **version**

```bash
git add .
```

```bash
git commit -m "some message"
```

```bash
git tag <chart-version>
```

```bash
git push
```

```bash
git push origin v0.0.0
```

* **Optionally for multiple tags in local repository**

```bash
git push origin --tags
```

## Securely packaging and signing charts

* **GPG Keys are needed ahead of time and imported in .gpg fmt within ~/.gnupg**

* Package and sign

```bash
helm package --sign --key mykey-gpg --keyring ~/.gnupg/secring.gpg charts/<chart-name>/ -d charts/<chart-name>/packages/
```

* Verify package signature

```bash
helm verify charts/<chart-name>/packages/<chart-name>-0.0.0.tgz --keyring ~/.gnupg/secring.gpg
```

* Index signed chart

```bash
helm repo index charts/<chart-name>/packages/
```

* Install and verify chart

```bash
helm upgrade --install --verify --keyring ~/.gnupg/secring.gpg <release-name> chaos-charts/<chart-name>
```

* Uninstall verified chart

```bash
helm uninstall <release-name>
```

* References for chart signing:
  * [Helm Docs](https://helm.sh/docs/topics/provenance/)
  * [GnuPG](https://gnupg.org/download/)

## Resources

* [Homelab Charts](https://helm.kmartinez.net)
* [Helm Getting Started](https://helm.sh/docs/chart_template_guide/getting_started/)
* [Official Helm Chart Repo Guide](https://v2.helm.sh/docs/developing_charts/#the-chart-repository-guide)
* [Git Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
* [Helm Plugins](https://github.com/topics/helm-plugins)

## Reference

* **Repository Commands**

```bash
helm repo list 
```

```bash
helm repo update 
```

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

```bash
helm repo remove bitnami
```

* **Search Commands**

```bash
helm search repo bitnami
```

```bash
helm search repo mysql
```

```bash
helm search repo database
```

```bash
helm search repo database --versions
```

* **Installation Commands**

```bash
helm install mydb bitnami/mysql 
```

* **With Values**

```bash
helm install mydb bitnami/mysql --values somepath/values.yaml
```

* **Install and reuse values**

```bash
helm install mydb bitnami/mysql --reuse-values
```

```bash
helm upgrade --install mydb bitnami/mysql 
```

```bash
helm upgrade mydb bitnami/mysql 
```

```bash
helm uninstall mydb
```

* **Optionally to keep history**

```bash
helm uninstall mydb --keep-history
```

* **Rollback Commands**

```bash
helm rollback mydb bitnami/mysql 
```

* **Status Commands**

```bash
helm status <some-release-name>
```

* **Template Commands**

```bash
helm template mydb bitnami/mysql
```

* **Helm Get Commands**

```bash
helm get all metallb-metallb -n flux-system
```

```bash
helm get values metallb-metallb -n flux-system
```

```bash
helm get manifest metallb-metallb -n flux-system
```

```bash
helm get manifest metallb-metallb -n flux-system --revision 2
```

* **Helm History Commands**

```bash
helm history metallb-metallb -n flux-system
```

* **Helm Rollback Commands**

```bash
helm rollback mywebserver 1
```

* **Helm Package Commands**

```bash
helm package charts/jellyfin-nfs
```

* **Helm dependency Commands**
  * Updates chart dependencies and downloads the list into charts dir next to templates dir. **Necessary steps when adding chart dependecies**

```bash
helm dependency update charts/getting-started/

```

## Dir Structure

```text
.
├── README.md
└── charts
    ├── cert-manager-issuers
    │   ├── Chart.yaml
    │   ├── README.md
    │   ├── templates
    │   │   ├── NOTES.txt
    │   │   ├── _helpers.tpl
    │   │   └── cluster-issuers.yaml
    │   └── values.yaml
    └── karpenter-nodepools
        ├── Chart.yaml
        ├── README.md
        ├── templates
        │   ├── _helpers.tpl
        │   ├── ec2nodeclass.yaml
        │   └── nodepool.yaml
        └── values.yaml

6 directories, 13 files

```
