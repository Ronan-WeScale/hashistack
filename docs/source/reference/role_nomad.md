
```{include} ../../../roles/nomad/README.md
```

## Defaults

```
nomad_datacenter_name: "{{ hs_workspace }}"
nomad_version: "1.4.3-1"

nomad_consul_address: "{{ hs_nomad_node_fqdn }}:8501"
nomad_consul_grpc_address: "{{ hs_nomad_node_fqdn }}:8503"
nomad_api_port: "4646"
nomad_address: "https://{{ hs_nomad_node_fqdn }}:{{ nomad_api_port }}"

hs_nomad_domain: "{{ public_domain }}"
hs_nomad_node_name: "{{ inventory_hostname | regex_replace('_', '-') }}"
hs_nomad_node_fqdn: "{{ hs_nomad_node_name }}.{{ hs_nomad_domain }}"

nomad_vault_address: "https://vault.{{ hs_nomad_domain }}:8200"
nomad_vault_token: ~

nomad_advertise_addr: "{{ hs_nomad_node_fqdn }}"

nomad_inventory_masters_group: "hashistack_masters"
nomad_inventory_minions_group: "hashistack_minions"
nomad_local_secrets_dir: "{{ hs_workspace_secrets_dir }}"

nomad_use_custom_ca: false
nomad_local_ca_cert: "{{ hs_workspace_secrets_dir }}/ca.cert.pem"

nomad_tls_ca_cert: "/etc/ssl/certs/ca-certificates.crt"
```

Container image used by consul connect to build sidecars and gateways when
instructed by nomad.

```
nomad_connect_image: "envoyproxy/envoy:v1.24.2"
```

ansible controller path where to store the root token of nomad bottstrap.

```
nomad_local_secret_file: "{{ hs_workspace_secrets_dir }}/root_nomad.yml"

nomad_ca_certificate_dir: "/usr/local/share/ca-certificates"
nomad_ca_certificate: "/etc/ssl/certs/ca-certificates.crt"
nomad_node_cert: "{{ nomad_local_secrets_dir }}/self.cert.pem"
nomad_node_cert_private_key: "{{ nomad_local_secrets_dir }}/self.cert.key"
nomad_node_cert_fullchain: "{{ nomad_local_secrets_dir }}/self.fullchain.cert.pem"
nomad_bootstrap_expect: "{{ groups[nomad_inventory_masters_group] | length }}"
```

## Volumes

Expected list objects format:
- name: ""
  path: ""
  read_only: [true|false]

```
nomad_volumes: []
nomad_sysctl:
  net.bridge.bridge-nf-call-arptables: "1"
  net.bridge.bridge-nf-call-ip6tables: "1"
  net.bridge.bridge-nf-call-iptables: "1"
