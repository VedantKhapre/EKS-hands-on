# EKS Foundations with Terraform

## Provided files

- `.devcontainer/`: an opinionated devcontainer that installs Terraform, the AWS CLI, and kubectl at pinned versions, with shell completion and a zsh setup.
- `.tool-versions`: the same pinned versions for a local install with mise or asdf.
- `.gitattributes`: enforces LF line endings so the project behaves the same on every operating system.
- `.gitignore`: keeps Terraform state, plans, `.tfvars`, and local kubeconfigs out of version control.
- `infra/`: the starter Terraform project (`versions.tf`, `providers.tf`, `backend.tf`), with state configured for an S3 backend.
- `k8s/`: ready-made Kubernetes manifests (an nginx Deployment and a LoadBalancer Service).
- `_labs/`: the written lab for each section, each with its goal, the configuration to target, and the tasks.

## Set up the toolchain

Pinned versions: Terraform `1.15.8`, AWS CLI `2.36.4`, kubectl `1.36.2`. Pick one path.

- **Devcontainer (recommended).** Open the folder in an editor that supports the Dev Containers spec and reopen it in the container, or open it as a GitHub Codespace. It needs a Docker-compatible engine: Docker Desktop (on Windows, use the WSL2 backend), or Colima or Rancher Desktop in its dockerd (moby) mode on macOS or Linux. The container builds with the tools already installed.
- **Local install.** Install the pinned versions with mise or asdf from `.tool-versions` (`mise install`), or install each tool by hand at those versions.

Confirm the versions either way:

```bash
terraform version            # Terraform v1.15.8
aws --version                # aws-cli/2.36.4
kubectl version --client     # Client Version: v1.36.2
```

## Configure AWS access

Configure your credentials with an access key pair and confirm which identity you are authenticated as. The devcontainer presets the region to `us-east-1`; to work in a different region, change `AWS_REGION` in `.devcontainer/devcontainer.json`.

```bash
aws configure
aws sts get-caller-identity
```

## Initialize Terraform

Terraform state lives in an S3 bucket you own. The backend is a partial configuration, so you pass your bucket at init time; the region is read from `AWS_REGION`. If you do not already have a bucket for state, create one first.

```bash
cd infra
terraform init -backend-config="bucket=YOUR_STATE_BUCKET"
```

Expect `Successfully configured the backend "s3"!` and `Terraform has been successfully initialized!`. From here the `infra/` project is ready to build on, starting with the first lab in `_labs/`.

## Labs

The labs in `_labs/` follow the order of the course. Each one describes the configuration to target rather than handing you the code, so you write the Terraform yourself and check it against the [course code repository](https://github.com/lm-academy/eks-foundations-solution) afterwards.

The cluster, the NAT gateway, the nodes, and the load balancer are all billed resources. Tear everything down at the end.