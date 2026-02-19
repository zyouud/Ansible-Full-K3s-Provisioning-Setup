# Ansible K3s Provisioning + Tools Manifests Generator

## What this does
- Installs K3s on the server (Traefik default)
- Installs cert-manager
- Creates namespaces: tools, monitoring, vault, frontend, backend
- Writes Kubernetes YAML manifests into: /root/tools/<service>/
  - postgres (1 instance)
  - redis (1 instance)
  - kafka (stack)
  - mongodb
  - vault (+ vault-fetch + root token secret yaml)
- Applies ONLY Vault (Option A) so Ansible can:
  - vault operator init
  - vault operator unseal
  - store vault init output locally to: ./vault-keys.json

## Edit-first workflow
- Tools stack YAMLs are generated but NOT applied.
- Vault is applied automatically (required for auto init/unseal).

## Configure
Edit:
- inventory/hosts.ini (add server IPs)
- group_vars/all.yml (true/false flags + passwords/ports)

## Run
From inside this folder:
ansible-playbook -i inventory/hosts.ini playbooks/site.yml

## Output locations
On server:
  /root/tools/...
On your laptop:
  ./vault-keys.json
