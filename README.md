# homelab-hosted-gh-runner

This repository contains infrastructure and automation to run GitHub Actions self-hosted runners on a homelab host.

## Purpose

This README explains how to install the Ansible role(s) declared in `requirements.yml` using `ansible-galaxy`. The included `requirements.yml` installs the `monolithprojects.github_actions_runner` role (version 1.26.0) from the role's repository.

## Prerequisites

- Ansible installed and available in your shell (`ansible` and `ansible-galaxy` commands).
	- On Windows it's recommended to run Ansible inside WSL (Ubuntu) or a Linux environment. If you prefer native Windows, ensure you have a working Python environment and Ansible installed via pip.
- Git (to clone/download this repo) if you haven't already.

If you need Ansible on Windows, a quick approach is to use WSL and install Ansible there. Installing Ansible on Windows natively is possible but can be more complex.

## Install the role from requirements.yml (PowerShell)

Open PowerShell (or a WSL shell) and change into this folder:

```powershell
cd .\homelab-hosted-gh-runner
```

Then run the ansible-galaxy command to install roles listed in `requirements.yml`.

Install into the default roles path:

```powershell
ansible-galaxy role install -r requirements.yml
```

Or install into a local `roles/` directory inside the current folder (recommended for repo-local dependencies):

```powershell
ansible-galaxy role install -r requirements.yml --roles-path ./roles
```

Notes:
- If the command fails because `ansible-galaxy` is not found, make sure Ansible is installed and on your PATH (or use WSL).
- Use `--force` to overwrite an existing role installation.

## Verify the installation

List installed roles:

```powershell
ansible-galaxy role list
```

Or inspect the local `roles/` directory (if you used `--roles-path ./roles`):

```powershell
Get-ChildItem .\roles\ -Directory
```

You should see the `monolithprojects.github_actions_runner` role (or a folder named for the role) installed.

## Run the playbook (local install)

To run the self-hosted runner playbook locally (on the host machine), you can run the following command from the `homelab-hosted-gh-runner` folder. This runs the playbook against localhost using the local connection and will prompt for the privilege escalation password:

```powershell
ansible-playbook ./self-hosted-runner.yml -i localhost, -c local --ask-become-pass
```

Notes:
- Run this in PowerShell or inside WSL (recommended) if Ansible is installed there.
- The trailing comma after `localhost,` is required when using an inventory inline like this.


## Notes & troubleshooting

- The included `requirements.yml` pins the role to version `1.26.0` and references the role source repository. You can edit `requirements.yml` to change versions or add more roles.
- If you are behind a corporate proxy, ensure `HTTP_PROXY`/`HTTPS_PROXY` environment variables are set in your shell.
- If you prefer to vendor roles into the repo, use the `--roles-path ./roles` option so repo contributors get consistent role versions.